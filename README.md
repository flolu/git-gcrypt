<div align="center">
  <h1>Encrypt Git Repositories</h1>
  <p>Guide to using encrypted Git remotes with the help of git-remote-gcrypt</p>
  <a href="https://youtu.be/XdoTca3EQGU">
    <img width="320px" height="180px" src="https://img.youtube.com/vi/XdoTca3EQGU/mqdefault.jpg" style="border-radius: 1rem;" />
    <p>Watch the YouTube Tutorial</p>
  </a>
</div>

## Requirements

- [Git](https://git-scm.com)
- [git-remote-gcrypt](https://spwhitton.name/tech/code/git-remote-gcrypt)
- [GnuPG](https://gnupg.org)

## Install requirements

**Debian**

```
apt install git git-remote-gcrypt gnupg
```

**Fedora**

```
dnf install git git-remote-gcrypt gnupg
```

**Arch**

```
pacman -S install git git-remote-gcrypt gnupg
```

## Usage

> Read the git-remote-gcrypt [documentation](https://github.com/spwhitton/git-remote-gcrypt) for more details

**1. Setup GnuPG key**

If you don't already have a GnuPG key, then you need to generate one:

```
gpg --full-gen-key
```

You can list all of your GnuGP keys with:

```
gpg --list-keys
```

**2. Backup your GnuPG key**

> I highly recommend to backup your GnuPG securely. Because loosing your GnuPG key also means loosing access to your encrypted Git remote!

You can obtain your key's fingerprint by running:

```
gpg --list-keys
```

Then, to backup your GnuPG key, run the command below and store the `private.gpg` file securely:

```
gpg -o private.gpg --export-options backup --export-secret-keys <gpg_key_fingerprint>
```

To restore you GnuPG you can use this command:

```
gpg --import-options restore --import private.gpg
```

**3. Add encrypted remote**

To add an encrypted remote, you simply have to prefix the remote url with `gcrypt::`, for instance:

```
git remote add origin gcrypt::https://github.com/flolu/encrypted
```

**4. Configure encryption**

You also need to specify which GnuPG keys can encrypt and decrypt this remote:

```bash
git config remote.origin.gcrypt-participants "<key_fingerprint>"
```

Lastly, you have to specify the GnuPG used for encryption

```bash
git config --global user.signingkey "<key_fingerprint>"
```

**5. Push changes to remote**

Now you can make commits and upload the changes to the encrypted remote as usual:

> Since git-remote-gcrypt uses `--force` to push changes, always make sure to run `git pull` first!

```
git push origin master
```

> On macOS, you might have to add `export GPG_TTY=$(tty)` to your `.zshrc` file to accept password inputs to access your private GnuPG key.

**6. Pull changes from remote**

Cloning an encrypted Git remote also works as usual. You just have to prefix the remote url with `gcrypt::`. You also need to have access to your GnuPG for decryption.

```
git clone gcrypt::<remote_url>
```
