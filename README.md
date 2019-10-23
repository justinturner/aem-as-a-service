# How-to Setup AEM As A Service on Linux (CentOS and Ubuntu)
Details on how-to setup AEM as a Service on Linux. Used CentOS 7 and Ubuntu 17.04 as an example.

## Pre-requisites
1. AEM Installed on your server. Copy the path of the install (e.g: /mnt/crx)
2. Java (atleast JRE) installed (To test if java is installed and the version, run this command `java -version`) 
3. Start AEM (e.g `java -jar cq-quickstart-author-p4502.jar`) once. This will generate all the necessary folders, especially **/mnt/crx/crx-quickstart/bin** that is required by the scripts.
4. Create a user who will have access to the service. (e.g: aem)

## Step-by-step guide
1. You will need root access
2. Create user aem
   * `sudo adduser --system --home /mnt/crx --disabled-login --group aem`   
   * Test the user 
     * `sudo su - aem -s /bin/bash -c "ls -la"`
     * `sudo su - aem -s /bin/bash -c "ls -la /root"` [This is accessible by root only]
2. Create these 2 files (Get the contents of the files based on the OS. Under /centos or /ubutnu in this project)
   * aem
   * aem.service
3. Open `aem` script file and update the below
   * AEM_ROOT (e.g: `/mnt/crx` is the root, where `/mnt/crx/crx-quickstart` is the full path)
   * AEM_USER (e.g: `aem`) 
4. SCP these files to the server
   * Copy `aem` to `/usr/bin/aem`
     * Example: From terminal on your desktop `$ scp <filename> user@1.1.1.1:/usr/bin/aem`
   * Copy `aem.service` to `/etc/systemd/system/aem.service`
     * Example: From terminal on your desktop `$ scp <filename> user@1.1.1.1:/etc/systemd/system/aem.service`
5. SSH to your server
   * `ssh user@1.1.1.1`
6. Give permissions to the files
   * `sudo chmod u+rwx /usr/bin/aem`
   * `sudo chmod u+rwx /etc/systemd/system/aem.service`
7. Update 
   * `cd /etc/systemd/system`
   * `sudo systemctl enable aem.service`
8. You can restart the server or run the below commands to start AEM. Make sure you run **Pre-requisite Step 2** before running this command.

## Commands to START, RESTART and STOP AEM
### Centos
* Start AEM - `sudo service aem start`
* Restart AEM - `sudo service aem restart`
* Stop AEM - `sudo service aem stop`
* Check Status of the service `sudo service aem status`

### Ubuntu
* Start AEM - `sudo systemctl start aem`
* Restart AEM - `sudo systemctl restart aem`
* Stop AEM - `sudo systemctlstop aem`
* Check Status of AEM `sudo systemctl status aem`

## Test if AEM is running
There are several ways we can test if AEM is running with the above commands
1. Run the command `ps -ef | grep java` , you should see something like this
   * `root      12958      1  5 20:39 ?        00:02:41 java -server -Xmx1024m -XX:MaxPermSize=256M -Djava.awt.headless=true -Dsling.run.modes=author,crx3,crx3tar -Djava.locale.providers=CLDR,JRE,SPI -jar crx-quickstart/app/cq-quickstart-6.4.0-standalone-quickstart.jar start -c crx-quickstart -i launchpad -p 4502 -Dsling.properties=conf/sling.properties
`
2. Test AEM in browser `http://<vm-ip-address>:4502` 

## Troubleshooting
If the command shows `active` and still AEM does not load (On a browser `http://localhost:4502`) then check the AEM logs
* Path for the log file: `/<aem-folder>/crx-quickstart/logs/stdout.log` 

## Notes
1. The example above was tested on CentOS 7 and Ubuntu 17.04
2. AEM 6.3 version was used. Although the above process should work for AEM 6.x
