A Simple Docker image to automate mounting a remote CIFS share into the Docker host.

### Usage

```
docker run -d \
    --privileged --pid=host \
    --restart=unless-stopped \
    -e SERVER=//server/cifsdir  \
    -e MOUNT=/mnt/folder \
    -e OPTIONS=username=user,password=Password,file_mode=0777,dir_mode=0777 \
    thisrepo
```

- SERVER : the remote CIFS share
- MOUNT : local host folder used for the mounting

and then to use it inside a container
```
   docker run --rm -v /mnt/folder:/data alpine ls /data
```
It support all Docker platforms that use the Moby Linux.<br>
  - Docker for Mac (untested)<br>
  - Docker for AWS<br/>
  - Docker for Windows<br/>
  - Should work on any Alpine docker host


### Here is how it works:<br/>
  nsenter to access the host namespace<br>
  mount the CIFS on the hostusing the -e MOUNT env from the run command<br>
  inotifywait to output logs for any folder changes
  
### NOTES: 
  I forked this from [vipconsoluting](https://github.com/vipconsult/dockerfiles/tree/master/moby-nfs-mount) in order to make a Windows client happy with a Synology NAS
