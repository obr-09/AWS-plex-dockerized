# AWS-plex-dockerized

This is a docker project for running a plex media server using an AWS S3 filesystem as storage. I made it because server storage is expensive. Keep in mind that the project is still rather experimental, although it works.  

## How it works

The project relies on three components or Docker images:
- **transmission**: torrent utility to directly download new stuff to your server (this one is optional if you just want plex + aws S3).  
- **plex**: plex media center official docker image.  
- **goofys**: this is a fuse utility which will mount your aws remote fs on your machine.  

Goofys will mount the AWS FS volume like it is on your filesystem, then transmission and plex will interact with it.  

## How to setup

### AWS

You will obviously need to setup the AWS S3 first.  

Once you get a bucket ready, setup the command line utility so you have a *~/.aws/credentials* file.  

### Using the project

Checkout the project on your server. You will have to fill in the Docker compose variables:
- *$PLEX_CLAIM*: this is the token plex gives you to identify your new plex server on its first connexion. Please follow the plex standard procedure. 
- *$ADVERTISE_IP*: the URL you will use to connect to your plex server (http or https).


