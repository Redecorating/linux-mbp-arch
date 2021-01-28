I am aware that this is a bad method, but it's just to test patches so meh, please don't judge me uwu


1. Get the source code for this release https://github.com/marcosfad/mbp-ubuntu-kernel/releases/tag/v5.7.19

2. Edit `build.sh`, add sonething to halt the build before the patches are applied (here I've added `top`

```sh
#### Apply patches
cd "${KERNEL_PATH}" || exit
top
echo >&2 "===]> Info: Applying patches... "
[ ! -d "${WORKING_PATH}/patches" ] && {
  echo 'Patches directory not found!'
  exit 1
}
```

3. If you aren't on Ubuntu, edit `build_in_docker.sh`, the additional `-v` option will make it save the packages on your filesystem:

```sh
#!/bin/bash

set -eu -o pipefail

DOCKER_IMAGE=ubuntu:20.04

docker pull ${DOCKER_IMAGE}
docker run \
  -t \
  --rm \
  -v "$(pwd)/apt-repo":/tmp/artifacts \
  -v "$(pwd)":/repo \
  ${DOCKER_IMAGE} \
  /bin/bash -c 'cd /repo && ./build.sh'
```

4. If you're on ubuntu run `build.sh`, otherwise run `build_in_docker.sh`, and wait for it to pause at the point you added

5. If you ran it without docker, you can copy the patches from this repo to /root/work/patches and overwrite the old ones and exit top with `q` and let it finish building, you're done. If you ran it in docker follow the next steps:

6. `docker ps -a`, get the container id for the next command.

7. `docker exec -it $CONTAINER_ID_FROM_LAST_STEP /bin/bash`

8. In this shell, install a text editor `apt install vim` (or nano, etc)

9. `cd /root/work/patches`

10. Use your editor to replace the two `400x` patches with the ones from this repo

11. kill htop from your shell

12. I know this is a very bad method of doing it, but eh, it seems to be working.
