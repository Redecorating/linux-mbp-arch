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

3. If you're on ubuntu run `build.sh`, otherwise run `build_in_docker.sh`, and wait for it to pause at the point you added

4. If you ran it without docker, you can copy the patches from this repo to /root/work/patches and overwrite the old ones and exit top with `q` and let it finish building, you're done. If you ran it in docker follow the next steps:

5. `docker ps -a`, get the container id for the next command.

6. `docker exec -it $CONTAINER_ID_FROM_LAST_STEP /bin/bash`

7. In this shell, install a text editor `apt install vim` (or nano, etc)

8. `cd /root/work/patches`

9. Use your editor to replace the two `400x` patches with the ones from this repo

10. kill htop from your shell

11. I know this is a very bad method of doing it, but eh, it seems to be working.
