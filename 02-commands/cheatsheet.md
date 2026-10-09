# Docker commands cheatsheet (lesson 2.1)

## Key idea: image vs container
- **Image** = read-only template (recipe). Stored on disk, downloaded once.
- **Container** = running instance of an image. One image → many containers.
- Container lives **only while its main process runs**. Process ends → container `Exited`.
- Changes inside a container never modify the image. Delete the container → its data is gone.

## Commands

| Command | What it does | Pitfall |
|---|---|---|
| `docker run <image>` | create + start a container (pulls image if missing) | takes an **image**, not a container |
| `docker run -d <image>` | run in background (detached), prints container ID | |
| `docker run --name web <image>` | set own name instead of random `adjective_scientist` | |
| `docker run ubuntu sleep 30` | override the command run inside the container | container dies when `sleep` ends |
| `docker run -it ubuntu bash` | interactive shell inside a new container | `exit` stops it (bash was the main process) |
| `docker ps` | running containers | stopped ones are hidden |
| `docker ps -a` | all containers incl. stopped | |
| `docker stop <name/id>` | stop: SIGTERM, then SIGKILL after 10s | |
| `docker rm <name/id>` | delete container | must be stopped first |
| `docker container prune` | delete **all** stopped containers | images are NOT touched |
| `docker images` | list images (REPOSITORY, TAG, IMAGE ID, SIZE) | |
| `docker pull <image>` | download only, no run | |
| `docker rmi <image>` | delete image | fails if **any** container (even stopped) uses it → `rm` them first |
| `docker exec <name> <cmd>` | run a command inside a **running** container | `docker exec -it <name> bash` = get a shell inside |
| `docker attach <name/id>` | connect terminal to a background container's output | Ctrl+C *tries* to stop the container |
| `docker logs -f <name>` | follow container output incl. history | safe: Ctrl+C only stops viewing |

## IDs
- Short ID is enough: `docker stop 71db` (add chars if ambiguous).
- `rm` takes **CONTAINER ID** (from `ps -a`), `rmi` takes **IMAGE ID** (from `images`). Mixing them → "No such ...".

## Exit codes (`STATUS` in `ps -a`)
| Code | Meaning |
|---|---|
| `Exited (0)` | finished normally |
| `Exited (1)` | app error |
| `Exited (137)` | killed (didn't stop in 10s, or out of memory) |

## Lessons learned (my own runs)
- `docker run ubuntu` → exits instantly: bash has no terminal, nothing to do.
- `misaogura/whalesay cowsay "Hi"` printed "cowsay Hi": the image already has `cowsay` as ENTRYPOINT → just `docker run misaogura/whalesay "Hi"` (see lesson 3.6).
- `rotorocloud/webapp`: Ctrl+C in `attach` did NOT stop it: main process is `/bin/sh -c ...`, which ignores SIGINT.
- Port not reachable from browser without `-p 5000:5000` (PORTS showed `5000/tcp` with no arrow).
- 4 stopped containers = 102 kB total: containers store only their own changes on top of the shared image.