# Step 6 - Extra for experts

AppArmor profiles are very application-specific. Although we've had some practice writing our own profiles, the preferred method is using tools to generate and debug them. In this step We'll explore `aa-complain` and `aa-genprof` that ship as part of the `apparmor-utils` package.

## Task
The following steps assume you are using a modern Ubuntu Linux Docker Host with a GUI installed and the FireFox web browser.

Install the apparmor-utils package.

`sudo apt install apparmor-utils -y`

Start aa-genprof to automatically generate a new AppArmor profile for the FireFox app.

`sudo aa-genprof firefox`

Your terminal will go into interactive mode. This is AppArmor watching the Firefox app.

Open Firefox and perform normal web browsing activities such as browsing websites and downloading content.

Go back to the terminal running `aa-genprof` and press `s` to view events from the system log and decide whether or not to include these events in your AppArmor profile as an allow or deny statement.

When you're done, press `f` to finish.

The AppArmor profile will be called `usr.bin.firefox` and will be stored in /etc/apparmor.d/ on Ubuntu systems.

You can also use `aa-autodep` to automatically generate profiles, but this will create an even more minimal profile.

To further refine an existing profile, `aa-logprof` operates in the same manner as `aa-genprof` but for amending a profile by scanning logs.

You can debug AppArmor profiles with `aa-complain`. As described earlier, AppArmor has a "complain" mode that logs disallowed events instead of actively blocking them. This is a great tool for testing and debugging.

Type the following command to view the Firefox profile ``- sudo aa-complain /etc/apparmor.d/usr.bin.firefox.``

Confirm that this set the Firefox policy to complain mode by running ``apparmor_status``.

To view any complaints from apparmor, run` dmesg`.
