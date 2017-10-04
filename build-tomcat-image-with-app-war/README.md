# Overview

* Builds a custom image, with an application's WAR baked into it ([Dockerfile](./Dockerfile)), based on the [official Tomcat image](https://hub.docker.com/_/tomcat/).
* Runs the image (as a container).

## Instructions

`cd` to this directory and run:

```
docker-compose up --build
```

Hit a sample app here:

http://localhost:8888/sample/

Clean up:

```
docker-compose down
```

For fun, try changing the Tomcat version in [Dockerfile](./Dockerfile), and repeat the above experiment.

## Scraps

```
exec -it tomcat-war bash -c 'echo $JAVA_VERSION'
```