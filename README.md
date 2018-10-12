# GatsD - The GatsbyJS Project Dockerizer

Develop any GatsbyJS project without the need for dependency management on your local machine - beyond a working installation of Docker, of course.

## How To Install

### New Project

1.  Create directory for new site and enter the new directory.

```
mkdir gatsby-site
cd gatsby-site
```

2.  Download gatsd sub-directory into new site.

```
RELEASE=0.2.0
curl -L https://www.github.com/imburbank/gatsd/archive/v${RELEASE}.tar.gz | tar xzv gatsd-${RELEASE}/gatsd/ --strip=1
```

3. Build GatsD Docker container. The container will use a tag if provided; otherwise, container will be tagged with the `gatsby-site` name.

```
./gatsd/docker-build [tag]
```

The project directory will be accessible from inside the container and any changes made to the project inside the container or on the local host will be visible to both.

4.  Create a new site.

```
./gatsd/new https://github.com/gatsbyjs/gatsby-starter-default
```

The site name is selected when the site directory is created in step 1, so it isn't needed to create a new site (the name in this example is gatsby-site).

> Optionally, you can also call the command with the site name included - as long as it matched the name of the current working directory. 
> 
> ```
> ./gatsd/new gatsby-site https://github.com/gatsbyjs/gatsby-starter-default
> ```
>
> This more verbose option is maintained for consistency the Gatsby CLI.

### Existing Project

TODO

## GatsD Basics

GatsD can be used from the local host machine
```
# For gatsd commands
./gatsd/[command]

# All other commands
./gatsd/run [command]
```

or from inside the container

```
# Enter the container
./gatsd/run

# For gatsd commands inside container
gatsd [command]

# For all other command inside container
[command]
```

## Using the GatsD CLI

GatsD's standard commands are wrappers to invoke Gatsby-CLI commands within the Gatsby-Docker container. 


The following commands are available

-   `gatsd develop` -- Will start a hot-reloading development environment accessible at http://0.0.0.0:8000.

-   `gatsd build` -- Will invoke `gatsby build` inside the container to create an optimized production build of the site.

-   `gatsd serve` -- Will start a local HTML server to serve the production site locally at http://localhost:8000.

-   `gatsd stage` -- GatsD will invoke `gatsd build` and `gatsd serve` to stage a local production build of your site.

### From Local Host Machine

Commands can be called from the local project directory using shortcuts to standard Gatsd commands:
```
./gatsd/[gatsd-command]

# Ex:
./gatsd/stage
```

Or sending commands directly into the container with `run`
```
./gatsd/run [any-command]

# Example running gatsd command in container without shortcut:
./gatsd/run gatsd stage

# Example of running any other command in container
./gatsd/run yarn add --dev eslint
```

### From Inside the Container

You can also enter the container and issue any commands from inside.

To enter the container:
```
./gatsd/run
```

Once inside, you can interact with the container as if it were a normal machine.

To enter GatsD commands, use `gatsd`
```
gatsd [command]

# Ex:
gatsd develop
```

For all other commands, issue them normally
```
yarn install

yarn add gatsby-plugin-react

gatsby develop --help
```

## Calling `docker` directly

The commands provided for this docker image are for convenience only - they are not required. You can also use docker directly with this project.

The `docker-build` file is used to create a default container for this project. `docker build` can also be called directly (read the `docker-build` script for inspiration). For example:

```
# Build gatsd docker container with tag [tag-name] to current directory
docker build -f gatsd/Dockerfile -t [tag-name] .

# Ex:
docker build -f gatsd/Dockerfile -t gatsby-site --build-arg SITE_NAME=gatsby-site --build-arg GATSBY_PORT=8000 .
```

The `docker run` command can also be explicitly invoked (read the `run` script for inspiration). For example:

````
docker run -it -p [port] -u [user] -v [volume] [tag-name] [arguments]

# Ex: 
docker run \
	-it \
	-p 8000:8000 \
	-u $(id -u):$(id -g) \
	-v $(pwd):/gatsby-site \
	-w /gatsby-site \
	gatsby-site \
	gatsby develop -H 0.0.0.0
```