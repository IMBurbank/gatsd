# GatsD - The GatsbyJS Project Dockerizer

Develop any GatsbyJS project without the need for dependency management on your local machine - beyond a working installation of Docker, of course.

If you haven't already set up your development environment to work on a website using GatsbyJS, you'll never need to. Maintain all the conveniences and power of Gatsby without the need to download or maintain version consistency of anything global -- node, npm, yarn, gatsby-cli, vips, fftw or any other obscure global package. 

If you already have all the global packages, stop worrying about managing them. 

GatsD will work just as well for new and existing GatsbyJS sites.

## How To Install

GatsD can be installed and used to create a new project, or it can be installed into an existing project. Either way, it's as simple as installing the `gatsd/` directory and running a command to build your Docker container.

### New Project

1.  Create directory for new site and enter the new directory.

```shell
mkdir mysite
cd mysite
```

2.  Download gatsd sub-directory into new site.

```shell
RELEASE=0.3.0
curl -L https://www.github.com/imburbank/gatsd/archive/v${RELEASE}.tar.gz | tar xzv gatsd-${RELEASE}/gatsd/ --strip=1
```

3. Initialize Gatsby project with GatsD.

```shell
./gatsd/new https://github.com/gatsbyjs/gatsby-starter-default
```

GatsD will build the Docker container and download the selected Gatsby starter template from github. By default, container will be tagged with the `mysite` name. If no site is passed as an argument to `./gatsd/new`, then the `gatsby-starter-default` template will be downloaded.

The project directory will be accessible from inside the container and any changes made to the project inside the container or on the local host will be visible to both.

### Existing Project

1.  Enter the site directory.

```shell
cd path/to/mysite/
```

2.  Download gatsd sub-directory into existing site.

```shell
RELEASE=0.3.0
curl -L https://www.github.com/imburbank/gatsd/archive/v${RELEASE}.tar.gz | tar xzv gatsd-${RELEASE}/gatsd/ --strip=1
```

3. Initialize GatsD.

```shell
./gatsd/new
```

GatsD will build the Docker container and initialize the `node_nodules` with `yarn`. By default, container will be tagged with the `mysite` name.

The project directory will be accessible from inside the container and any changes made to the project inside the container or on the local host will be visible to both.

## GatsD Basics

You can continue to develop your Gatsby site with all the normal `docker` and `gatsby` commands, but the GatsD convenience wrapper is now available.

GatsD can be used from the local host machine.
```shell
# For gatsd commands
./gatsd/[command]

# To run all other commands inside the container
./gatsd/run [command]
```

GatsD also provide an easy way to enter the container

```shell
# Enter the container
./gatsd/run
```
```shell
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
```shell
./gatsd/[gatsd-command]

# Ex:
./gatsd/stage
```

Or sending commands directly into the container with `run`
```shell
./gatsd/run [any-command]

# Example running gatsd command in container without shortcut:
./gatsd/run gatsd stage

# Example of running any other command in container
./gatsd/run yarn add --dev eslint
```

### From Inside the Container

You can also enter the container and issue any commands from inside.

To enter the container:
```shell
./gatsd/run
```

Once inside, you can interact with the container as if it were a normal machine.

To enter GatsD commands, use `gatsd`
```shell
gatsd [command]

# Ex:
gatsd develop
```

For all other commands, issue them normally
```shell
yarn install

yarn add gatsby-plugin-react

gatsby develop --help
```

## Calling `docker` directly

The commands provided for this docker image are for convenience only - they are not required. You can also use docker directly with this project.

The `docker-build` file is used to create a default container for this project. `docker build` can also be called directly (read the `docker-build` script for inspiration). For example:

```shell
# Build gatsd docker container with tag [tag-name] to current directory
docker build -f gatsd/Dockerfile -t [tag-name] .

# Ex:
docker build -f gatsd/Dockerfile -t mysite --build-arg SITE_NAME=mysite --build-arg GATSBY_PORT=8000 .
```

The `docker run` command can also be explicitly invoked (read the `run` script for inspiration). For example:

````shell
docker run -it -p [port] -u [user] -v [volume] [tag-name] [arguments]

# Ex: 
docker run \
	-it \
	-p 8000:8000 \
	-u $(id -u):$(id -g) \
	-v $(pwd):/mysite \
	-w /mysite \
	mysite \
	gatsby develop -H 0.0.0.0
```