# Example for ZTP + EEM + GuestShell

# Configure Guest Shell via EEM applet

This applet is used to send the CLI commands to configure guest shell. Note the wait times to allow for day0guestshell to shut down and remove some conflicting CLI's before the CLI's are applied by this applet.

# Enable Guest Shell via EEM applet

This applet is used to enable Guest Shell and run a script automatically. Note the wait time to allow for Guest Shell to start after the day0guestshell is destroyed and conflicting CLI's removed.

# Run the EEM applets and copy commands
Copy some files to the guest-share folder so that the EEM applet can execute them. Now the EEM applets are run. Again note the wait times, first the config CLI's are added, then Guest Shell is enabled and command is run.



