# Influx CLI
## Installation
- **Linux**
  ```shell
  # amd64
  curl -O https://dl.influxdata.com/influxdb/releases/influxdb2-client-2.7.5-linux-amd64.tar.gz
  tar xvzf influxdb2-client-2.7.5-linux-amd64.tar.gz
  sudo cp ./influx /usr/local/bin/
  ```
- **Windows**  
  Download and install [Influx CLI](https://docs.influxdata.com/influxdb/v2/reference/cli/influx/?t=Windows#download-from-your-browser)

## Setup fresh InfluxDB Installation
```shell
influx setup \
  --host http://<HOST>:8086
  --org local \
  --bucket local \
  --username admin \
  --password 123456789 \
  --force
```

## Defining a connection
```shell
influx config create --config-name <CONFIG_FRIENDLY_NAME> \
  --host-url http://<IP>:8086 \
  --org <ORGANIZATION> \
  --token <TOKEN> \
  --active
```
This command creates a new entry in `~/.influxdbv2/configs` file.

## Usage
Switching between active conenctions - [`influx config`](https://docs.influxdata.com/influxdb/v2/reference/cli/influx/config/)
```shell
influx config CONFIG_NAME
```

## User accounts
Create user
```shell
influx user create \
  --name USERNAME \
  --password PASSWORD
```

Change user password
```shell
influx user password \
  --name USERNAME \
  --password PASSWORD
```

### Buckets
List buckets - [`influx bucket`](https://docs.influxdata.com/influxdb/v2/reference/cli/influx/bucket/)
```shell
influx bucket list --org my-org
```

Create a bucket
```shell
influx bucket create \
  --org my-org \
  --name my-bucket \
  --retention 1d
```

Delete a bucket
```shell
influx bucket delete --name bucket-name
influx bucket delete --id bucket-id
```

### Querying / manipulating data
We can use [Flux](https://docs.influxdata.com/flux/v0/) or [InfluxQL](https://docs.influxdata.com/influxdb/v1/query_language/) (SQL-like) to query data.

```shell
# InfluxQL
influx query --org my-org --bucket my-bucket 'SELECT * FROM my_measurement'
# Flux interactive (write query to stdin)
influx query
# Flux non-interactive
influx query --org my-org --bucket my-bucket '<flux query>'
```

Delete based on predicate
```shell
influx delete \
  --org ORGANIZATION
  --bucket BUCKET \
  --start '1970-01-01T00:00:00Z' \
  --stop $(date -u +"%Y-%m-%dT%H:%M:%SZ") \
  --predicate '_measurement="example-measurement" AND exampleTag="exampleTagValue"'
```

## Data replication
**Only write operations are replicated. Other data operations (like deletes or restores) are not replicated.**

1. Define remote - [`influx remote`](https://docs.influxdata.com/influxdb/v2/reference/cli/influx/remote/)
   ```shell
   influx remote create \
   --name REMOTE_NAME \
   --remote-url http://REMOTE_HOST:8086 \
   --remote-api-token TOKEN \
   --remote-org-id remote-org-name
   ```
2. Create *replication* - [`influx replication`](https://docs.influxdata.com/influxdb/v2/reference/cli/influx/replication/)
   ```shell
   influx replication create \
   --name REPLICATION_STREAM_NAME \
   --remote-id REMOTE_ID \
   --local-bucket-id LOCAL_BUCKET_ID \
   --remote-bucket REMOTE_BUCKET_NAME
   ```

Influx documentation [explains how to replicate downsampled data](https://docs.influxdata.com/influxdb/v2/write-data/replication/replicate-data/#replicate-downsampled-or-processed-data).

## Backup
**For creating and restoring backup, admin token is needed!**  

Create a backup
```shell
influx backup /path/to/backup
```

Restore backup
```shell
influx restore 
```
