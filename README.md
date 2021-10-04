# bsd-amd64-filebeat
pfsense / freebsd binaries and configuration files for using the pfsense as an network sensor for the Elastic Stack. 

#### Install

WARNING: take a config snapshot of your pfsense configuration and make sure you can use it before proceeding.

Download this project as a zip file.

Upload it to pfsense by navigating to Diagnostics->Command Prompt->Upload file in the pfsense admin console.

ssh into the pfsense device. Then execute:

    cd /tmp
    unzip bsd-amd64-filebeat-main.zip
    cd bsd-amd64-filebeat-main
    cp ./filebeat /usr/local/sbin/filebeat
    cp ./filebeat.yml /usr/local/etc/filebeat.yml
    mkdir /var/db/beats/filebeat/
    cp -rvp var_lib/filebeat/ /var/db/beats/filebeat/



