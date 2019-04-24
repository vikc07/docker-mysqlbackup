![Docker Cloud Automated build](https://img.shields.io/docker/cloud/automated/vikramchauhan/ffmpeg.svg) ![Docker Cloud Build Status](https://img.shields.io/docker/cloud/build/vikramchauhan/ffmpeg.svg)
# docker-mysqlbackup
Dockerized mysqlbackup

## Instructions
Pull the image
    
    docker pull vikramchauhan/mysqlbackup

First time run
    
    docker run -it -v /dir/config:/config vikramchauhan/mysqlbackup bash
    
Once inside the container, run the following command
    
    cp /app/mysqlbackup/cfg_orig/sample_mysqlbackup.json /config/mysqlbackup.json
    
Exit

Edit the file `/dir/config/mysqlbackup.json`
    
    nano /dir/config/mysqlbackup.json
    
Run the following command for subsequent runs    
    
    docker run -it -v /dir/config:/config -v /dir/output:/output vikramchauhan/mysqlbackup "python 
    /app/mysqlbackup/mysqlbackup.py"
   
    