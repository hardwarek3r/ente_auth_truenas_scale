# Ente Auth on TrueNAS Scale

## Intro

This is a guide on how to quickly setup Ente Auth on TrueNAS Scale Electron Eel (24.10)

## Requirements

- TrueNAS Scale 24.10 (Because you need the feature of custom docker apps)


## Steps

1- Create a Dataset of type `Apps` named Ente (in my case it's inside `Apps_Config` Dataset)

2- Run the commands bellow using the Shell (System -> Shell):

```bash
$  cd /mnt/POOL/Apps_Config/Ente
$  curl -LO https://raw.githubusercontent.com/ente-io/ente/main/server/compose.yaml
$  curl -LO https://raw.githubusercontent.com/ente-io/ente/main/server/scripts/compose/credentials.yaml
$  curl -LO https://raw.githubusercontent.com/ente-io/ente/main/server/scripts/compose/minio-provision.sh
$  touch museum.yaml
$  mkdir data

```
POOL is the name of your pool


3- Open `compose.yaml` with `nano` of `vim` and replace the following:

```diff
3,6c3
<     build:
<       context: .
<       args:
<         GIT_COMMIT: development-cluster
---
>     image: ghcr.io/ente-io/server
15c12
<       ENTE_CREDENTIALS_FILE: /credentials.yaml
---
>       ENTE_CREDENTIALS_FILE: /mnt/nas/Apps_Config/Ente/credentials.yaml
18,20c15,17
<       - ./museum.yaml:/museum.yaml:ro
<       - ./scripts/compose/credentials.yaml:/credentials.yaml:ro
<       - ./data:/data:ro
---
>       - /mnt/nas/Apps_Config/Ente/museum.yaml:/museum.yaml:ro
>       - /mnt/nas/Apps_Config/Ente/credentials.yaml:/credentials.yaml:ro
>       - /mnt/nas/Apps_Config/Ente/data:/data:ro
80c77
<       - ./scripts/compose/minio-provision.sh:/provision.sh:ro
---
>       - /mnt/nas/Apps_Config/Ente/minio-provision.sh:/provision.sh:ro
```

4- Copy the content of the modifier `compose.yaml` then go to `Apps -> Discover Apps -> Install via YAML`
and paste the content

5- Make sure the ports 8080, 2112, 5432, 3200, 3201 aren't used by other apps, if so replace them

6- After that, you can test it by going to `http://TRUENAS_IP:8080/ping`


## References

- https://github.com/ente-io/ente/blob/main/server/docs/docker.md