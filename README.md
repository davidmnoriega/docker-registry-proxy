# Docker Registry Proxy Cache

This vagrant runs a Docker registry proxy cache for local development uses. So long as you don't
destroy the vagrant, the cache will continue to persist.

## Requirements

* [Vagrant](https://www.vagrantup.com)
* [vagrant-hostmanager](https://github.com/smdahlen/vagrant-hostmanager) plugin

## Usage

Simply use `vagrant up` to bring the vagrant up. When you no longer need it running, use `vagrant
suspend` or `vagrant halt` to suspend to disk/shut down the vagrant without losing data. Using
`vagrant destroy` will destroy the cache.

With the vagrant running, then configure your docker daemon to use it as a registry mirror.
Configuring your docker daemon to use the mirror is out of scope, but here is an example using
`docker-machine`:

    docker-machine create -d virtualbox --virtualbox-host-dns-resolver --engine-registry-mirror http://registry.local default

Use of `--virtualbox-host-dns-resolver` is required so the docker-machine can resolve the registry
mirror name from within the VM

## Benchmark

This is a simple test using `time`, take it for what its worth

Before:

    $ time docker pull registry
    Using default tag: latest
    latest: Pulling from library/registry
    ...
    Status: Downloaded newer image for registry:latest

    real	0m57.389s
    user	0m0.267s
    sys	0m0.075s

After:

    $ time docker pull registry
    Using default tag: latest
    latest: Pulling from library/registry
    ...
    Status: Downloaded newer image for registry:latest

    real	0m24.382s
    user	0m0.256s
    sys	0m0.062s

## Acknowledgements

Inspired by [How to Set Up a Registry Proxy Cache with Docker Open Source
Registry](https://blog.docker.com/2015/10/registry-proxy-cache-docker-open-source/) blog post from
Docker.
