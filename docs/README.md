> :fr: French version [here](./README.fr.md)

# Development container images

In this repository, I'll store and share some 
custom images I make to develop locally on containers

The goal is to enable development without installing
extra software -- just bind the container to 
directories on your filesystem and use a container 
engine like
[`docker`](https://www.docker.com/),
[`podman`](https://podman.io/)
or any other one.

Even though none of them are named `Dockerfile`, all 
the files in this repository (except for the scripts)
are valid `Dockerfile`s

## How to run the images

You can build the images by running (I'll be using 
docker for common understanding, however it works 
the exact same with podman):

```bash
docker build -t [your_image_name] -f path_to_dockerfile .
```

Now you got an image tagged `your_image_name` that you
can use to create a development container.
You will then be able to run that container with:

```bash
docker run -it [your_other_options] --mount type=bind,src=[your_src_dir],target=[your_target_dir]
```

You'll now be able to edit your files using your preferred editor (mine is vim, sorry not sorry).
You'll then be able to either rebuild your project, run your favorite interpreter, launch your 
server or anything on the container

> :warning: If you are developing a project that should run on a server you'll likely have to 
> forward some ports to be able to make requests to that server

## The `base` image

The base image is the one I'll use to build all the subsequent layers.
Basically I know that in all my developments containers I'll need *at least*
the following tools:
- [curl](https://curl.se/) to retrieve urls *Too lazy to learn wget*
- [vim](https://www.vim.org/) for editing *Did I mention that I use vim?*
- [less](http://www.greenwoodsoftware.com/less/) for easy file viewing *Less is way more than [more](https://www.man7.org/linux/man-pages/man1/more.1.html)*
- [tmux](https://github.com/tmux/tmux/wiki) for multiplexing *like inception, but with windows*
- [man](https://www.man7.org/linux/man-pages/man1/man.1.html) *The only* ***real*** *man*
- [git](https://git-scm.com/) *Because I also commit from my containers*

So I install them in my base image and I just have to build my other images using that one

## Instantiating containers from the base

If you're not willing to build the base image yourself, you can run:

```bash
docker pull charystag/development_base
```

which will pull the image for you to use in subsequent images
or to run containers from

## Images list

- `base` which contains `curl`, `vim`, `less`, `man`
- `python_base` [based on: `base`] which contains only the python interpreter
- `gcloud_cli` [based on: `python_base`] which contains the google cloud cli

## Note for non `x86_*` architectures

You'll likely be unable to build the subsequent images if you're not running
on an `arm` processor. So you have to do the following, for each image you'd
like to build

```bash
docker build -t [you_tag] -f base .
```
and then for each image, replace `charystag/base:latest` with 
`localhost/[your_tag]` before manually rebuilding the image
