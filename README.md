# ShutItFile

## Automation for everyone

A ShutItFile is an extension of the Dockerfile concept.

In the same way that Dockerfiles were designed as a simple way to create Docker images, ShutItFiles are a simple way to create ShutIt scripts.

ShutItFiles can be used to automate tasks, or automate the building of composable Docker images.

Here's an annotated example TODO:

```
# Specify we are working in a simple bash shell (the default). Other options include a 'docker' container.
DELIVERY bash

# Log into my server
LOGIN ssh imiell@shutit.tk
# A password is required, so we prompt for one here
GET_PASSWORD Input your password

# Run the command 'whoami'
RUN whoami
# If the output of the previous RUN command is not as we expect (imiell), throw an error
ASSERT_OUTPUT imiell

# Remove any left-over file, sleep for 15 seconds, then create a file in the /tmp directory
RUN rm -f /tmp/event_complete && sleep 15 && touch /tmp/event_complete &
# List files in the /tmp directory
RUN ls /tmp
# Wait until the output of the previous command contains the filename created.
# It re-tries every 5 seconds
UNTIL ['.*event_complete.*']

# You can debug by creating a 'pause point'. This will give you a shell mid-run to examine the state of the system.
PAUSE_POINT You now have a shell to examine the situation
```

TODO: video of above


A cheat sheet for ShutIt commands is available [here][https://github.com/ianmiell/shutit-templates/blob/shutitfile/CheatSheet.md]

## Installation

```
sudo pip install shutit
```


## Examples 

0) Set up
1) Create file locally if it doesn't exist
2) Create Docker image, commit and push
3) Docker image with option to build dev tools in

## 0) Set up

Check out this repo and the examples ShutItFiles:

```
sudo pip install shutit
git clone https://github.com/ianmiell/shutitfile && cd shutitfile
```
                                                                                                                                             

## 1) Create file locally if it doesn't exist

This ShutItFile which creates the file /tmp/shutit_marker on the running host if it doesn't exist:

```
IF_NOT FILE_EXISTS /tmp/shutit_marker
RUN touch /tmp/shutit_marker
ENDIF
```

Create the above file 'Shutitfile' in a folder.

To run it, install ShutIt and run:

```
shutit skeleton --shutitfile examples/simple_bash.sf --accept
```

follow the instructions.

TODO: video running this twice

This all happened on the local host, and is the default 'bash' mode.

The next example does the same, but creates a simple Docker image.

## 2) Create Docker image, commit and push

```
DELIVERY docker
FROM debian
INSTALL nginx
COMMIT imiell/example_shutitfile:latest
PUSH imiell/example_shutitfile:latest
```

Your Docker credentials need to be in your ~/.shutit/config file, eg:
NB this file is created by ShutIt and stored with 0600 permissions (ie only you can view it)

```
[repository]
user:myusername
password:mypassword
email:you@example.com
```


TODO: video
