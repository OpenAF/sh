# sh

Builds single static file OpenAF executables for Linux and Mac. These single files can be copied and used, without any installation or privilege access (e.g. power user or root), to run OpenAF scripts and oJobs. 

To download and use (_curl_, _bash_ and _tar_ are required):

```bash
curl https://openaf.io/getStatic.sh | sh
```

If you don't have curl you can run the following command directly using only bash:

```bash
/bin/bash -c "exec 3<>/dev/tcp/ojob.io/80 && echo -e \"GET /getStatic.sh HTTP/1.1\nHost: ojob.io\nUser-Agent: curl\nConnection: close\n\n\" >&3 && cat <&3" | sed '1,/connection: close/d' | tail -n +2 > getStatic.sh && sh getStatic.sh && rm getStatic.sh
```

To download directly for a specific distribution, OS and/or architecture choose one of the following available builds:

| OpenAF distribution | OS | Architecture | Link |
|---------------------|----|--------------|------|
| stable | Linux | x86_64 | https://openaf.io/oaf-linux-x86_64 |
| stable | Linux | aarch64 | https://openaf.io/oaf-linux-aarch64 |
| stable | Alpine Linux | x86_64 | https://openaf.io/oaf-alpine-x86_64 |
| stable | Alpine Linux | aarch64 | https://openaf.io/oaf-alpine-aarch64 |
| stable | Mac | x64_64 | https://openaf.io/oaf-mac-x86_64 |
| stable | Mac | aarch64 | https://openaf.io/oaf-mac-aarch64 | 
| nightly | Linux | x86_64 | https://openaf.io/nightly/oaf-linux-x86_64 |
| nightly | Linux | aarch64 | https://openaf.io/nightly/oaf-linux-aarch64 |
| nightly | Alpine Linux | x86_64 | https://openaf.io/nightly/oaf-alpine-x86_64 |
| nightly | Alpine Linux | aarch64 | https://openaf.io/nightly/oaf-alpine-aarch64 |
| nightly | Mac | x64_64 | https://openaf.io/nightly/oaf-mac-x86_64  |
| nightly | Mac | aarch64 | https://openaf.io/nightly/oaf-mac-aarch64 | 
| t8 | Linux | x86_64 | https://openaf.io/t8/oaf-linux-x86_64 |
| t8 | Linux | aarch64 | https://openaf.io/t8/oaf-linux-aarch64 |
| t8 | Alpine Linux | x86_64 | https://openaf.io/t8/oaf-alpine-x86_64 |
| t8 | Alpine Linux | aarch64 | https://openaf.io/t8/oaf-alpine-aarch64 |
| t8 | Mac | x64_64 | https://openaf.io/t8/oaf-mac-x86_64 |
| t8 | Mac | aarch64 | https://openaf.io/t8/oaf-mac-aarch64 | 

> Note: currently all files are built with Java Runtime Environment version 21 for each of the corresponding architectures.

> All extra files (for example, opacks) are kept in temporary folders. To ensure that they get loaded properly just use the OpenAF function _includeOPack_ or fill the corresponding oJob "opacks" section. For "air-gap" environments (without Internet) you just need to place the necessary .opack files in the same folder from where you are running you script or oJob

## Manual usage

### Linux Intel

```bash
# Download the correct binary file
wget https://openaf.io/oaf-linux-x86_64

# On the target system execute one first time to create the necessary symlinks
chmod u+x oaf-linux-x86_64
./oaf-linux-x86_64
# It will reply 'Please use the created symlinks: oaf, ojob, opack, oafc, oaf-sb or ojob-sb'

# And it's running
./oaf -c 'print(123)'
```

### Linux ARM

```bash
# Download the correct binary file
wget https://openaf.io/oaf-linux-aarch64

# On the target system execute one first time to create the necessary symlinks
chmod u+x oaf-linux-aarch64
./oaf-linux-aarch64
# It will reply 'Please use the created symlinks: oaf, ojob, opack, oafc, oaf-sb or ojob-sb'

# And it's running
./oaf -c 'print(123)'
```

### Mac Intel

```bash
# Download the correct binary file
wget https://openaf.io/oaf-mac-x86_64

# On the target system execute one first time to create the necessary symlinks
chmod u+x oaf-mac-x86_64
./oaf-mac-x86_64
# It will reply 'Please use the created symlinks: oaf, ojob, opack, oafc, oaf-sb or ojob-sb'

# And it's running
./oaf -c 'print(123)'
```

### Mac ARM

```bash
# Download the correct binary file
wget https://openaf.io/oaf-mac-aarch64

# On the target system execute one first time to create the necessary symlinks
chmod u+x oaf-mac-aarch64
./oaf-mac-aarch64
# It will reply 'Please use the created symlinks: oaf, ojob, opack, oafc, oaf-sb or ojob-sb'

# And it's running
./oaf -c 'print(123)'
```
