# Setting up your SSH client

Throughout this tutorial we will use `ssh` to connect to linux server.

* On linux or osx ssh is already installed.
* On windows, install git with bash tools and open git-bash.

Make sure you have an ssh rsa key pair.

It should be located in `~/.ssh`:

  * The private key is located at `~/.ssh/id_rsa`.  Keep it safe! Make sure you have a backup.
  * The public key is located at `~/.ssh/id_rsa.pub`.   This should be uploaded to your server.  Display it with the following command:

         cat ~/.ssh/id_rsa.pub

**Note:** If you do not have `id_rsa` and `id_rsa.pub`, run the following command to create the key pair:

    ssh-keygen

Most service providers allows to create servers with your public key already installed on the server.  Otherwise, add manually to your server's '/root/.ssh/authorized_keys` file.  (Tip:  most linux distros include a ssh-copy-id batch file that does this for you).

## Connecting To Your Server

Connect to your server with the following command:

    ssh root@myproject.mydomain.com

You might need to approve your host one time:

    The authenticity of host 'myproject.mydomain.com (111.222.33.4)' can't be established.
    ECDSA key fingerprint is SHA256:xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx.
    Are you sure you want to continue connecting (yes/no)?

Enter `yes`.

You should be connected to your server now :-)

Try running some commands on your server:

    pwd
    uptime
    ls -lh
    uname -a
    python --version

Use <kbd>Ctrl</kbd>+<kbd>D</kbd> to disconnect.
