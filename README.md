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

### Setting up the project

Checkout the project on your server. You will have to fill in the Docker compose variables:
- **$PWD**: docker compose does not handle relative paths well, as far as I know. The PWD is simply the path where you cloned the project. 
- **$PLEX_CLAIM_TOKEN**: this is the token plex gives you to identify your new plex server on its first connexion. Please follow the plex standard procedure. 
- **$YOUR_PLEX_URL**: the URL you will use to connect to your plex server (http or https).
- **$YOUR_AWS_BUCKET_NAME**: the name you gave to your aws bucket
- **$YOUR_AWS_REGION**: The region your bucket is in (example: eu-west-1)
- **$TRANSMISSION_USERNAME**: an custom username for transmission login
- **$TRANSMISSION_PASSWORD**: the transmission custom password 

Those are all the variables you need to change. For the two AWS access variables, we will source them when starting the project. 

### Running the project

When starting the project, you can run the Docker compose stack after sourcing the source_me.sh file.  
`source source_me.sh && docker-compose up -d`  

If you'd like to schedule it at server startup, simply put the command in a scheduling tool like cron.  

Note that if you stop the stack, you will need to manually unmount the aws fs before restarting it (`sudo unmount ${PWD}/data/aws_fs`).  

## Additional informations

- Note that the project is quite experimental. It works for me, but I do not guarantee anything! Watch your AWS consumption closely at first.  

- AWS is billing S3 clients based on the amount of requests made and the storage used, so the price should increase with downloads and usage. I think mine was around 5$, but I did not use it a lot. 

- Transmission is configured to use a local volume for the pending downloads. I did that to avoid additional costs when using the AWS fs, however that means you need to have the download disk space available when starting a download. You could shift that volume on the AWS S3, it might however increases the costs a lot. 

- When finishing a download, transmission needs to move it from the "current download" FS to the AWS one. It can take a few minutes, and transmission tends to be unavailable during that time (at least the web interface). 
