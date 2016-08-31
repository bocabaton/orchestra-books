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
GRAFANA= "${GRAFANA}"

body = {"dashboard":
{
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
          "0PointMode": "connected",
          "aliasColors": {},
          "bars": False,
          "datasource": DATASOURCE,
          "editable": True,
          "error": False,
          "fill": 1,
          "grid": {
            "threshold1": 0,
            "threshold1Color": "rgba(12, 12, 10, 0.93)",
            "threshold2": 0,
            "threshold2Color": "rgba(234, 112, 112, 0.22)"
          },
          "id": 13,
          "interval": "1m",
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
          "timeFrom": "24h",
          "timeShift": 0,
          "title": "CPU Usage",
          "tooltip": {
            "msResolution": False,
            "shared": True,
            "sort": 0,
            "value_type": "cumulative"
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
          "0PointMode": "connected",
          "aliasColors": {},
          "bars": False,
          "datasource": DATASOURCE,
          "editable": True,
          "error": False,
          "fill": 1,
          "grid": {
            "threshold1": 0,
            "threshold1Color": "rgba(5, 5, 5, 0.93)",
            "threshold2": 0,
            "threshold2Color": "rgba(234, 112, 112, 0.22)"
          },
          "id": 14,
          "interval": "1m",
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
          "timeFrom": "24h",
          "timeShift": 0,
          "title": "Mem Usage",
          "tooltip": {
            "msResolution": False,
            "shared": True,
            "sort": 0,
            "value_type": "cumulative"
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
          "0PointMode": "connected",
          "aliasColors": {},
          "bars": False,
          "datasource": DATASOURCE,
          "editable": True,
          "error": False,
          "fill": 1,
          "grid": {
            "threshold1": 0,
            "threshold1Color": "rgba(9, 9, 8, 0.97)",
            "threshold2": 0,
            "threshold2Color": "rgba(234, 112, 112, 0.22)",
            "thresholdLine": False
          },
          "id": 15,
          "interval": "1m",
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
              "query": "SELECT mean(\"used_percent\") FROM \"disk\" WHERE $timeFilter GROUP BY time($interval) fill(0)",
              "rawQuery": True,
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
          "timeFrom": "24h",
          "timeShift": 0,
          "title": "Disk Usage",
          "tooltip": {
            "msResolution": True,
            "shared": True,
            "sort": 0,
            "value_type": "cumulative"
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
    },
    {
      "collapse": False,
      "editable": True,
      "height": 251.4375,
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
          "id": 19,
          "interval": "30s",
          "isNew": True,
          "legend": {
            "avg": False,
            "current": False,
            "max": False,
            "min": False,
            "show": True,
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
          "span": 12,
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
              "policy": "default",
              "query": "SELECT 100-mean(\"usage_idle\") FROM \"cpu\" WHERE \"cpu\" = 'cpu-total' AND $timeFilter GROUP BY time($interval), \"host\" fill(0)",
              "rawQuery": True,
              "refId": "A",
              "resultFormat": "time_series",
              "select": [
                [
                  {
                    "params": [
                      "value"
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
          "timeFrom": "1h",
          "timeShift": 0,
          "title": "CPU detail",
          "tooltip": {
            "msResolution": True,
            "shared": True,
            "sort": 0,
            "value_type": "cumulative"
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
              "max": 0,
              "min": 0,
              "show": True
            },
            {
              "format": "short",
              "label": 0,
              "logBase": 1,
              "max": 0,
              "min": 0,
              "show": True
            }
          ]
        }
      ],
      "title": "New row"
    },
    {
      "collapse": False,
      "editable": True,
      "height": "250px",
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
          "id": 17,
          "interval": "30s",
          "isNew": True,
          "legend": {
            "avg": False,
            "current": False,
            "max": False,
            "min": False,
            "show": True,
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
          "span": 12,
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
              "policy": "default",
              "query": "SELECT mean(\"used_percent\") FROM \"mem\" WHERE $timeFilter GROUP BY time($interval), \"host\" fill(0)",
              "rawQuery": True,
              "refId": "A",
              "resultFormat": "time_series",
              "select": [
                [
                  {
                    "params": [
                      "value"
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
          "timeFrom": "1h",
          "timeShift": 0,
          "title": "Memory detail",
          "tooltip": {
            "msResolution": True,
            "shared": True,
            "sort": 0,
            "value_type": "cumulative"
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
              "max": 0,
              "min": 0,
              "show": True
            },
            {
              "format": "short",
              "label": 0,
              "logBase": 1,
              "max": 0,
              "min": 0,
              "show": True
            }
          ]
        }
      ],
      "title": "New row"
    },
    {
      "collapse": False,
      "editable": True,
      "height": "250px",
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
          "id": 18,
          "interval": "30s",
          "isNew": True,
          "legend": {
            "avg": False,
            "current": False,
            "max": False,
            "min": False,
            "show": True,
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
          "span": 12,
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
              "policy": "default",
              "query": "SELECT mean(\"used_percent\") FROM \"disk\" WHERE $timeFilter GROUP BY time($interval), \"host\" fill(0)",
              "rawQuery": True,
              "refId": "A",
              "resultFormat": "time_series",
              "select": [
                [
                  {
                    "params": [
                      "value"
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
          "timeFrom": "1h",
          "timeShift": 0,
          "title": "Disk detail",
          "tooltip": {
            "msResolution": True,
            "shared": True,
            "sort": 0,
            "value_type": "cumulative"
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
              "max": 0,
              "min": 0,
              "show": True
            },
            {
              "format": "short",
              "label": 0,
              "logBase": 1,
              "max": 0,
              "min": 0,
              "show": True
            }
          ]
        }
      ],
      "title": "New row"
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
  "version": 10,
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
