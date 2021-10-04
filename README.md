# bsd-amd64-filebeat
pfsense / freebsd binaries and configuration files for using the pfsense as an network sensor for the Elastic Stack. 

WARNING: Please take a config snapshot of your pfsense configuration and make sure you can use it before proceeding.


#### Prequisites
* A pfsense device
* ssh and web admin console access to the pfsense.
* An Elastic Stack ([cloud](https://cloud.elastic.co/) or self hosted) instance that can be reached by the pfsense.
* Using the [package manager](https://docs.netgate.com/pfsense/en/latest/packages/manager.html) to install [suricata](https://docs.netgate.com/pfsense/en/latest/packages/manager.html) and [zeek](https://github.com/shadonet/pfSense-pkg-zeek).

#### Install


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
    cp ./filebeat_service.sh /usr/local/etc/rc.d/filebeat
    cp ./filebeat_service.sh /usr/local/etc/rc.d/filebeat.sh
    echo "filebeat_enable=YES" >> /etc/rc.conf.local
    
 Customize your filebeat.yml file with your Elatic Stack authentication details.

    vi /usr/local/etc/filebeat.yml 
    
    output.elasticsearch:
      hosts: ['https://yourElasticIp.com:9200']
      ssl.verification_mode: "none"
      username: elastic
      password: "censored#ChangeToYours"
      pipeline: geoip-info




