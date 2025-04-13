# sonatype-maven-central-example
A repo to test publishing to Sonatype Maven Central Repository

## Step to Publish this repo to Sonatype Maven Central

Run the following commands in your terminal with their correct values.

### 1. Generate Key pair or make use of already existing key pair if any

The command below generate new one using `GnuPG` and follow the prompt. Install it if not found in your system

```bash
gpg --gen-key
```

### 2. Get your Sonatype Maven Central User Token(MILL_SONATYPE_USERNAME) and Token Key value(MILL_SONATYPE_PASSWORD)

```bash
export MILL_SONATYPE_USERNAME=PutUserTokenHere
export MILL_SONATYPE_PASSWORD=PutTokenKeyValueHere
```

### 3. This command exports your PGP secret key in Base64 format

```bash
export MILL_PGP_SECRET_BASE64=PutKeyHere
```

The passphrase associated with your PGP key, if any

```bash
export MILL_PGP_PASSPHRASE=PutPassphraseHere
```

### 4. Send your public key to where Sonatype Maven Central can use to verify

This command sends your public key

```bash
gpg --keyserver  keyserver.ubuntu.com --send-keys longIDHere
```

This command checks the server to confirm it's sent. You should see info about the key to show it's sent successfully. Run the command few seconds to few minutes from running the command that sent the key

```bash
gpg --keyserver  keyserver.ubuntu.com --recv-keys longIDHere
```

### 5. Publish to Sonatype Maven Central (Individual module `foo`)

```bash
./mill -i foo.publishSonatypeCentral
```

### 6. Publish to Sonatype Maven Central(Several modules publish at once)

```bash
mill -i \
mill.contrib.sonatypecentral.SonatypeCentralPublishModule/publishAll \
--username myusername \
--password mypassword \
--gpgArgs --passphrase=$MILL_PGP_PASSPHRASE,--no-tty,--pinentry-mode,loopback,--batch,--yes,-a,-b \
--publishArtifacts __.publishArtifacts \
--readTimeout  36000 \
--awaitTimeout 36000 \
--connectTimeout 36000 \
--shouldRelease false \
--bundleName io.github.c0d33ngr-foo:0.0.3
```

![screenshoot_one](Screenshot%20from%202025-04-13%2008-36-53.png)

![screenshot_two](Screenshot%20from%202025-04-13%2009-25-29.png)



Note: This was done using `sonatypecentral contrib plugin` from `Mill` which might not be a plugin in the future in Mill ecosystem