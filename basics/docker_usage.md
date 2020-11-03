# Using Docker
Presenting an example of getting setup with Docker.

There are many detailed tutorials out there on doing this, and I'll be linking to resources I found especially useful. 

What I hope to contribute here is to fill in some of the gaps that I had from only doing
simple applications (e.g. following stackoverflow directions, or blog posts). 
It was only after completing some basic training that I really started to understand how images, containers, volumes 
and all the other objects in docker work together. This short intro is meant to share those learnings.

# Setup
I followed this well structured 
[guide](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-ubuntu-20-04) 
from the Digital Ocean folks to get Docker setup on my laptop. 
You can disregard the comments about doing it on Ubuntu server. 

Briefly, the steps are
- Prepare your system for installation (repos, pre-reqs)
- Install docker
- Validate

# Clean Up
As mentioned earlier, I had already been playing around with docker by following blog posts and other online resources like StackOverflow.
One such endeavour involved [PacketBeat](https://www.elastic.co/guide/en/beats/packetbeat/master/running-on-docker.html)
This resulted in me running Elasticsearch and other applications via docker containers. 
I ended up with a bunch of images, containers, and volumes being present... and I didn't really understand how they interacted. 

After completing the O'Reilly course, one of the most useful things I learned is how to clean up the docker objects on your system. 

To that extent, the most helpful new command I learned was the `prune` and `rm` actions. This will remove objects that are not being used. 
- `docker container prune`: This removes all stopped containers. **Be careful** with this... before running it, ensure you really do what to do this. 
Sometimes you may have stopped containers 
- `docker container rm <container id>`: Removes a specific container. Useful when you don't want to just `prune`.

# Images
Images are central to containerization. Yet, it wasn't obvious to me exactly what they were, and how containers related to them.

An image contains everything you need to create a container -- which is a running instance of an image (possibly with other runtime changes).
It's a read-only template specifying how to create that container, and it will create that same consistent container regardless of the underlying computing system (physical, virtual, OS, etc.). 
This independence is a big part of what makes Docker (and other container frameworks) so powerful. 

I found it helpful to think of Images in 2 ways:
1. The `Dockerfile`: The Dockerfile is a template that you express in plaintext. It defines what goes into the image.
Very similar to a playbook (e.g. for Ansible) if you've got experience with that. 
I also like to think of it as a recipe. It contains the ingredients and instructions for constructing a container. 
2. As a snapshot: If you've worked with Jupyter notebooks, then you may know that underneath the web UI, there's an XML representation of the notebook. That XML describes the state of the notebook.
Another analogy would be a snapshot of a Virtual Machine. It describes the state of the VM.
Similarly, you can think of a `docker image` as being a description of a container. 
This differs from the `Dockerfile` in that you can create a container, do things to it (train a model, install software, etc.) and then create an image from the current container state via `docker commit`

# Running Containers
I never fully understood the difference between `docker container run` and `docker container start`. 
The documentation is clear... I just never really dived into the details when following online instructions to just "get something working". 

Here's my key learnings:
- `docker run` will create a NEW container from an image and start it. This new container is distinct from any earlier ones you may have created.
- `docker start` will start an existing container.

I was in the habit of always doing `docker run` and not really getting why I had so many containers appearing under `docker container ps -a`

## Example of Running Containers
Create and start a container based on the `ubunut:latest` image called `ubuntu1` (see docs for `-itd` meaning, but they're commonly used when testing ideas/functionality)
```
$ docker run -itd --name ubuntu1 ubuntu:latest
Unable to find image 'ubuntu:latest' locally
latest: Pulling from library/ubuntu
6a5697faee43: Pull complete 
ba13d3bc422b: Pull complete 
a254829d9e55: Pull complete 
Digest: sha256:fff16eea1a8ae92867721d90c59a75652ea66d29c05294e6e2f898704bdb8cf1
Status: Downloaded newer image for ubuntu:latest
e65526c348667597e4d8d584073f5e86ae285639e187d2990721c923cd3da0c4
```
Next, we attach to it and create a file in this container:
```
$ docker attach ubuntu1
root@e65526c34866:/# echo "This is ubuntu1" > /tmp/msg.txt
root@e65526c34866:/# exit
```
Note that because we attached to the container, when we exited it stopped the container automatically. Use `docker exec` if you want to be able to access the shell in a running container and not stop it when you're done.

Now, let's start a new container based on the same image:
```
$ docker run -itd --name ubuntu2 ubuntu:latest
259fa16f3a2f9b71542bd1ba30f8f704fa5d6c36dca3a94ab3f3378d52f46367
```
You'll notice less output since things are now locally available to create the container from the image.

Let's see if that file is present in `/tmp/`
```
$ docker attach ubuntu2
root@259fa16f3a2f:/# ls /tmp/
root@259fa16f3a2f:/# exit
```
And we see no file. That makes sense because `ubuntu2` is a distinct container. It's isolated from `ubuntu1` container.
They're both based on the same image, but distinct, separate containers.

Let's start our existing `ubuntu1` container and check that the file is still there.
```
$ docker container start ubuntu1
ubuntu1
```
And attach to it:
```
$ docker attach ubuntu1
root@e65526c34866:/# cat /tmp/msg.txt 
This is ubuntu1
```
And indeed our file is still there. 

Last thing I do is clean things up so I don't have containers lying around that are not in use. 
In my case, I only have the 2 containers just created: `ubuntu1` and `ubuntu2`, so I just run a prune:
```
$ docker container prune
WARNING! This will remove all stopped containers.
Are you sure you want to continue? [y/N] y
Deleted Containers:
259fa16f3a2f9b71542bd1ba30f8f704fa5d6c36dca3a94ab3f3378d52f46367
e65526c348667597e4d8d584073f5e86ae285639e187d2990721c923cd3da0c4

Total reclaimed space: 109B
```
and I've got no containers remaining:
```
$ docker container ls -a
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES

```
