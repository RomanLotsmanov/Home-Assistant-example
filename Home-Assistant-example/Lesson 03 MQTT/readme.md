# Настройка MQTT сервера/брокера

## Первый способ интеграции через configuration.yaml

   ```
   mqtt:
  sensor:
    - name: "CO2"
      object_id: air_monitor_bedroom_co2
      device_class: carbon_dioxide
      unit_of_measurement: "ppm"
      state_topic: "qingping/582D347046EF/up"
      value_template: >-
        {{ value_json.sensorData.0.co2.value
          if value_json.type=="17" else this.state }}
      availability:
        - topic: "qingping/582D347046EF/up"
          payload_available: "true"
          payload_not_available: "false"
          value_template: >-
            {{ "true" if value_json else "false" }}
      device:
        identifiers: ["582D347046EF"]
        name: "Air monitor"
        model: "CGS2"
        manufacturer: "qingping"
      unique_id: f0864ae1d3e66cdb5a29c894dd6a192f8ba528ec
    - name: "PM2.5"
      object_id: air_monitor_bedroom_pm25
      device_class: pm25
      state_topic: "qingping/582D347046EF/up"
      value_template: >-
        {{ value_json.sensorData.0.pm25.value
          if value_json.type=="17" else this.state }}
      unit_of_measurement: "µg/m³"
      availability:
        - topic: "qingping/582D347046EF/up"
          payload_available: "true"
          payload_not_available: "false"
          value_template: >-
            {{ "true" if value_json else "false" }}
      device:
        identifiers: ["582D347046EF"]
        name: "Air monitor"
        model: "CGS2"
        manufacturer: "qingping"
      unique_id: 0c4f328dd042cdfc17f01ec2f880e7dd5317750b
    - name: "PM10"
      object_id: air_monitor_bedroom_pm10
      device_class: pm10
      unit_of_measurement: "µg/m³"
      state_topic: "qingping/582D347046EF/up"
      value_template: >-
        {{ value_json.sensorData.0.pm10.value
          if value_json.type=="17" else this.state }}
      availability:
        - topic: "qingping/582D347046EF/up"
          payload_available: "true"
          payload_not_available: "false"
          value_template: >-
            {{ "true" if value_json else "false" }}
      device:
        identifiers: ["582D347046EF"]
        name: "Air monitor"
        model: "CGS2"
        manufacturer: "qingping"
      unique_id: 0e330464965310305e8df5187ba00e5a6b515471
    - name: "TVOC"
      object_id: air_monitor_bedroom_tvoc
      device_class: volatile_organic_compounds
      unit_of_measurement: "mg/m³"
      state_topic: "qingping/582D347046EF/up"
      value_template: >-
        {{ ((value_json.sensorData.0.tvoc_index.value * 0.023) + 0.124) | round(3)
          if value_json.type=="17" else this.state }}
      availability:
        - topic: "qingping/582D347046EF/up"
          payload_available: "true"
          payload_not_available: "false"
          value_template: >-
            {{ "true" if value_json else "false" }}
      device:
        identifiers: ["582D347046EF"]
        name: "Air monitor"
        model: "CGS2"
        manufacturer: "qingping"
      unique_id: 145ba4f594273ed8cfcf44f8df1983e3a229b0d1
    - name: "Humidity"
      object_id: air_monitor_bedroom_humidity
      device_class: humidity
      state_topic: "qingping/582D347046EF/up"
      value_template: >-
        {{ value_json.sensorData.0.humidity.value|round(2)
          if value_json.type=="17" else this.state }}
      unit_of_measurement: "%"
      availability:
        - topic: "qingping/582D347046EF/up"
          payload_available: "true"
          payload_not_available: "false"
          value_template: >-
            {{ "true" if value_json else "false" }}
      device:
        identifiers: ["582D347046EF"]
        name: "Air monitor"
        model: "CGS2"
        manufacturer: "qingping"
      unique_id: 5648fe503a9c4c7563643425de97f184a3282a66
    - object_id: air_monitor_bedroom_temperature
      name: "Temperature"
      device_class: temperature
      unit_of_measurement: "°C"
      state_topic: "qingping/582D347046EF/up"
      value_template: >-
        {{ value_json.sensorData.0.temperature.value|round(2)
            if value_json.type=="17" else this.state }}
      availability:
        - topic: "qingping/582D347046EF/up"
          payload_available: "true"
          payload_not_available: "false"
          value_template: >-
            {{ "true" if value_json else "false" }}
      device:
        identifiers: ["582D347046EF"]
        name: "Air monitor bedroom"
        model: "CGS2"
        manufacturer: "qingping"
    - object_id: air_monitor_bedroom_noise
      name: "Noise"
      device_class: sound_pressure
      unit_of_measurement: "dB"
      state_topic: "qingping/582D347046EF/up"
      value_template: >-
        {{ value_json.sensorData.0.noise.value|round(2)
            if value_json.type=="17" else this.state }}
      availability:
        - topic: "qingping/582D347046EF/up"
          payload_available: "true"
          payload_not_available: "false"
          value_template: >-
            {{ "true" if value_json else "false" }}
      device:
        identifiers: ["582D347046EF"]
        name: "Air monitor bedroom"
        model: "CGS2"
        manufacturer: "qingping"
      unique_id: eeac725e7c2bc220e8ef3602b7a506a9855a36c6
   ```


## Изменение частоты опроса датчика воздуха Qingping

Через MQTT Explorer перейти в Publish и послать на устройство команду

Топик выбрать qingping/{MAC}/down

```
{
  "id": 1,
  "need_ack": 1,
  "type": "17",
  "setting": {
    "report_interval": 15,
    "collect_interval": 15,
    "co2_sampling_interval": 15,
    "pm_sampling_interval": 15
  }
}  
```