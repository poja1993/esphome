# esphome
Collection of configuration files for ESPHome

## water_meter.yaml
Use a tracking sensor (i.e. KY-033) to monitor my water meter at home.  
The water meter in my appartment has a rotating disk that is half black and half white.  
The sensor is able to detect the difference and sends the status on change via MQTT to Home Assistant.

In Home Assistant I have the following setup:
* binary_sensor.water_meter_input: MQTT Binary Sensor from ESP8266
* counter.water_meter: Counter (with initial value) I'm using for tracking the overall quantity displayed on the gauge
* Automation: it increases the counter when the binary sensor is reporting an ON state
* Templates to track water usage in m3 and l:
```yaml
  - platform: template
    water_usage_m3:
      friendly_name: "Water Usage"
      unit_of_measurement: 'm³'
      value_template: >
        "{{ ( states('counter.water_meter') | float / 1000 ) }}"
    water_usage_l:
      friendly_name: "Water Usage"
      unit_of_measurement: 'l'
      value_template: >
        "{{ states('counter.water_meter') }}"
```
* Utility helper to track daily/monthly consumption
