# docker-image-composer

This repository contains the Docker image used to run Composer for a
specific PHP version.

## Usage

To run Composer you should make sure you map a volume to the /data directory within the container:

```
docker run \
    --rm \
    -it \
    -v $(pwd):/data \
    chesszebra/composer:php7.0
```

### Caching

It's often a good idea to map your home directory to the container because Composer caches the packages that it 
downloads. This will speed up future installations:

```
docker run \
    --rm \
    -it \
    -v $(pwd):/data \
    -v ~/.composer:/home/php/.composer 
    chesszebra/composer:php7.0
```

### SSH Agent Forwarding

Some packages are installed via Git and require your SSH identity. We can forward the SSH agent easily:

```
docker run \
    --rm \
    -it \
    -v $(pwd):/data \
    -v $HOME/.ssh:/home/php/.ssh/ \
    -v $SSH_AUTH_SOCK:/ssh-agent \
    -e SSH_AUTH_SOCK=/ssh-agent \
    chesszebra/composer:php7.0
```
