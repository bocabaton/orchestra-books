# Crete Grafana Dashboard

## Environment

Keyword     | Value             | Description
----        | ----              | ----
GRAFANA_USER    | admin         | Grafana User ID
GRAFANA_PW      | admin         | Grafana Password
DATABASE        | mydatabase    | Database name
GRAFANA         | localhost     | GRAFANA Serve Address

# Create Grafana Dashboard

~~~python
import requests
import json

user = '${GRAFANA_USER}'
pw = '${GRAFANA_PW}'
DATABASE = '${DATABASE}'
TITLE = "%s-dashboard" % DATABASE
DATASOURCE = "%s-influxdb" % DATABASE

body = {"dashboard":{
  "id": 0,
  "title": TITLE,
  "tags": [],
  "style": "dark",
  "timezone": "browser",
  "editable": True,
  "hideControls": False,
  "sharedCrosshair": False,
  "rows": [
    {
      "collapse": False,
      "editable": True,
      "height": "200px",
      "panels": [
        {
          "aliasColors": {},
          "bars": False,
          "datasource": DATASOURCE,
          "editable": True,
          "error": False,
          "fill": 1,
          "grid": {
            "threshold1": 0,
            "threshold1Color": "rgba(216, 200, 27, 0.27)",
            "threshold2": 0,
            "threshold2Color": "rgba(234, 112, 112, 0.22)"
          },
          "id": 13,
          "interval": "30s",
          "isNew": True,
          "legend": {
            "avg": False,
            "current": False,
            "max": False,
            "min": False,
            "show": False,
            "total": False,
            "values": False
          },
          "lines": True,
          "linewidth": 2,
          "links": [],
          "0PointMode": "connected",
          "percentage": False,
          "pointradius": 5,
          "points": False,
          "renderer": "flot",
          "seriesOverrides": [],
          "span": 4,
          "stack": False,
          "steppedLine": False,
          "targets": [
            {
              "dsType": "influxdb",
              "groupBy": [
                {
                  "params": [
                    "$interval"
                  ],
                  "type": "time"
                },
                {
                  "params": [
                    "0"
                  ],
                  "type": "fill"
                }
              ],
              "measurement": "cpu",
              "policy": "default",
              "query": "SELECT 100-mean(\"usage_idle\") FROM \"cpu\" WHERE \"cpu\" = 'cpu-total' AND $timeFilter GROUP BY time($interval) fill(0)",
              "rawQuery": True,
              "refId": "A",
              "resultFormat": "time_series",
              "select": [
                [
                  {
                    "params": [
                      "usage_idle"
                    ],
                    "type": "field"
                  },
                  {
                    "params": [],
                    "type": "mean"
                  }
                ]
              ],
              "tags": [
                {
                  "key": "cpu",
                  "operator": "=",
                  "value": "cpu-total"
                }
              ]
            }
          ],
          "timeFrom": 0,
          "timeShift": 0,
          "title": "CPU Usage",
          "tooltip": {
            "msResolution": False,
            "shared": True,
            "value_type": "cumulative",
            "sort": 0
          },
          "type": "graph",
          "xaxis": {
            "show": True
          },
          "yaxes": [
            {
              "format": "percent",
              "label": 0,
              "logBase": 1,
              "max": 100,
              "min": 0,
              "show": True
            },
            {
              "format": "short",
              "label": 0,
              "logBase": 1,
              "max": 0,
              "min": 0,
              "show": False
            }
          ]
        },
        {
          "aliasColors": {},
          "bars": False,
          "datasource": DATASOURCE,
          "editable": True,
          "error": False,
          "fill": 1,
          "grid": {
            "threshold1": 0,
            "threshold1Color": "rgba(216, 200, 27, 0.27)",
            "threshold2": 0,
            "threshold2Color": "rgba(234, 112, 112, 0.22)"
          },
          "id": 14,
          "isNew": True,
          "legend": {
            "avg": False,
            "current": False,
            "max": False,
            "min": False,
            "show": False,
            "total": False,
            "values": False
          },
          "lines": True,
          "linewidth": 2,
          "links": [],
          "0PointMode": "connected",
          "percentage": False,
          "pointradius": 5,
          "points": False,
          "renderer": "flot",
          "seriesOverrides": [],
          "span": 4,
          "stack": False,
          "steppedLine": False,
          "targets": [
            {
              "dsType": "influxdb",
              "groupBy": [
                {
                  "params": [
                    "$interval"
                  ],
                  "type": "time"
                },
                {
                  "params": [
                    "0"
                  ],
                  "type": "fill"
                }
              ],
              "measurement": "mem",
              "policy": "default",
              "refId": "A",
              "resultFormat": "time_series",
              "select": [
                [
                  {
                    "params": [
                      "used_percent"
                    ],
                    "type": "field"
                  },
                  {
                    "params": [],
                    "type": "mean"
                  }
                ]
              ],
              "tags": []
            }
          ],
          "timeFrom": 0,
          "timeShift": 0,
          "title": "Mem Usage",
          "tooltip": {
            "msResolution": False,
            "shared": True,
            "value_type": "cumulative",
            "sort": 0
          },
          "type": "graph",
          "xaxis": {
            "show": True
          },
          "yaxes": [
            {
              "format": "percent",
              "label": "",
              "logBase": 1,
              "max": 100,
              "min": 0,
              "show": True
            },
            {
              "format": "short",
              "label": 0,
              "logBase": 1,
              "max": 0,
              "min": 0,
              "show": False
            }
          ]
        },
        {
          "aliasColors": {},
          "bars": False,
          "datasource": DATASOURCE,
          "editable": True,
          "error": False,
          "fill": 1,
          "grid": {
            "threshold1": 0,
            "threshold1Color": "rgba(216, 200, 27, 0.27)",
            "threshold2": 0,
            "threshold2Color": "rgba(234, 112, 112, 0.22)",
            "thresholdLine": False
          },
          "id": 15,
          "interval": "30s",
          "isNew": True,
          "legend": {
            "avg": False,
            "current": False,
            "max": False,
            "min": False,
            "show": False,
            "total": False,
            "values": False
          },
          "lines": True,
          "linewidth": 2,
          "links": [],
          "0PointMode": "connected",
          "percentage": False,
          "pointradius": 5,
          "points": False,
          "renderer": "flot",
          "seriesOverrides": [],
          "span": 4,
          "stack": False,
          "steppedLine": False,
          "targets": [
            {
              "dsType": "influxdb",
              "groupBy": [
                {
                  "params": [
                    "$interval"
                  ],
                  "type": "time"
                },
                {
                  "params": [
                    "0"
                  ],
                  "type": "fill"
                }
              ],
              "measurement": "disk",
              "policy": "default",
              "refId": "A",
              "resultFormat": "time_series",
              "select": [
                [
                  {
                    "params": [
                      "used_percent"
                    ],
                    "type": "field"
                  },
                  {
                    "params": [],
                    "type": "mean"
                  }
                ]
              ],
              "tags": []
            }
          ],
          "timeFrom": 0,
          "timeShift": 0,
          "title": "Disk Usage",
          "tooltip": {
            "msResolution": True,
            "shared": True,
            "value_type": "cumulative",
            "sort": 0
          },
          "type": "graph",
          "xaxis": {
            "show": True
          },
          "yaxes": [
            {
              "format": "percent",
              "label": 0,
              "logBase": 1,
              "max": 100,
              "min": 0,
              "show": True
            },
            {
              "format": "short",
              "label": 0,
              "logBase": 1,
              "max": 0,
              "min": 0,
              "show": False
            }
          ]
        }
      ],
      "showTitle": True,
      "title": "Avg Infra monitoring"
    }
  ],
  "time": {
    "from": "now-15m",
    "to": "now"
  },
  "timepicker": {
    "refresh_intervals": [
      "5s",
      "10s",
      "30s",
      "1m",
      "5m",
      "15m",
      "30m",
      "1h",
      "2h",
      "1d"
    ],
    "time_options": [
      "5m",
      "15m",
      "1h",
      "6h",
      "12h",
      "24h",
      "2d",
      "7d",
      "30d"
    ]
  },
  "templating": {
    "list": []
  },
  "annotations": {
    "list": []
  },
  "refresh": "30s",
  "schemaVersion": 12,
  "version": 1,
  "links": [],
  "gnetId": 0
}
}

def makePost(url, header, body):
    r = requests.post(url, headers=header, auth=(user, pw), data=json.dumps(body))
    if r.status_code == 200:
        return json.loads(r.text)
    print r.text
    raise NameError(url)

url = 'http://%s:3000/api/dashboards/db' % GRAFANA
hdr = {'Content-Type':'application/json'}

makePost(url, hdr, body)

~~~
