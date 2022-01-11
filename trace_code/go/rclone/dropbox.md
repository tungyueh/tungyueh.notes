# rclone - package dropbox
## dropbox.go
``` go
// Fs represents a remote dropbox server
type Fs struct {
	name           string         // name of this remote
	root           string         // the path we are working on
	opt            Options        // parsed options
	ci             *fs.ConfigInfo // global config
	features       *fs.Features   // optional features
	srv            files.Client   // the connection to the dropbox server
	svc            files.Client   // the connection to the dropbox server (unauthorized)
	sharing        sharing.Client // as above, but for generating sharing links
	users          users.Client   // as above, but for accessing user information
	team           team.Client    // for the Teams API
	slashRoot      string         // root with "/" prefix, lowercase
	slashRootSlash string         // root with "/" prefix and postfix, lowercase
	pacer          *fs.Pacer      // To pace the API calls
	ns             string         // The namespace we are using or "" for none
	batcher        *batcher       // batch builder
}
```
``` go
// getMetadata gets the metadata for a file or directory
func (f *Fs) getMetadata(ctx context.Context, objPath string) (entry files.IsMetadata, notFound bool, err error) {
	err = f.pacer.Call(func() (bool, error) {
		entry, err = f.srv.GetMetadata(&files.GetMetadataArg{
			Path: f.opt.Enc.FromStandardPath(objPath),
		})
		return shouldRetry(ctx, err)
	})
	if err != nil {
		switch e := err.(type) {
		case files.GetMetadataAPIError:
			if e.EndpointError != nil && e.EndpointError.Path != nil && e.EndpointError.Path.Tag == files.LookupErrorNotFound {
				notFound = true
				err = nil
			}
		}
	}
	return
}
```
* `f.pacer.Call` could retry calls with a configurable delay in between
* `shouldRetry(ctx, err)` decide to retry or not
* `switch e := err.(type)` use type switch to return notFound if GetMetadataAPIError
``` go
// getFileMetadata gets the metadata for a file
func (f *Fs) getFileMetadata(ctx context.Context, filePath string) (fileInfo *files.FileMetadata, err error) {
	entry, notFound, err := f.getMetadata(ctx, filePath)
	if err != nil {
		return nil, err
	}
	if notFound {
		return nil, fs.ErrorObjectNotFound
	}
	fileInfo, ok := entry.(*files.FileMetadata)
	if !ok {
		if _, ok = entry.(*files.FolderMetadata); ok {
			return nil, fs.ErrorIsDir
		}
		return nil, fs.ErrorNotAFile
	}
	return fileInfo, nil
}
```
* `fileInfo, ok := entry.(*files.FileMetadata)` use type assertion to check and convert type
``` go
// Return an Object from a path
//
// If it can't be found it returns the error fs.ErrorObjectNotFound.
func (f *Fs) newObjectWithInfo(ctx context.Context, remote string, info *files.FileMetadata) (fs.Object, error) {
	o := &Object{
		fs:     f,
		remote: remote,
	}
	var err error
	if info != nil {
		err = o.setMetadataFromEntry(info)
	} else {
		err = o.readEntryAndSetMetadata(ctx)
	}
	if err != nil {
		return nil, err
	}
	return o, nil
}
```
* return the pointer to Object init with info or ctx
``` go
// NewObject finds the Object at remote.  If it can't be found
// it returns the error fs.ErrorObjectNotFound.
func (f *Fs) NewObject(ctx context.Context, remote string) (fs.Object, error) {
	if f.opt.SharedFiles {
		return f.findSharedFile(ctx, remote)
	}
	return f.newObjectWithInfo(ctx, remote, nil)
}
```
* Get object with nil info
``` go
// listSharedFoldersApi lists all available shared folders mounted and not mounted
// we'll need the id later so we have to return them in original format
func (f *Fs) listSharedFolders(ctx context.Context) (entries fs.DirEntries, err error) {
	started := false
	var res *sharing.ListFoldersResult
	for {
		if !started {
			arg := sharing.ListFoldersArgs{
				Limit: 100,
			}
			err := f.pacer.Call(func() (bool, error) {
				res, err = f.sharing.ListFolders(&arg)
				return shouldRetry(ctx, err)
			})
			if err != nil {
				return nil, err
			}
			started = true
		} else {
			arg := sharing.ListFoldersContinueArg{
				Cursor: res.Cursor,
			}
			err := f.pacer.Call(func() (bool, error) {
				res, err = f.sharing.ListFoldersContinue(&arg)
				return shouldRetry(ctx, err)
			})
			if err != nil {
				return nil, fmt.Errorf("list continue: %w", err)
			}
		}
		for _, entry := range res.Entries {
			leaf := f.opt.Enc.ToStandardName(entry.Name)
			d := fs.NewDir(leaf, time.Now()).SetID(entry.SharedFolderId)
			entries = append(entries, d)
			if err != nil {
				return nil, err
			}
		}
		if res.Cursor == "" {
			break
		}
	}

	return entries, nil
}
```
* Use `started` as a flag to call first API
* `entries` is named return value to store result
* `res` contains entries result and cursor
* Break loop if cursor is empty
``` go
// findSharedFolder find the id for a given shared folder name
// somewhat annoyingly there is no endpoint to query a shared folder by it's name
// so our only option is to iterate over all shared folders
func (f *Fs) findSharedFolder(ctx context.Context, name string) (id string, err error) {
	entries, err := f.listSharedFolders(ctx)
	if err != nil {
		return "", err
	}
	for _, entry := range entries {
		if entry.(*fs.Dir).Remote() == name {
			return entry.(*fs.Dir).ID(), nil
		}
	}
	return "", fs.ErrorDirNotFound
}
```
* `entry.(*fs.Dir).Remote()` check `entry` is type of `fs.Dir` then call Remote()
