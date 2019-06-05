# Set up automatic updates

The unattended-upgrades package can be used to automatically install updated packages, and can be configured to update all packages or just install security updates, follow the next steps:

**Install the unattended-upgrades package:**
```bash
sudo apt install unattended-upgrades
```

**configure automatic updates by editing the configuration file:**

```bash
sudo nano /etc/apt/apt.conf.d/50unattended-upgrades
```
The beginning of the configuration file should look like this:
```
// Automatically upgrade packages from these (origin:archive) pairs
//
// Note that in Ubuntu security updates may pull in new dependencies
// from non-security sources (e.g. chromium). By allowing the release
// pocket these get automatically pulled in.
Unattended-Upgrade::Allowed-Origins {
        "${distro_id}:${distro_codename}";
        "${distro_id}:${distro_codename}-security";
        // Extended Security Maintenance; doesn't necessarily exist for
        // every release and this system may not have it installed, but if
        // available, the policy for updates is such that unattended-upgrades
        // should also install from here by default.
        "${distro_id}ESM:${distro_codename}";
//      "${distro_id}:${distro_codename}-updates";
//      "${distro_id}:${distro_codename}-proposed";
//      "${distro_id}:${distro_codename}-backports";
};
```
Anything after a double slash `//` is a comments and has no effect.
To `enable` a line, remove the double slash at the beginning of the line (replace with nothing or with spaces to keep alignment).

The most important: uncomment the “updates” line by deleting the two slashes at the beginning of it:
```
"${distro_id}:${distro_codename}-updates";
```

Optional: You should uncomment and adapt the following lines to ensure you’ll be notified if an error happens:
```
Unattended-Upgrade::Mail "user@example.com";
Unattended-Upgrade::MailOnlyOnError "true";
```

Recommended: remove unused kernel packages and dependencies and make sure the system automatically reboots if needed by uncommenting and adapting the following lines:
```
Unattended-Upgrade::Remove-Unused-Kernel-Packages "true";
```
↑ You may have to add a semicolon at the end of this line. ↑
```
Unattended-Upgrade::Remove-Unused-Dependencies "true";
Unattended-Upgrade::Automatic-Reboot "true";
Unattended-Upgrade::Automatic-Reboot-Time "02:38";
```
To save your changes in nano, use `Ctrl + O` followed by `Enter`. To quit, use `Ctrl + X`.

**Enable automatic updates and set up update intervals by running:**
```bash
sudo nano /etc/apt/apt.conf.d/20auto-upgrades
```

In most cases, the file will be empty. Copy and paste the following lines:

```
APT::Periodic::Update-Package-Lists "1";
APT::Periodic::Download-Upgradeable-Packages "1";
APT::Periodic::AutocleanInterval "7";
APT::Periodic::Unattended-Upgrade "1";
```

The time interval are specified in days, feel free to change the values. Save changes and exit.

**You can see if the auto-upgrades work by launching a dry run:**
```
sudo unattended-upgrades --dry-run --debug
```
