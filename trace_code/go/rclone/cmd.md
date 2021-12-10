# rclone - package cmd
## cmd.go
``` go
// Main runs rclone interpreting flags and commands out of os.Args
func Main() {
	if err := random.Seed(); err != nil {
		log.Fatalf("Fatal error: %v", err)
	}
	setupRootCommand(Root)
	AddBackendFlags()
	if err := Root.Execute(); err != nil {
		if strings.HasPrefix(err.Error(), "unknown command") && selfupdateEnabled {
			Root.PrintErrf("You could use '%s selfupdate' to get latest features.\n\n", Root.CommandPath())
		}
		log.Fatalf("Fatal error: %v", err)
	}
}
```
* `setupRootCommand(Root)` setupRootCommand sets default usage, help, and error handling for the root command.
* `AddBackendFlags()` creates flags for all the backend options
* `Root.Execute()` execute command
``` go
var backendFlags map[string]struct{}

// AddBackendFlags creates flags for all the backend options
func AddBackendFlags() {
	backendFlags = map[string]struct{}{}
	for _, fsInfo := range fs.Registry {
		done := map[string]struct{}{}
		for i := range fsInfo.Options {
			opt := &fsInfo.Options[i]
			// Skip if done already (e.g. with Provider options)
			if _, doneAlready := done[opt.Name]; doneAlready {
				continue
			}
			done[opt.Name] = struct{}{}
			// Make a flag from each option
			name := opt.FlagName(fsInfo.Prefix)
			found := pflag.CommandLine.Lookup(name) != nil
			if !found {
				// Take first line of help only
				help := strings.TrimSpace(opt.Help)
				if nl := strings.IndexRune(help, '\n'); nl >= 0 {
					help = help[:nl]
				}
				help = strings.TrimRight(strings.TrimSpace(help), ".!?")
				if opt.IsPassword {
					help += " (obscured)"
				}
				flag := pflag.CommandLine.VarPF(opt, name, opt.ShortOpt, help)
				flags.SetDefaultFromEnv(pflag.CommandLine, name)
				if _, isBool := opt.Default.(bool); isBool {
					flag.NoOptDefVal = "true"
				}
				// Hide on the command line if requested
				if opt.Hide&fs.OptionHideCommandLine != 0 {
					flag.Hidden = true
				}
				backendFlags[name] = struct{}{}
			} else {
				fs.Errorf(nil, "Not adding duplicate flag --%s", name)
			}
			//flag.Hidden = true
		}
	}
}
```
* `var backendFlags map[string]struct{}` define backendFlags is map with string key and struct{} value
* `backendFlags = map[string]struct{}{}` init backendFlags with empty map
* `for _, fsInfo := range fs.Registry` range each filesystem information
* `done := map[string]struct{}{}` done keep track of already processed options and use map with empty struct as `Set` to save memory
``` go
// initConfig is run by cobra after initialising the flags
func initConfig() {
	ctx := context.Background()
	ci := fs.GetConfig(ctx)

	// Start the logger
	fslog.InitLogging()

	// Finish parsing any command line flags
	configflags.SetFlags(ci)

	// Load the config
	configfile.Install()

	// Start accounting
	accounting.Start(ctx)

	// Hide console window
	if ci.NoConsole {
		terminal.HideConsole()
	}

	// Load filters
	err := filterflags.Reload(ctx)
	if err != nil {
		log.Fatalf("Failed to load filters: %v", err)
	}

	// Write the args for debug purposes
	fs.Debugf("rclone", "Version %q starting with parameters %q", fs.Version, os.Args)

	// Inform user about systemd log support now that we have a logger
	if fslog.Opt.LogSystemdSupport {
		fs.Debugf("rclone", "systemd logging support activated")
	}

	// Start the remote control server if configured
	_, err = rcserver.Start(context.Background(), &rcflags.Opt)
	if err != nil {
		log.Fatalf("Failed to start remote control: %v", err)
	}

	// Setup CPU profiling if desired
	if *cpuProfile != "" {
		fs.Infof(nil, "Creating CPU profile %q\n", *cpuProfile)
		f, err := os.Create(*cpuProfile)
		if err != nil {
			err = fs.CountError(err)
			log.Fatal(err)
		}
		err = pprof.StartCPUProfile(f)
		if err != nil {
			err = fs.CountError(err)
			log.Fatal(err)
		}
		atexit.Register(func() {
			pprof.StopCPUProfile()
		})
	}

	// Setup memory profiling if desired
	if *memProfile != "" {
		atexit.Register(func() {
			fs.Infof(nil, "Saving Memory profile %q\n", *memProfile)
			f, err := os.Create(*memProfile)
			if err != nil {
				err = fs.CountError(err)
				log.Fatal(err)
			}
			err = pprof.WriteHeapProfile(f)
			if err != nil {
				err = fs.CountError(err)
				log.Fatal(err)
			}
			err = f.Close()
			if err != nil {
				err = fs.CountError(err)
				log.Fatal(err)
			}
		})
	}

	if m, _ := regexp.MatchString("^(bits|bytes)$", *dataRateUnit); m == false {
		fs.Errorf(nil, "Invalid unit passed to --stats-unit. Defaulting to bytes.")
		ci.DataRateUnit = "bytes"
	} else {
		ci.DataRateUnit = *dataRateUnit
	}
}
```
* `ctx := context.Background()` init the root context
## all.go
* import all cmd package to init the commands
## about.go
``` go
func init() {
	cmd.Root.AddCommand(commandDefinition)
	cmdFlags := commandDefinition.Flags()
	flags.BoolVarP(cmdFlags, &jsonOutput, "json", "", false, "Format output as JSON")
	flags.BoolVarP(cmdFlags, &fullOutput, "full", "", false, "Full numbers instead of human-readable")
}
```
* Add command when init
``` go
var commandDefinition = &cobra.Command{
    ...
    Run: func(command *cobra.Command, args []string) {
		cmd.CheckArgs(1, 1, command, args)
		f := cmd.NewFsSrc(args)
		cmd.Run(false, false, command, func() error {
			doAbout := f.Features().About
			if doAbout == nil {
				return fmt.Errorf("%v doesn't support about", f)
			}
			u, err := doAbout(context.Background())
			if err != nil {
				return fmt.Errorf("About call failed: %w", err)
			}
			if u == nil {
				return errors.New("nil usage returned")
			}
			if jsonOutput {
				out := json.NewEncoder(os.Stdout)
				out.SetIndent("", "\t")
				return out.Encode(u)
			}

			printValue("Total", u.Total, true)
			printValue("Used", u.Used, true)
			printValue("Free", u.Free, true)
			printValue("Trashed", u.Trashed, true)
			printValue("Other", u.Other, true)
			printValue("Objects", u.Objects, false)
			return nil
		})
	},
}
```
* `cmd.Run(false, false, command, func() error` not retry and not show stat
* `doAbout := f.Features().About` get doAbout function
``` go
// printValue formats uv to be output
func printValue(what string, uv *int64, isSize bool) {
	what += ":"
	if uv == nil {
		return
	}
	var val string
	if fullOutput {
		val = fmt.Sprintf("%d", *uv)
	} else if isSize {
		val = fs.SizeSuffix(*uv).ByteUnit()
	} else {
		val = fs.CountSuffix(*uv).String()
	}
	fmt.Printf("%-9s%v\n", what, val)
}
```
* print usage value
## backend.go
``` go
	func(command *cobra.Command, args []string) error {
		cmd.CheckArgs(2, 1e6, command, args)
		name, remote := args[0], args[1]
		cmd.Run(false, false, command, func() error {
			// show help if remote is a backend name
			if name == "help" {
				fsInfo, err := fs.Find(remote)
				if err == nil {
					return showHelp(fsInfo)
				}
			}
			// Create remote
			fsInfo, configName, fsPath, config, err := fs.ConfigFs(remote)
			if err != nil {
				return err
			}
			f, err := fsInfo.NewFs(context.Background(), configName, fsPath, config)
			if err != nil {
				return err
			}
			// Run the command
			var out interface{}
			switch name {
			case "help":
				return showHelp(fsInfo)
			case "features":
				out = operations.GetFsInfo(f)
			default:
				doCommand := f.Features().Command
				if doCommand == nil {
					return fmt.Errorf("%v: doesn't support backend commands", f)
				}
				arg := args[2:]
				opt := rc.ParseOptions(options)
				out, err = doCommand(context.Background(), name, arg, opt)
			}
			if err != nil {
				return fmt.Errorf("command %q failed: %w", name, err)

			}
			// Output the result
			writeJSON := false
			if useJSON {
				writeJSON = true
			} else {
				switch x := out.(type) {
				case nil:
				case string:
					fmt.Println(out)
				case []string:
					for _, line := range x {
						fmt.Println(line)
					}
				default:
					writeJSON = true
				}
			}
			if writeJSON {
				// Write indented JSON to the output
				enc := json.NewEncoder(os.Stdout)
				enc.SetIndent("", "\t")
				err = enc.Encode(out)
				if err != nil {
					return fmt.Errorf("failed to write JSON: %w", err)
				}
			}
			return nil
		})
		return nil
```
* `var out interface{}` out value provided by caller
* `switch x := out.(type)` type switch would handle each case
## bisync.go
``` go
// Opt keeps command line options
var Opt Options

func init() {
	cmd.Root.AddCommand(commandDefinition)
	cmdFlags := commandDefinition.Flags()
	flags.BoolVarP(cmdFlags, &Opt.Resync, "resync", "1", Opt.Resync, "Performs the resync run. Path1 files may overwrite Path2 versions. Consider using --verbose or --dry-run first.")
	flags.BoolVarP(cmdFlags, &Opt.CheckAccess, "check-access", "", Opt.CheckAccess, makeHelp("Ensure expected {CHECKFILE} files are found on both Path1 and Path2 filesystems, else abort."))
	flags.StringVarP(cmdFlags, &Opt.CheckFilename, "check-filename", "", Opt.CheckFilename, makeHelp("Filename for --check-access (default: {CHECKFILE})"))
	flags.BoolVarP(cmdFlags, &Opt.Force, "force", "", Opt.Force, "Bypass --max-delete safety check and run the sync. Consider using with --verbose")
	flags.FVarP(cmdFlags, &Opt.CheckSync, "check-sync", "", "Controls comparison of final listings: true|false|only (default: true)")
	flags.BoolVarP(cmdFlags, &Opt.RemoveEmptyDirs, "remove-empty-dirs", "", Opt.RemoveEmptyDirs, "Remove empty directories at the final cleanup step.")
	flags.StringVarP(cmdFlags, &Opt.FiltersFile, "filters-file", "", Opt.FiltersFile, "Read filtering patterns from a file")
	flags.StringVarP(cmdFlags, &Opt.Workdir, "workdir", "", Opt.Workdir, makeHelp("Use custom working dir - useful for testing. (default: {WORKDIR})"))
	flags.BoolVarP(cmdFlags, &tzLocal, "localtime", "", tzLocal, "Use local time in listings (default: UTC)")
	flags.BoolVarP(cmdFlags, &Opt.NoCleanup, "no-cleanup", "", Opt.NoCleanup, "Retain working files (useful for troubleshooting and testing).")
}
```
* Keep command line options into `Opt`
``` go
	RunE: func(command *cobra.Command, args []string) error {
		cmd.CheckArgs(2, 2, command, args)
		fs1, file1, fs2, file2 := cmd.NewFsSrcDstFiles(args)
		if file1 != "" || file2 != "" {
			return errors.New("paths must be existing directories")
		}

		ctx := context.Background()
		opt := Opt
		opt.applyContext(ctx)

		if tzLocal {
			TZ = time.Local
		}

		commonHashes := fs1.Hashes().Overlap(fs2.Hashes())
		isDropbox1 := strings.HasPrefix(fs1.String(), "Dropbox")
		isDropbox2 := strings.HasPrefix(fs2.String(), "Dropbox")
		if commonHashes == hash.Set(0) && (isDropbox1 || isDropbox2) {
			ci := fs.GetConfig(ctx)
			if !ci.DryRun && !ci.RefreshTimes {
				fs.Debugf(nil, "Using flag --refresh-times is recommended")
			}
		}

		fs.Logf(nil, "bisync is EXPERIMENTAL. Don't use in production!")
		cmd.Run(false, true, command, func() error {
			err := Bisync(ctx, fs1, fs2, &opt)
			if err == ErrBisyncAborted {
				os.Exit(2)
			}
			return err
		})
		return nil
    }	
```
* `fs1, file1, fs2, file2 := cmd.NewFsSrcDstFiles(args)` fs1 is source fs and fs2 is destination fs
* `commonHashes == hash.Set(0)` means no common hashes because two hash number do and operation would return zero if not the same
## cat.go
``` go
	Run: func(command *cobra.Command, args []string) {
		usedOffset := offset != 0 || count >= 0
		usedHead := head > 0
		usedTail := tail > 0
		if usedHead && usedTail || usedHead && usedOffset || usedTail && usedOffset {
			log.Fatalf("Can only use one of  --head, --tail or --offset with --count")
		}
		if head > 0 {
			offset = 0
			count = head
		}
		if tail > 0 {
			offset = -tail
			count = -1
		}
		cmd.CheckArgs(1, 1, command, args)
		fsrc := cmd.NewFsSrc(args)
		var w io.Writer = os.Stdout
		if discard {
			w = ioutil.Discard
		}
		cmd.Run(false, false, command, func() error {
			return operations.Cat(context.Background(), fsrc, w, offset, count)
		})
	},
```
* `usedOffset`, `usedHead`, `usedTail` to validate flag
* `var w io.Writer = os.Stdout` set output to stdout
## check.go
``` go
	RunE: func(command *cobra.Command, args []string) error {
		cmd.CheckArgs(2, 2, command, args)
		var (
			fsrc, fdst fs.Fs
			hashType   hash.Type
			fsum       fs.Fs
			sumFile    string
		)
		if checkFileHashType != "" {
			if err := hashType.Set(checkFileHashType); err != nil {
				fmt.Println(hash.HelpString(0))
				return err
			}
			fsum, sumFile, fsrc = cmd.NewFsSrcFileDst(args)
		} else {
			fsrc, fdst = cmd.NewFsSrcDst(args)
		}

		cmd.Run(false, true, command, func() error {
			opt, close, err := GetCheckOpt(fsrc, fdst)
			if err != nil {
				return err
			}
			defer close()

			if checkFileHashType != "" {
				return operations.CheckSum(context.Background(), fsrc, fsum, sumFile, hashType, opt, download)
			}

			if download {
				return operations.CheckDownload(context.Background(), opt)
			}
			hashType := fsrc.Hashes().Overlap(fdst.Hashes()).GetOne()
			if hashType == hash.None {
				fs.Errorf(nil, "No common hash found - not using a hash for checks")
			} else {
				fs.Infof(nil, "Using %v for hash comparisons", hashType)
			}
			return operations.Check(context.Background(), opt)
		})
		return nil
	}
```
* `opt, close, err := GetCheckOpt(fsrc, fdst)` get check options and close method to be defer
* `operations.CheckSum` CheckSum checks filesystem hashes against a SUM file
* `operations.CheckDownload` check file size and actual file content
*  `operations.Check` check file size and hash
