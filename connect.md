So... if you read this we assume you can connect to your new vps with a public key using the following command:

    ssh root@myproject.mydomain.com

You might need to approve your host one time:

    The authenticity of host 'myproject.mydomain.com (111.222.33.4)' can't be established.
    ECDSA key fingerprint is SHA256:xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx.
    Are you sure you want to continue connecting (yes/no)?

Enter `yes`.

In the next step we will create another user called `sysop`.  This user will be used for all administrative tasks instead of root.  We will give this user full premissions, just as root, but only when using the `sudo` command.  This does not provide better security, but will allow us to make less damage unnoticed :-)

Create the `sysop` user:

    adduser sysop --gecos '' --disabled-password

This also create the folder `/home/sysop/` which will hold or project files.

Let's allow this user to run commands as root ("sudo") without a password:

    echo 'sysop ALL=(ALL) NOPASSWD:ALL' > /etc/sudoers.d/50-sysop
    chmod 0440 /etc/sudoers.d/50-sysop
    visudo -c

And copy the current allowed public keys from `root` to `sysop`:

    mkdir /home/sysop/.ssh
    cp .ssh/authorized_keys /home/sysop/.ssh/
    chown sysop /home/sysop/.ssh /home/sysop/.ssh/*

Now logout from the remote system using <kbd>Ctrl</kbd>+<kbd>D</kbd> and connect as sysop:

    ssh sysop@myproject.mydomain.com

Once connected, make sure you are able to run commands as root without entering a password:

    sudo echo hi
