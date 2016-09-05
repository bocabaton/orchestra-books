# Crete InfluxDB Database

## Environment

Keyword     | Value             | Description
----        | ----              | ----
INFLUXDB_USER    | admin         | InfluxDB User ID
INFLUXDB_PW      | admin         | InfluxDb Password
DATABASE        | mydatabase    | Database name
INFLUXDB        | localhost     | InfluxDB address
RETENTION       | 12w           | Retention Policy

Last update 2016-06-03


# Create Data Source

~~~python
import requests
import json
import urllib

# This is translation from jeju to python variable
user = '${INFLUXDB_USER}'
pw = '${INFLUXDB_PW}'
DATABASE = '${DATABASE}'
INFLUXDB = '${INFLUXDB}'
RETENTION = '${RETENTION}'

body = {'q':"CREATE DATABASE \"%s\"" % DATABASE}

def makeGet(url, header):
    r = requests.get(url, headers=header)
    if r.status_code == 200:
        return json.loads(r.text)
    print r.text
    raise NameError(url)

url = 'http://%s:8086/query' % INFLUXDB
url2 = "%s?%s&db=_internal" % (url, urllib.urlencode(body))
hdr = {'Content-Type':'application/json'}

print url2
makeGet(url2, hdr)

body = {'q':"ALTER RETENTION POLICY default ON \"%s\" DURATION %s" % (DATABASE,RETENTION)}
url2 = "%s?%s&db=_internal" % (url, urllib.urlencode(body))
print url2
makeGet(url2, hdr)
~~~

