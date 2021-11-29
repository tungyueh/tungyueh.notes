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
