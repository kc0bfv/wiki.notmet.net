---
title: "Docker Cleanup"
date: 2021-10-24T11:41:00-05:00
draft: false
---

# Cleaning Up Docker

I've been using Docker for development and deployment recently, and while it's great there are a few problems with it - ok, a few problems with me.  I'm bad at it still.  Practice makes perfect.  So I make a lot of mistakes and leave a lot of junk behind.  Here's how I'm cleaning it up.

## Remove Stopped Containers

`sudo docker rm $(sudo docker ps -a -f status=exited -q)`

`docker ps` lists containers, and `-q` reduces the output to only the container IDs (after they've been filtered, in this case).

## Remove Orphaned Images

`docker image prune`

Once the containers no longer exist, now you've got a bunch of images that aren't needed anymore.  Any that aren't tagged or tied to a container will get pruned.

## Some More Things

It's possible to prune other resources...  https://www.digitalocean.com/community/tutorials/how-to-remove-docker-images-containers-and-volumes