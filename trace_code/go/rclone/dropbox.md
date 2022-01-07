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
