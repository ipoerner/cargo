# Cargo

A few scripts which assist with the management of custom build environments
using Docker.

Implements rudimentary user-mapping between the host system and the container,
as well as SSH agent forwarding for simplified SSH key management.

## <a name="usage"></a>Usage

    bin/cargo-create <base-image>

Creates a local Cargo base image to use as a base for your projects. These base
images just add some custom features on top of existing public base images.

You'll find the corresponding Dockerfiles under `base/Dockerfile.*`, where the
file extension identifies the base image name to use.

Building a Cargo base image is the first step before doing anything else.

    bin/cargo-load [<project-name>]

Creates a local Cargo image that represents your custom build environment. The
project name will be either derived automatically from your current working
directory, or you can supersede it with something else.

In any case, your current working directory must contain a Dockerfile that uses
an existing Cargo base image as a base.

    bin/cargo-deliver <project-name> [<args>]

Spawns a temporary Docker container which will bring you to your custom build
environment. This script must be invoked from within the projects source
repository, as it will use Git to determine the project root and mount this to
your home directory within the Docker container.

If your project is not a Git repository, it will use `pwd` to determine the
current path. Thus, you'll first need to explicitly change to the exact
directory you want to see mounted in the container.

Once the container is running, you'll end up with a login shell of the `cargo`
user. This user is basically a mapping of your host's user ID to a local user
who has sudo rights and uses the password 'cargo'. All files created locally by
the `cargo` user will be owned by your original user on the host system.

You can also append a list of additional arguments to pass to the `docker run`
command if you like – for instance, for adding additional mount points.

## <a name="dirstruct"></a>Directory Structure

* base

  Contains a number of Dockerfiles to build the Cargo base images from.

* bin

  Executables (also see [Usage](#usage)).

  * cargo-create

    Creates a new Cargo base image to use for your own project images.

  * cargo-load

    To build a new Docker image for a given project. Invoke this from within
    your project path.

  * cargo-deliver

    Spawns a new Docker container for a given project, mounts the project source
    tree to the your home directory within the Docker container and brings you
    to a login shell. Invoke this script from somewhere within your projects
    source repository.

* config

  Put links to your configuration files here (i.e. `~/.gitconfig`). These will
  be mirrored on your Docker containers home directory.

  You can also provide additional SSH keys which are not installed on your host
  system by putting them into `config/.ssh`.

* examples

  A few example projects and a template Dockerfile to use for your own projects.

## License

Please see [LICENSE](/LICENSE) for licensing details.

## Links

[https://www.docker.com](https://www.docker.com)
