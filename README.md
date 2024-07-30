# iDelta Add-on for GivEnergy

GivEnergy are a UK based producer of battery storage systems for domestic and commercial use cases, including solar energy storage.

https://givenergy.co.uk

This add-on allows you to ingest data from the GivEnergy API, specifically the Get Latest System Data endpoint.  This endpoint returns the current readings from the inverter.  The inverter acts as the brains of the system, providing the basic use case of converting DC solar generated energy to AC for use in the home; routing power to the home, battery or grid as required.

* battery: charge percent, power output/input, temperature
* overall site power consumption
* grid: current, frequency, power, voltage
* inverter: frequency, voltage, power, temperature
* solar: power output, per array power/current/voltage
* inverter status

[GivEnergy API documentation](https://givenergy.cloud/docs/api/v1#inverter-data-GETinverter--inverter_serial_number--system-data-latest)

# UCC Framework
This add-on uses the Splunk [UCC Framework](https://splunk.github.io/addonfactory-ucc-generator/).  Once you clone or fork the code from this repository, use ucc-gen to [build](https://splunk.github.io/addonfactory-ucc-generator/quickstart/#ucc-gen-build) and [package](https://splunk.github.io/addonfactory-ucc-generator/quickstart/#ucc-gen-package) the Splunk app.  

Alternatively you can download and install the already packaged app from splunkbase. 

# Splunkbase
This add-on is available to download and install direct from [splunkbase](https://splunkbase.splunk.com/app/7480)

# Installation and Configuration
The add-on can be run from a standalone splunk server, a heavy forwarder or dedicated data ingestion server, an IDM (for Splunk Cloud Classic stacks) or direct on the search heads (Splunk Cloud Victoria).
## Installation
There are four ways to install the add-on:
1. From your Splunk server use the Apps > Find More Apps menu option and search for GivEnergy, then install
2. Download the add-on from Splunkbase and install onto your Splunk server using Apps > Manage Apps > Install app from file
3. Clone/Fork the code from this repo, build and package using ucc-gen, then install the package using method (2) above
4. Clone/Fork the code from this repo, build and then either copy (to splunk server etc/apps directory) or symlink the folder within your build directory
Option 1 is the easiest and is recommended if you want to try out the add-on.  Option 4 is useful if you plan to make code changes.
## Configuration
You need two things to run the add-on:
* a GivEnergy API Key
* the serial number of your inverter
### API Key and Serial Number
The API key can be generated using the [GivEnery Portal](https://givenergy.cloud).  Navigate to Account Settings > Manage Account Security > My API Keys.
The Serial Number can be viewed by choosing the My Inverter option in the portal menu.  The serial number is displayed to the top-right of the image of the inverter.  
### Add-on Account Setup
In Splunk Web go into the Add-on and access the Configuration > Accounts page.  Click Add, then supply a name for your site/account and the API key.
### Inputs Setup
From the Inputs tab, click on Create New Input.  Supply the values as indicated below:
Name: Give the input a name
Inverter Serial Number: provide the inverter serial number
Interval: choose how frequently you want to collect the readings, note that the values supplied by the API only change every 5 minutes so 300 is recommended
Account to use: select the account you setup in the previous section
# Using the data
As an example the following splunk search will generate a graph showing the power input/output to/from the battery, solar and grid:
```
index=givenergy
|timechart span=5m 
max(data.battery.power) as battery 
max(data.grid.power) as grid 
max(data.solar.power) as solar
```
The following search will return the current battery charge level (as a percentage):
```
index=givenergy
|stats latest(data.battery.percent) as battery_charge
```
# Troubleshooting
Check the [issues](https://github.com/srsplunk/idelta_addon_for_givenergy/issues) listed in this repository.
The monitoring dashboard within the add-on provides an overview of the add-on operations.
The following search will show the internal log for the add-on:
```
index=_internal source=*idelta_addon_for_givenergy*
```
Debug logging can be enabled via Configuration > Logging

You can check if the API call works, independently of Splunk and the add-on, using the curl command for the [Get Latest System Data API](https://givenergy.cloud/docs/api/v1#inverter-data-GETinverter--inverter_serial_number--system-data-latest)
You can add an issue to the repository if none of the above help.
