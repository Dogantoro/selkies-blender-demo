
# Selkies Blender Working Demo

Runs Blender on Selkies

These demo files are almost exactly the example files on the Selkies Repo, with some slight changes to run Blender instead.


## Running

These running instructions should work fine on both Windows and Linux; you do not need to create a virtual machine if on Windows! This will make networking more painful.

Clone repo into a local directory, and if on windows ensure that it uses LF endings instead of CRLF. Then run the following commands in powershell, replacing <name> and <container> with the appropiate strings.

```bash
  docker build -t <name>/<container> .
  docker run --name selkies -it --rm -p 8080:8080 -p 3478:3478 <name>/<container>
```

Test by going on localhost:8080
## Troubleshooting

### Common Problem 1: localhost:8080 connects fine in VM, but not on host machine

Port Forwarding is not going to cut it for Selkies GStreamer, which uses UDP on a broad range of ports. If on VirtualBox you will need to setup either a **Bridged Adapter** which works incredibly well when connected to a simple private wifi, but fails when connected to a more restricted wifi (e.g. University wifi). If you are on a restricted local network like that, you will need to set up a **Host-Only Adapter**. This was quite painful for me to set up, I will highly reccomend that if you get to this point, just don't run Selkies in a VM!

### Common Problem 2: ```INFO gave up: entrypoint entered FATAL state, too many start retries too quickly```

The problem here is (almost definitely) that the Ubuntu container is unable to execute the .sh scripts because of CRLF line endings. This repo has LF line endings on all the scripts, however, ```git clone``` tends to change line endings to the local standard, which is CRLF on Windows. So the easy fix here is to just change the line endings back to LF. The simplest way to do this is to open the files in VSCode, click CRLF on the bottom right, change to LF, and then save the file. Do this for both **entrypoint.sh** and **selkies-gstreamer-entrypoint.sh**! It shouldn't matter if **Dockerfile** or **supervisord.conf** have CRLF or LF line endings. 
