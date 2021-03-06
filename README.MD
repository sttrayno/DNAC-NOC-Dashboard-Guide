# Network Operations Center Dashboard

## DNA-C Network Operations Centre Custom Dashboard

This lab is based from a blog by Adam Radford which walks through creating your own dashboard for DNA-C we'll walk through this process in a little more detail here https://blogs.cisco.com/developer/dna-center-noc-dashboard

With the availability of more programmatic interfaces and greater level of data available in controller based networking this starts to present even more opportunties for utilising telemetry data from the infrastructure. This lab will walk through how we can look to leverage some of that available data to create a simple network operations centre dashboard.

In this blog we'll use the TIG stack (Telegraf - the agent which connects to our custom python script, InfluxDB - time series database and Grafana - Visualisation and dashboards) to collect, store and display the network and device data from DNA-C.

Step 0 - Download code and configure TIG stack


###  Setup The Custom Script

The script is going to call DNAC API via the newly released SDK.

I am going to assume a directory /opt/telegraf.
If you want to install elsewhere you need to change the paths in the custom.conf file.

First (optional) step, create a vitualenv. This makes it less likely to clash with other python libraries in future.
Once the virtualenv is created, need to activate it.
```buildoutcfg
python3 -mvenv env3
source env3/bin/activate
```

Next clone the code.

```buildoutcfg
git clone https://github.com/CiscoDevNet/DNAC-NOC.git
```

Then install the  requirements (after upgrading pip). 
Older versions of pip may not install the requirements correctly.
```buildoutcfg
pip install -U pip
pip install -r DNAC-NOC/requirements.txt
```
### Credentials/Run the Script
By default the script uses the devnet sandbox.  If you want to change these you can edit the dnac_config.py file.
You are able to either edit the defaults, or use environment variables if you are familiar with them.

Finallly, test the script.  
Should see something like this.  
You can edit the dnac_config.py file to change the DNAC you are connecting to.  The default is a devnet sandbox.
```
$ ./DNAC-NOC/dnac_assurance.py 
{"Core.count": null, "totalcount": 14, "WIRELESS-client.count": 80, "WLC.count": null, 
"WIRED-client.count": 2, "AP.count": null, "WIRED-client.value": 100, "Access.count": null, 
"totalscore": 100, "WIRELESS-client.value": 25, "AP.score": 100, "Router.score": 100, 
"ALL-client.value": 27, "Router.count": null, 
"Access.score": 100, "Core.score": 100, "WLC.score": 100, "ALL-client.count": 82}
```
### Get Telegraf to use the custom script
Now install the telegraf custom script, restart telegraf.
```buildoutcfg
sudo cp DNAC-NOC/telegraf.d/custom.conf /etc/telegraf/telegraf.d/
sudo systemctl restart telegraf
```

### Import Dashboard Definition in Grafana
log into grafana, select + create -> import
![json](images/first.png?raw=true "Import JSON")

![json](images/2-json.png?raw=true "Import JSON")

Select the jsonfile grafana/dashboard.json
![json](images/3-file.png?raw=true "Import JSON")

Then add the dashboard
![json](images/importjson.png?raw=true "Import JSON")

This will give you an empty dashboard.
![json](images/first-dash.png?raw=true "First Dashbaord")

Soon you wil see updated data
![json](images/final.png?raw=true "First Dashbaord")



