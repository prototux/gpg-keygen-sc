# gpg-keygen-sc

Simple script to automate GPG key creations + export to smartcard (eg. yubikey)

## Features

* Creates a master key (exported) + 3 subkeys (sign, crypt, auth)
* Keys are RSA 4096bits by default (supports yubikey 4)
* Creates a lightweight backup if `paperkey` is present
* Exports to smartcards (by default)
* Supports multiple smartcards

## WARN

* If gpg-agent is already running, it will kill it (to access the smartcard)
* If a smartcard is present, it WILL factory reset it, no questions asked

## Usage and config

just run the script, a optional config file parameter can be set

Config format:
```bash
NAME="John Doe" # It will ask by default
EMAIL="nobody@example.org" # It will ask by default
COMMENT="" # Current date by default
EXPIRATION="2y" # Default
KEYTYPE="RSA" # Default
KEYSIZE="4096" # Default
OUTDIR="keyout" # Output dir
NOSC="false" # No smartcard (bypasses key export)
```

## Todo

Because i'm lazy:
* Have a way to define the admin pin and reset pin to something else than the passphrase
* Have shellcheck... check the script
* Check if some best practices hasn't been forgoten
* Maybe check if there's a way to not do echo pipe gpg with gpgme

## What to do next

### Use your GPG key for SSH auth
Simply put `export SSH_AUTH_SOCK=$(gpgconf –list-dirs agent-ssh-socket)` in your bashrc/zshrc/etc
This need `enable-ssh-support` in `~/.gnupg/gpg-agent.conf` (you can do `gpgconf –kill gpg-agent && eval $(gpg-agent –daemon)` to reload the config

### Sign your git commits
You can configure git to automatically sign your commits
```
git config –global user.signingkey (key ID)
git config –global commit.gpgsign true
```

### Use GPG as your password store
See https://www.passwordstore.org/, firefox ext @ https://codeberg.org/PassFF/passff chrome ext @ https://github.com/browserpass/,
menu @ https://github.com/carnager/rofi-pass, and TOTP support @ https://github.com/tadfisher/pass-otp/

### Sign/Decrypt emails
See https://www.enigmail.net
