# Kubectl with auto-completion in a docker container

The purpose of this tool is to have a fully portable `kubectl` command line tool with a few convinience utilities.
Quick testing of a cluster with well-known/customized `kubectl` setup.

## What is included

There are two images, one simple image with *bash* shell containing:

- `kubectl` - Kuberneted CLI v 1.17.2 with bash completion
- `k9s` - cluster monitoring tool
- popular tools: `curl, wget, git`
- useful aliases

And one fancy image for *zsh* with more tools preinstalled:

- `kubectl` - Kubernetes CLI v 1.17.2 with zsh completion
- `zsh-autosuggestions`
- `helm` - Kubernetes package manager
- `okteto` - development platform for Kubernetes applications
- `k9s` - cluster monitoring tool
- `krew` - Kubernetes plugin manager
- `tmux` based on highly customized, [great repo from samoshkin](https://github.com/samoshkin/tmux-config)
- popular tools: `curl, wget, git`
- useful aliases

## How to use

After running docker container, all the clusters running on the localhost should be available for `kubectl` command.

## Supported tags

- **zsh**
  docker pull piotrzan/kubectl-comp:zsh
  [Dockerfile](https://github.com/Piotr1215/kubectl-container/blob/master/zsh/Dockerfile)

- **bash**
  docker pull piotrzan/kubectl-comp
  [Dockerfile](https://github.com/Piotr1215/kubectl-container/blob/master/bash/Dockerfile)

## How the images are build

### Build image with bash shell

docker build --rm -f "Dockerfile" -t piotrzan/kubectl-comp "."

### Build image with zsh shell

docker build --rm -f "Dockerfile" -t piotrzan/kubectl-comp:zsh "."

## Convinient scripts to run the contianer

Use `run.ps1` or `run.sh` for windows or linux respectivels.
Alternatively use docker-compose.yaml, this works by mounting a volume on the $HOME/.kube folder on the host.

Another option is running `make` (defaults to content of run script). `Make` can be run from root directory and will run images depending on the tasks. Linux has `make` installed by default, for Windows please install first [Make for Windows](http://gnuwin32.sourceforge.net/packages/make.htm).

ZSH container will be ran by default.

### Linux Example

**Run contianer with passthrough to local network**
`docker run -d --network=host --name=kubectl-host --rm -it piotrzan/kubectl-comp`

**Generate raw config from kubeclt on localhost and copy the config to the container**
`kubeclt config view --raw > config
docker cp ./config kubectl-host:./root/.kube`

**Attach back to the contianer with kubeconfig file containing info about clusters running on localhost**
`docker attach kubectl-host`

## Extending the image

If you would like to add your own customization, you can easily do it and use `docker commit` to create your own version of the image.

`docker commit $(docker ps -aqf "name=kubectl-host") piotrzan/kubectl-comp:zsh` - this captures contianer kubectl-host as a new tag
`docker push piotrzan/kubectl-comp:zsh`
