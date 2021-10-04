# bsd-amd64-filebeat
pfsense / freebsd binaries and configuration files for using the pfsense as an network sensor for the Elastic Stack. 

WARNING: Please take a config snapshot of your pfsense configuration and make sure you can use it before proceeding.


#### Prerequisites
* A pfsense device
* ssh and web admin console access to the pfsense.
* An Elastic Stack ([cloud](https://cloud.elastic.co/) or self hosted) instance that can be reached by the pfsense.
* Using the [package manager](https://docs.netgate.com/pfsense/en/latest/packages/manager.html) to install [suricata](https://docs.netgate.com/pfsense/en/latest/packages/manager.html) and [zeek](https://github.com/shadonet/pfSense-pkg-zeek).

#### Install


Download this project as a zip file.

Upload it to pfsense by navigating to Diagnostics->Command Prompt->Upload file in the pfsense admin console.

ssh into the pfsense device (e.g. ssh admin@10.1.1.1). select option 8 for console access. Then execute:

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
    
    
#### Configuration

Customize the filebeat.yml file with the Elatic Stack authentication details.

    vi /usr/local/etc/filebeat.yml 
    
    # The kibana and elasticsearch sections...
    output.elasticsearch:
      hosts: ['https://yourElasticIp.com:9200']
      ssl.verification_mode: "none"
      username: elastic
      password: "censored#ChangeToYours"
      pipeline: geoip-info
      
      
Test the configuration and output.

    #$ filebeat -path.home /var/db/beats/filebeat -path.config /usr/local/etc test config
    
    results:
    Config OK
    
    #$ filebeat -path.home /var/db/beats/filebeat -path.config /usr/local/etc test output
    
    results:
    elasticsearch: https://10.3.3.102:30036...
      parse url... OK
      connection...
        parse host... OK
        dns lookup... OK
        addresses: 10.3.3.102
        dial up... OK
      TLS...
        security... WARN server's certificate chain verification is disabled
        handshake... OK
        TLS version: TLSv1.3
        dial up... OK
      talk to server... OK
      version: 7.15.0

Run the setup command.

    #$ filebeat -path.home /var/db/beats/filebeat -path.config /usr/local/etc setup
    
    
 Start the service.
 
   service filebeat start
   

In Kibana->Security->Overview filebeat network events should appear.






