# ESPHome InfluxDB Cloud
Component &amp; example project for sending sensor data to cloud

## Component

Built on top of [esphome-influxdb](https://github.com/jabastien/esphome-influxdb/tree/use_http_request) and using [ESP8266 Influxdb](https://registry.platformio.org/libraries/tobiasschuerg/ESP8266%20Influxdb), this is a simplified component to send data to an InfluxDB 2 database. It will send all sensor updates in one batch, as fields of the same measurement.

You can use it in your project by pulling from Git or downloading the source - see the [air-quality.yaml](air-quality.yaml) file.

## Example Project

We will send data from an air quality sensor directly to the cloud, so we can avoid having to set up any local hub like Home Assistant. We will use the free versions of [InfluxDB Cloud Serverless](https://www.influxdata.com/products/influxdb-cloud/serverless/) and [Grafana Cloud](https://grafana.com/auth/sign-up?refCode=gr8Ln4qVGeMKBY6), which lets us retain data for 30 days.

### Hardware

Ingredients:
- NodeMCU-ESP32S - [documentation](https://docs.ai-thinker.com/en/esp32/boards/nodemcu_32s), [buy](https://www.aliexpress.com/item/1005001621773806.html) (select model `NodeMCU-ESP32S`)
- Sensirion SEN55 - [documentation](https://sensirion.com/products/catalog/SEN55/), [buy](https://www.soselectronic.com/en/products/sensirion/sen55-sdn-t-371697)
- JST GHR-06V-S connector - [buy](https://www.aliexpress.com/item/1005002288388835.html) (select model `Single Connector`, `6p`)

Wiring:

| SEN55 Pin | NodeMCU Pin  |
|-----------|--------------|
| 1 - VDD   | 5V - VIN 5V  |
| 2 - GND   | GND          |
| 3 - SDA   | P21 - GPIO21 |       
| 4 - SCL   | P22 - GPIO22 |
| 5 - SEL   | GND          |
| 6 - NC    | -            |

Cabling:

![cable][assets/cable.jpg]

### Firmware

See the [ESPHome Sen5x documentation](https://esphome.io/components/sensor/sen5x.html) for details on the sensor, and the [air-quality.yaml](air-quality.yaml) file for an example configuration.

### Cloud

Set up a free [InfluxDB Cloud Serverless](https://www.influxdata.com/products/influxdb-cloud/serverless/) account, create a bucket and a token with write access to the bucket. Configure the access parameters in your `secrets.yaml`, see the [secrets.template.yaml](secrets.template.yaml) for example.

After flashing and running the firmware, you should see a `Connected to InfluxDB` message in the logs and you should be able to query your data points with the Data Explorer on the InfluxDB Cloud portal.

Set up a free [Grafana Cloud](https://grafana.com/auth/sign-up?refCode=gr8Ln4qVGeMKBY6) account as well, and follow the [instructions](https://docs.influxdata.com/influxdb/cloud-serverless/process-data/visualize/grafana/) to connect them. I used the FlightSQL plugin, which had to be [installed first](https://grafana.com/docs/grafana/latest/administration/plugin-management/) in Grafana.    

### Dashboard

You can find the description of the sensor readings in the application notes from the sensor [documentation](https://sensirion.com/products/catalog/SEN55/) page. See an example Grafana dashboard at [air-quality.grafana.json](air-quality.grafana.json), or another one at [anotter-sensor-hub](https://github.com/Jana-Marie/anotter-sensor-hub).

![grafana][assets/grafana.png]
