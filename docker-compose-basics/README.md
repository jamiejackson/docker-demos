# Overview

Intro to docker compose.

## Instructions

### Container-written file persistence

#### Files aren't persisted by default

`cd` to this directory and run:

```sh
# start container with a "volume" (a shared directory, or file)
docker run --rm -d -v $(pwd)/host-volume:/tmp/shared --name=non-compose-example ubuntu tail -f /dev/null
# write a file from the container to a non-shared location
docker exec -it non-compose-example bash -c 'echo "hi, i am the container" > /etc/created_in_container.txt'
# show the file in the container
docker exec -it non-compose-example ls /etc/created_in_container.txt
# stop the container
docker stop non-compose-example
# start container
docker run --rm -d -v $(pwd)/host-volume:/tmp/shared --name=non-compose-example ubuntu tail -f /dev/null
# show the file in the container. (it's gone.)
docker exec -it non-compose-example ls /etc/created_in_container.txt
```

#### Persisting files with volumes

There are a few types of volumes. This example uses a "host" volume (which is the easiest to understand, but not usually recommended -- see references).

```
# start container with a "volume" (a shared directory in this case; but it can be a file)
docker run --rm -d -v $(pwd)/host-volume:/tmp/shared --name=non-compose-example ubuntu tail -f /dev/null
# add a file to the local folder
echo "hi, i am the host" > ./host-volume/created_on_host.txt
# view the "shared" directory in the container
docker exec -it non-compose-example ls -l /tmp/shared
# write a file from the container
docker exec -it non-compose-example bash -c 'echo "hi, i am the container" > /tmp/shared/created_in_container.txt'
# view the directory on the host
ls -l ./host-volume
# clean up
# docker stop non-compose-example
```

#### Breaking down the `docker run` command:

```
docker run --rm -d -v $(pwd)/host-volume:/tmp/shared --name=non-compose-example ubuntu tail -f /dev/null
```

* `docker run` - Run a command in a container. (Many images have default commands, and these can be overridden)
* `--rm` - Remove the container when it's stopped. (The default is to keep it around, in a stopped state, so it would need to be cleaned up afterwards.)
* `-d` - Run in "detached" mode. Otherwise, the stdout of the command streams to the console.
* `-v $(pwd)/host-volume:/tmp/shared` - Share the hosts's `./host-volume` directory with the container, as `/tmp/shared` in the container. These example paths are arbitrary.
* `ubuntu` - The image to use; in this case, the latest ubuntu official image: https://hub.docker.com/_/ubuntu/
* `tail -f /dev/null` - This is a very arbitrary and somewhat weird example. Normally a foreground service (like your app server, or a db server) would keep the container running. This is a "cheat" to simply get a container to stay running.

See `docker help run` for more info.

## Docker Compose

The following is too much to type:

`docker run --rm -d -v $(pwd)/host-volume:/tmp/shared --name=non-compose-example ubuntu tail -f /dev/null`.

We can shorten this with `docker-compose` and its [configuration file](./docker-compose.yml):

```
docker-compose --rm run -d
```

# References

* Docker Volumes: https://nschoe.com/articles/2017-01-28-Docker-Taming-the-Beast-Part-4.html
* Docker compose file: https://docs.docker.com/compose/compose-file/
