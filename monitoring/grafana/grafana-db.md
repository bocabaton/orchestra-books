# Crete Grafana Datasource

## Environment

Keyword     | Value             | Description
----        | ----              | ----
GRAFANA_USER    | admin         | Grafana User ID
GRAFANA_PW      | admin         | Grafana Password
DATABASE        | mydatabase    | Database name
GRAFANA         | localhost     | Grafana Server address

Last update 2016-06-03


# Create Data Source

~~~python
import requests
import json

user = '${GRAFANA_USER}'
pw = '${GRAFANA_PW}'
DATABASE = '${DATABASE}'
GRAFANA = '${GRAFANA}'

body = {
    "name":"%s-influxdb" % DATABASE,
    "type":"influxdb",
    "access":"proxy",
    "url":"http://localhost:8086",
    "password": "root",
    "user": "root",
    "database": "%s" % DATABASE,
    "basicAuth": False,
    "basicAuthUser": "",
    "basicAuthPassword": "",
    "withCredentials": False,
    "isDefault": False
}

def makePost(url, header, body):
    r = requests.post(url, headers=header, auth=(user, pw), data=json.dumps(body))
    if r.status_code == 200:
        return json.loads(r.text)
    print r.text
    raise NameError(url)

url = 'http://%s:3000/api/datasources' % GRAFANA
hdr = {'Content-Type':'application/json'}

makePost(url, hdr, body)
~~~

