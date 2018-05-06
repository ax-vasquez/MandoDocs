# [Getting Started on Mac](https://docs.docker.com/docker-for-mac/)
This guide covers the basic steps necessary to set up your local Mac OS machine with Docker CE


## [Download and Install](https://store.docker.com/editions/community/docker-ce-desktop-mac?tab=resources)
1. All you need to do is download the `.dmg` from the Docker store (linked in header)
2. Double-click the `.dmg` file you just downloaded
3. Drag the Docker icon to the applications folder
    - You will have to enter you password

## Verify the Installation
Once Docker is up and running, open a terminal window and confirm that `docker`, `docker-compose`, and `docker-machine` are properly installed:
```bash
docker --version
Docker version 18.03.1-ce, build 9ee9f40

docker-compose --version
docker-compose version 1.21.1, build 5a3f1a3

docker-machine --version
docker-machine version 0.14.0, build 89b8332
```
_**Your output may differ slightly**_