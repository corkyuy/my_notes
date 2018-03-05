# Authenticator

## Using oathtool

secret=`cat Volumes/Keys/keys/$1`
oathtool --totp -b <secret> | pbcopy;
