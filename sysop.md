# Creating a sysop user

So... if you read this we assume you can connect to your new vps with a public key using the following command:

```sh
ssh root@myproject.mydomain.com
```

In the next step we will create another user called `sysop`.  This user will be used for all administrative tasks instead of root.  We will give this user full premissions, just as root, but only when using the `sudo` command.

Create the `sysop` user:

```bash
adduser sysop --gecos '' --disabled-password
```

This also create the folder `/home/sysop/` which will hold or project files.

Let's allow this user to run commands as root ("sudo") without a password:

```bash
echo 'sysop ALL=(ALL) NOPASSWD:ALL' > /etc/sudoers.d/50-sysop
chmod 0440 /etc/sudoers.d/50-sysop
visudo -c
```

And copy the current allowed public keys from `root` to `sysop`:

```bash
mkdir /home/sysop/.ssh
cp .ssh/authorized_keys /home/sysop/.ssh/
chown sysop /home/sysop/.ssh /home/sysop/.ssh/*
```

Now logout from the remote system using <kbd>Ctrl</kbd>+<kbd>D</kbd> and connect as sysop:

    ssh sysop@myproject.mydomain.com

Once connected, make sure you are able to run commands as root without entering a password:

```bash
sudo echo hi
```


**Note:**   Creating another user with `sudo` does not provide better security, but will allow us to make less damage by mistake (For example, installing python packages into system folders with `pip`).