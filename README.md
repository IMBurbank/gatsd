# GatsD - The GatsbyJS Project Dockerizer

Develop any GatsbyJS project without the need for dependency management on your local machine - beyond a working installation of Docker. 

## How To Install

### New Project

1.  Create directory for new site and enter the new directory.

```
mkdir gatsby-site
cd gatsby-site
```

2.  Download gatsd directory into new site.

```
curl -L https://api.github.com/repos/imburbank/gatsd/gatsd/tarball | tar xz
```

3. Build GatsD Docker container.
```
./gatsd/docker-build
```

The project directory will be accessible from inside the container and any changes made to the project inside the container or on the local host will be visible to both.

### Existing Project

TODO

## Using GatsD

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

-   `gatsd serve` -- Will start a local HTML server to serve the production site locally at `http://localhost:8000.

-   `gatsd stage` -- GatsD will invoke `gatsd build` and `gatsd serve` to stage a local production build of your site.

### From Local Host Machine

Commands can be called from the local project directory using shortcuts to standard Gatsd commands:
```
./gatsd/[gatsd command]

# Ex:
./gatsd/stage
```

Or sending commands directly into the container with `run`
```
./gatsd/run [any command]

# Example running gatsd command in container without shortcut:
./gatsd/run gatsd stage

# Example of running any other command in container
./gatsd/run ls -a
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
ls -a

whoami

gatsby develop -H 0.0.0.0
```