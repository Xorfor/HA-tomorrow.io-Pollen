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
Create the `/config/sensors/tomorrow_pollen.yaml` or add these lines to `configuration.yaml` as shown in: [tomorrow_pollen.yaml](/sensors/tomorrow_pollen.yaml)

## Dashboard
From [tomorrow.io](https://www.tomorrow.io/) you will get a pollen index from 1 to 5. Data can be presented on your dashboard. Look at [dashboard.yaml](dashboard.yaml) for an example.
