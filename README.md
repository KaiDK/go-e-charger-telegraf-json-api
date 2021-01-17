# go-e-charger-telegraf-json-api
Configuration and tweaks for importing data from the go-eCharger JSON API to InfluxDB using Telegraf 

## The following procedures (paths etc.!) are based on Raspberry OS (Rasbian) 10 Buster

Set Telegraf environment variables in /etc/default/telegraf
(this environment variables will be exported by the systemctl daemon via telegraf.service)

```
INFLUX_URL=your-influx-cloud-url
INFLUX_TOKEN=your-influx-cloud-token
INFLUX_ORGA=your-orga (Email adress)
INFLUX_BUCKET=your-influx-bucket
GOE_TOKEN=your-go-e-charger-cloud-token
GOE_IP=your-go-e-charger-local-ip
```

## Cloud API or local API: It is your choice

Using go-e Cloud API
- Limited to ~25000 requests per year (which is ~ every 30s)
- No local computer required. Telegraf can run from everywhere.
- Definition of environment variable GOE_TOKEN required (GOE_IP not required)
- see configuration file: `telegraf.cloud.conf`

Using go-eCharger local API
- Update every 5s possible
- Local computer (e.g. Raspi) required
- Definition of environment variable GOE_IP required (GOE_TOKEN not required)
- see configuration file: `telegraf.local.conf`

Copy the content from the configuration file of your choice (using Cloud API or local API) to your `/etc/telegraf/telegraf.conf`


## Store telegraf.conf in InfluxDB cloud

**Note:** If you do not want to store the telegraf.conf locally and you want to download it instead from influxdata, you need to modify the telegraf.service file (/lib/systemd/system/telegraf.service) accordingly:

```
ExecStart=/usr/bin/telegraf -config https://eu-central-1-1.aws.cloud2.influxdata.com/api/v2/telegrafs/abcdefghi
```

Do not forget to reload the systemctl daemon after modifying this files: `sudo systemctl daemon-reload`



For commandline debug purpose you can do
```bash
export INFLUX_URL=your-influx-cloud-url
export INFLUX_TOKEN=your-influx-cloud-token
export INFLUX_ORGA=your-orga (Email adress)
export INFLUX_BUCKET=your-influx-bucket
export GOE_TOKEN=your-go-e-charger-cloud-token
export GOE_IP=your-go-e-charger-local-ip

telegraf --config https://eu-central-1-1.aws.cloud2.influxdata.com/api/v2/telegrafs/abcdefghi --test
```
