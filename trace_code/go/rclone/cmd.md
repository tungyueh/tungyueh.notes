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
