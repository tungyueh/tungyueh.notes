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
## all/all.go
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
