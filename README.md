# ahoy-iobroker-influx-grafana
AHOY-DTU (https://github.com/grindylow/ahoy) delivers power metrics from inverter via MQTT to ioBroker. Your ioBroker writes this data to InfluxDB. The Grafana dashboard consumes data from InfluxDB. This is the successor of the former solution https://github.com/c1328/ahoy-mosquitto-telegraf-influx-grafana

## Architecture:
(Hoymiles-)Inverter -> Ahoy-DTU -> ioBroker (MQTT Broker/Client Adapter) -> InfluxDB -> Grafana

Warning: The connection Ahoy-DTU -> ioBroker (MQTT Broker/Client Adapter) is currently insecure (no SSL available).

**Dashboard content are three sections:**
1. Overview: Main power metrics from Ahoy-DTU, Shelly power consumption, Cloudiness (forecast), daily production of last month
2. Details: Some inverter metrics like power limit, irridation details of your panels, weekly production of last 20 weeks
3. Balance: Ahoy-DTU power vs. Shelly power consumtion


## Prerequisites/Installation:
* Ahoy-DTU with configured MQTT broker/server (topic will be "ahoy-dtu" and name of inverter 0 will be "Hoymiles1")
* ioBroker has MQTT Broker/Client Adapter installed and instanced with default settings (listening on port 1883; SSL is not supported by Ahoy-DTU yet)
* with the first connection og Ahoy-DTU to ioBroker all objects are created as shown: <img width="793" alt="image" src="https://user-images.githubusercontent.com/112856305/214140268-28008102-92be-45ea-9ff3-3d1d1c36e1e6.png">
* if you want to set power limits via your ioBroker later you have to create "ctrl" datapoints manually with MQTT explorer (http://mqtt-explorer.com), I only created the nonpersistent ones: <img width="738" alt="image" src="https://user-images.githubusercontent.com/112856305/214140843-5a8fd3dc-4549-4a01-a401-8266bcf3d96f.png">
* Please check https://github.com/lumapu/ahoy/blob/main/User_Manual.md accordingly and how to control your inverter via MQTT messages.
* ensure having an influxDB (version 1.x) connected to your ioBroker environment.
* choose your desired datapoints to be stored in a influxDB by click on the custom settings wheel at the right and activate influxDB: <img width="809" alt="image" src="https://user-images.githubusercontent.com/112856305/214141448-92813295-61c5-49fe-ac83-59cf1b7ce8c4.png">
* ensure that you setup the same influxDB as data-source in your Grafana installation.
* import the JSON file of this repo.... Important: This JSON contains my individual dashboard. It might be needed to adjust it to your needs. There are also some "historical" queries because I switched from a _Inverter -> Ahoy-DTU -> MQTT (mosquitto) -> Telegraf -> InfluxDB -> Grafana_ architecture to the one with ioBroker, plesae checkout here the former solution: https://github.com/c1328/ahoy-mosquitto-telegraf-influx-grafana


Enjoy dashboard and producing energy (dashboard is currenly configured for one inverter and two panels east/west - please adjust to your needs)

Dashboard variables:
* kwhPrice: Define your individual price of a kWh to calculate your potential daily savings - in case you consumed all of your produced energy.
* ahoyTopic: currenly not in use
* ahoyInvertername: currenly not in use


### Option "weather":
I added some weather metrics to the dashboard (e.g. cloudiness of the region my PV/inverter is located). To get the weather metrics working:
* Get an API key from https://openweathermap.org
* add and configure the openweathermap.org adapter to you ioBroker
* choose desired metrics and write them to your influxDB, e.g. "clouds": ![image](https://user-images.githubusercontent.com/112856305/214143783-54f45b39-440f-49d2-a16d-6ecb37b325f1.png)

### Option "power consumption":
I added some shelly metrics to the dashboard (e.g. power) to have a balance chart that shows production vs. consumption. I have only some shelly plugs to measure my consumption but thats fine so far. If you have any better data source (Shelly EM3 or direct access to your "Stromzähler" please change accordingly):
* add and configure the Shelly adapter to you ioBroker
* choose desired metrics and write them to your influxDB, e.g. "Power"
* configure your transform of the consumption panel: <img width="748" alt="image" src="https://user-images.githubusercontent.com/112856305/214150172-04008fa0-0042-4270-8528-fed43083a336.png">



**Feel free to feedback!**
