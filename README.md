# Continuous Delivery with Kubernetes, CircleCI and Vault

> Work-in-Progress. Mostly for my own use right now.

Some DevOps fun and a bootstrap of minimal, appropriate devops for the languages I've used. The point of this is to exercise some good practices, make it easy to reuse and also document it all in a way that is easy to teach others.

If you are a software developer, don't get frustrated by resource allocation or a devops team. If you are a CTO, don't shy away from the appropriate setup because you think stuff like Kubernetes is too complex or you don't understand microservices.

Links to elaborate on:

* [Kubernetes](https://kubernetes.io/docs/home/) for provisioning and container management
  * [Docker](https://www.docker.com/) to containerize code plus libraries
* [Vault](https://www.vaultproject.io/) for secrets
* [Launch Darkly](https://launchdarkly.com/) for feature flags and having a constant ship train to live
* [GitHub](https://github.com/) for tracking software

## Docker

[Docker](https://docs.docker.com/engine/) let's you wrap up both code and dependencies and run them as a "container" on the same computer. It is a fast and easy way to reliably run different software on the same computer. It mainly works by resource isolation. Same kernel but mem usage, CPU, threads, can't interfere with each other. Great summary on [wikipedia](https://en.wikipedia.org/wiki/Docker_(software)) and [history here](https://blog.aquasec.com/a-brief-history-of-containers-from-1970s-chroot-to-docker-2016) since this idea is an old one.

In practice, you make a file named `Dockerfile` that describes a base Linux OS plus stuff to install or configure. You use `docker` commands to build images that can be deployed an run. [Docker hub](https://hub.docker.com/) has many pre-made images exist for common stuff like dbs, message brokers, etc. It works on OSX (i.e. your Macbook Pro) by running Linux in a VM locally then using that kernel for Docker images.

Why do this?

* It avoids [dependency hell](https://en.wikipedia.org/wiki/Dependency_hell). You don't have to care if two parts of the software rely different packages. You also don't have to care if installing some needed binary messes with the OS.

* You don't have to code everything in exactly one language. Nor in one really big application. Nor get it perfect the first time around. Microservices are nice. Scope something to the size that can be rewritten by one or two people sub-week. It is then no big deal to prototype or let folks try newer tools and languages.

* It makes it easy to package up an entire software project in one place, including as a simple tarball. Key to making sure you can both test and recreate the exact setup needed to run software.

Below are some examples of usage.

```
# make a Docker image based on the Dockerfile (see docker/hello-node)
cd docker/hello-node
docker build -t hello-node/v1 .

# check to see that `hello-node/v1` exists -- a linux server with node.js installed on it
docker images
# REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
# hello-node/v1       latest              f35a041887c9        27 minutes ago      655MB

# run the node server, which will appear on http://localhost:8080
docker run --rm hello-node/v1 node sever.js

# (optional) attach to see stdout for a running container
docker container ls
docker container attach 4bad9a6e7525 # container's id

# drop the image from your local store when it is no longer needed
docker rmi f35a041887c9
```
