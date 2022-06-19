# HA-tomorrow.io-Pollen
Get information about pollen in the air at your current location from tomorrow.io

## API Key (secret key)
Got to [tomorrow.io](https://www.tomorrow.io/), create an account and get your secret key

## Location (lat,lon)
Find out your location you want to monitor on pollen

## Test
Test the tomorrow.io service with

```https://api.tomorrow.io/v4/timelines?location=<lat>,<lon>&timesteps=current&fields=treeIndex,grassIndex,weedIndex&apikey=<apikey>```

Replace the `<lat>`, `<lon>`, and `<apikey>` with your values. 

You should get something like:

```{"data":{"timelines":[{"timestep":"current","endTime":"2022-06-19T19:12:00Z","startTime":"2022-06-19T19:12:00Z","intervals":[{"startTime":"2022-06-19T19:12:00Z","values":{"grassIndex":2,"treeIndex":1,"weedIndex":0}}]}]}}```

## Implementation

### secrets.yaml
If above test gives you the correct result, then create add an entry to `/config/secrets.yaml`:

```tomorrow-api-pollen: https://api.tomorrow.io/v4/timelines?location=<lat>,<lon>&timesteps=current&fields=treeIndex,grassIndex,weedIndex&apikey=<apikey>```

See [secrets.yaml](secrets.yaml) as an example.

Replace the `<lat>`, `<lon>`, and `<apikey>` with your values as done [Test](#test).

### tomorrow_pollen.yaml
Create the `/config/sensors/tomorrow_pollen.yaml` or add these lines to `configuration.yaml`:

```
- platform: rest
  name: Pollen
  scan_interval: 600
  resource: !secret tomorrow-api-pollen
  json_attributes_path: "$.data.timelines[0].intervals[0].values"
  json_attributes:
    - treeIndex
    - weedIndex
    - grassIndex
  value_template: >-
    {{ value_json.message }}

- platform: template
  sensors:
  
    pollen_tree:
      friendly_name: "Pollen - Bomen"
      value_template: '{{ int(states.sensor.pollen.attributes["treeIndex"], default=0) }}'

    pollen_weed:
      friendly_name: "Pollen - Kruiden"
      value_template: '{{ int(states.sensor.pollen.attributes["weedIndex"], default=0) }}'

    pollen_grass:
      friendly_name: "Pollen - Grassen"
      value_template: '{{ int(states.sensor.pollen.attributes["grassIndex"], default=0) }}'
```
