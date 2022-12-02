# MyHelloWorld Dockerized

This guide shows how to set up a maven project that automatically builds a docker image
for a Java application and pushes it to [docker hub](https://hub.docker.com/) repository.

## Pre requisites

### Create your Repository on [Docker Hub](https://hub.docker.com/repository/docker)

1) create an account
2) create a private repository

Follow the [official guide](https://docs.docker.com/docker-hub/) in case you have difficulties in process.

In this guide I'll use my private repository: `adarko22/tutorial`

## Maven Plugins

In order to build and push to your repo the image for your Java application you first need to:

1) create a Jar with all the dependencies required to run your application;
   for this task `maven-assembly-plugin` is ideal!
2) create the image with a Dockerfile and push it to your repo;
   the `dockerfile-maven-plugin` is what we need!

### maven-assembly-plugin

The Assembly Plugin for Maven enables to combine the project output into a single distributable archive
that also contains dependencies, modules, site documentation, and other files.
The plugin is easy to configure, see [my-hello-world pom.xml](my-hello-world/pom.xml),
the most important property is your application's _main class_.

More in [usage documentation](https://maven.apache.org/plugins/maven-assembly-plugin/usage.html)

### dockerfile-maven-plugin

todo add some info
We'll be
using [Authentication with maven settings.xml](https://github.com/spotify/dockerfile-maven/blob/master/docs/authentication.md#authenticating-with-maven-settingsxml).

todo specify how to configure:

- contextDirectory
- repository
- tag
- useMavenSettingsForAuth & authentication via settings.xml

More in [documentation](https://github.com/spotify/dockerfile-maven)

#### authentication

add to settings.xml:

```        
<pluginGroups>
    <pluginGroup>com.spotify</pluginGroup>
</pluginGroups>

<server>
    <id>docker.io</id>
    <username>FOO</username>
    <password>BAR</password>
</server>
```

## Deploying

`mvn clean install -Dmaven.deploy.skip` to deploy only Docker images 
(default deploy phase is skipped)

This is necessary because we haven't configured any distribution management
for maven default deploy (see [Distribution Management](https://maven.apache.org/pom.html#Distribution_Management)

In case something goes wrong try to:

1) logout from docker: `docker logout`
2) delete config.json: `rm ~/.docker/config.json`
3) restart docker: `service docker restart`
4) login into docker using your docker.io credentials: `docker login docker.io`

## Running the image

Once deployed the image is available in the local repository; verify it with:
```
docker images
```
In my case the output is:
```
REPOSITORY           TAG              IMAGE ID       CREATED         SIZE
adarko22/tutorial    my-hello-world   8ebfd19a3dc1   4 minutes ago   132MB
```

When running the image with:
```
docker run adarko22/tutorial:my-hello-world
```
The image is retrieved from the local repo. 
In order to get the image from the remote repo remove the image from the local repo with:
```
docker rmi -f 8ebfd19a3dc1
```
(where *8ebfd19a3dc1* is the image-id to remove); 
at this point the `docker run` command automatically pulls the image from the remote repo.