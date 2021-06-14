---
layout: page
title: PGP
---
[Back to Cheat Sheets](/resources/cheat-sheets/)

[Pretty Good Privacy (PGP)](https://en.wikipedia.org/wiki/Pretty_Good_Privacy)

See [Introduction to GnuPG]{:target="_blank"} for more detail.

``` shell
# install using homebrew
brew install gpg

# generate your personal key
gpg --gen-key

# list keys
gpg --list-keys

# list secret keys
gpg --list-secret-keys

# encrypt a file (requires specifying recipient)
gpg -e -r jsmith@example.com secret.txt

# decrypt and view encrypted file contents
gpg -d secret.txt.gpg

# decrypt and save file contents to a new file
gpg -d -o secret.txt secret.txt.gpg

# import a persons public key
gpg --import publickey.txt

# get ASCII-armored public key
gpg --output publickey.txt --armor --export jsmith@example.com
```

[Introduction to GnuPG]: http://www.ianatkinson.net/computing/gnupg.htm
