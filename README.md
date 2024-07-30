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

# Splunkbase
This add-on is available to download and install direct from [splunkbase](https://splunkbase.splunk.com/app/7480)
