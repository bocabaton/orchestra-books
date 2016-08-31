# Create Servers

Deploy Servers at Joyent Cloud

# Environment

Keyword | Value | Description
----    | ----  | ----
URL     | http://127.0.0.1/api/v1 | URL for request
TOKEN   | my_token                | Override real token
METADATA      | http://127.0.0.1/api/v1/catalog/{stack_id}/env  | METADATA is overrided by system
ZONE_ID | 14b14664-705e-42e9-8106-240b83f9df79  | Override real zone ID of Joyent Zone
IMAGE_ID   | a601aa10-100a-11e6-9eb1-2bbb19e1e3e6 | Ubuntu 14.04 Container at Joyent
FLAVOR_ID   | g4-general-4G     | 1 vcpu, 4G memory
KEY_NAME   | server     | Keyname for access
PROJECT | monitoring           | Project Name
STACK_ID    | xxxxxx            | Stack ID is automatically overrided by system

# Create Server

~~~python
import requests
import json

hdr = {'Content-Type':'application/json','X-Auth-Token':'${TOKEN}'}
def createServer(req):
    url = '${URL}/provisioning/servers'
    try:
        r =requests.post(url, headers=hdr, data=json.dumps(req))
        if (r.status_code == 200):
            result = json.loads(r.text)
            return result
    except requests.exception.ConnectionError:
        print "Failed to connect"

def getServerDetail(server_id, key):
    url = '${URL}/provisioning/servers/%s/detail' % server_id
    req = {'get':key}
    try:
        r = requests.post(url, headers=hdr, data=json.dumps(req))
        if (r.status_code == 200):
            result = json.loads(r.text)
            return result
    except requests.exception.ConnectionError:
        print "Failed to connect"

def addEnv(url, body):
    r = requests.post(url, headers=hdr, data=json.dumps(body))
    if r.status_code == 200:
        result = json.loads(r.text)

node_name = "${PROJECT}"
req = {'zone_id': '${ZONE_ID}',
        'name':node_name,
        'key_name':'${KEY_NAME}',
        'stack_id':'${STACK_ID}',
        'floatingIP':False,
        'request':{
            'name': node_name,
            'package': '${FLAVOR_ID}',
            'image': '${IMAGE_ID}'
        }
    }

cluster=[]
server = createServer(req)
print server
cluster.append(server['server_id'])
addr = getServerDetail(server['server_id'],'floatingip')
grafana_ip = addr['floatingip']

body = {'add':{'grafana':cluster}}
addEnv('${METADATA}', body)

# Update Stack Endpoint
stack_url = '${URL}/catalog/stacks/%s/endpoint' % STACK_ID
body = {'add':{'INFLUXDB':grafana_ip}}
makePost(stack_url, hdr, body)

stack_url = '${URL}/catalog/stacks/%s/endpoint' % STACK_ID
body = {'add':{'GRAFANA':grafana_ip}}
makePost(stack_url, hdr, body)

grafana_url='http://%s:3000' % grafana_ip
stack_url = '${URL}/catalog/stacks/%s/endpoint' % STACK_ID
body = {'add':{'URL':grafana_url}}
makePost(stack_url, hdr, body)


~~~

