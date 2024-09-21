### Generate GPG Keys

## Generate Key
```
gpg --gen-key
```

## Add key to git
```
git config --global user.signingkey <KEY_ID>
```

## Sign all git commits by default
```
git config --global commit.gpgsign true
```
