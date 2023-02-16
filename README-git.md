
GIT Details
===========

Overview
--------

Notes about setting up git, gpg, and signing.

By practice, I dump my source code from WSL into the windows file system so
that the code isn't contained just within the WSL virtual machine.  As an 
example, in my Ubuntu WSL instance:

```bash
$ ln -s /mnt/c/Users/username/source ~/src
```


WSL Setup
---------

[hint](https://askubuntu.com/questions/1115564/wsl-ubuntu-distro-how-to-solve-operation-not-permitted-on-cloning-repository)

By default, WSL mounts local filesystems from Windows as network mounts which
do not allow file chmod operations.

Common error message from this is found as:
```
Cloning into '<repo>'...
error: chmod on /mnt/c/Users/Efsta/Code/<repo>/.git/config.lock failed: Operation not permitted
fatal: could not set 'core.filemode' to 'false'
```

This is caused by the default WSL mount.  You can remount the file system, but
you generally have to enable `metadata`.  This is easily enabled by editing the
file `/etc/wsl.conf` or creating it if it does not exist.  Set the following:

```
[automount]
options = "metadata"
```

This will allow you to write to Windows file systems.


Generate GPG Keys
-----------------

[hint](https://brain2life.hashnode.dev/how-to-sign-your-git-commits-in-ubuntu-2004-and-why-you-need-it)

### Generate the Key

```bash
gpg --full-gen-key
```

### Get the Key Format

```bash
gpg --list-secret-keys --keyid-format LONG {{ email address }}
```

### Export the Pub Key

```bash
gpg --armor --export {{ key id }}
```


Configure/Test GPG
------------------

[hint](https://stackoverflow.com/questions/41052538/git-error-gpg-failed-to-sign-data)

### Enable GPG Signing on TTY

Enable on the current TTY:
```bash
export GPG_TTY=$(tty)
```

Persist:
```bash
echo export GPG_TTY=\$\(tty\) >> ~/.bashrc
```

### Test GPG Signing
```bash
$ echo "test" | gpg --clearsign
```


Link GPG Key with Git
---------------------

[hint](https://brain2life.hashnode.dev/how-to-sign-your-git-commits-in-ubuntu-2004-and-why-you-need-it#heading-associate-your-gpg-key-with-git)

### Configure the signing key

```bash
git config --global user.signingkey {{ key id }}
```

### Force Signing

```bash
git config --global commit.gpgsign true
```
