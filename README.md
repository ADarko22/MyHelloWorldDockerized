# MyHelloWorldDockerized

This guide will show you how to:
1) create a Docker image for you Java application using Maven plugins
2) how to push the image to Docker.io repository using Maven plugins
3) how to pull and run your dockerized application

### But first, what is a container?
Simply put, a container is a sandboxed process that is isolated from all other processes on the host.
That isolation leverages kernel namespaces and cgroups. To summarize, a container:
- is a runnable instance of an image. You can create, start, stop, move, or delete a container using the DockerAPI or CLI.
- can be run on local machines, virtual machines or deployed to the cloud
- is portable (can be run on any OS)
- is isolated from other containers and runs its own software, binaries, and configurations

When running a container, it uses an isolated filesystem, provided by a container image.
The image must contain everything needed to run an application: all dependencies, configurations, scripts, binaries, etc.

### Docker

**Docker** allows to create independent and isolated environments where to launch and deploy applications. 
These environments are then called *containers*. 
In order to run an application within a Docker container you need to create its *Docker Image*.

*Follow the instructions and examples below to learn how to use Maven Plugins to generate and push the Docker Images for your Java Applications!*

## Maven Plugins

Each module uses the following two maven plugins for building the Docker Image:

### maven-assembly-plugin

The Assembly Plugin for Maven enables to combine the project output into a single distributable archive 
that also contains dependencies, modules, site documentation, and other files. More in [usage documentation](https://maven.apache.org/plugins/maven-assembly-plugin/usage.html)

### dockerfile-maven-plugin

We'll be using [Authentication with maven settings.xml](https://github.com/spotify/dockerfile-maven/blob/master/docs/authentication.md#authenticating-with-maven-settingsxml).
More in [documentation](https://github.com/spotify/dockerfile-maven)

### Create Docker.io account and repo
Docker Hub repositories allow you share container images with your team, customers, or the Docker community at large.

Docker images are pushed to Docker Hub through the docker push command. A single Docker Hub repository can hold many Docker images (stored as tags).

Creating repositories
To create a repository, sign into Docker Hub, click on Repositories then Create Repository:

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

`mvn -Dmaven.deploy.skip` deploy to deploy only Docker images (default deploy phase is skipped) 

This is necessary because we haven't configured any distribution management 
for maven default deploy (see [Distribution Management](https://maven.apache.org/pom.html#Distribution_Management)

In case something goes wrong try to:
1) logout from docker: `docker logout`
2) delete config.json: `rm ~/.docker/config.json`
3) restart docker: `service docker restart`
4login into docker using your docker.io credentials: `docker login docker.io`

TODO: 
    1) CLEAN UP description
    2) document creation of private REPO
    3) describe the Dockerfile and the assembly.xml used by dockerfile-maven-plugin
    4) describe authentication used for dockerfile-maven-plugin
    5) describe deploy instruction

    add hello world that listen on a PORT that is configured from a command line arg
    add hello world that listen on a PORT that is configured from an xml file passed through command line arg
