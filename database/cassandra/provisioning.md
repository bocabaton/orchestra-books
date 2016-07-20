# Create Servers

Deploy Servers at Joyent Cloud

# Environment

Keyword | Value | Description
----    | ----  | ----
URL     | http://127.0.0.1/api/v1 | URL for request
TOKEN   | my_token                | Override real token
METADATA      | http://127.0.0.1/api/v1/catalog/{stack_id}/env  | METADATA is overrided by system
ZONE_ID | 14b14664-705e-42e9-8106-240b83f9df79  | Override real zone ID of Joyent Zone
NUM_NODES   | 3                   | Number of Cassandra nodes
IMAGE_ID   | a601aa10-100a-11e6-9eb1-2bbb19e1e3e6 | Ubuntu 14.04 Container at Joyent
FLAVOR_ID   | g4-general-4G     | 1 vcpu, 4G memory
KEY_NAME   | my-key-name     | Keyname for access
PROJECT | myproject           | Project Name

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

cluster = []
addresses = []
for i in range(${NUM_NODES}):
    node_name = "${PROJECT}-cassandra-%.2d" % (i+1)
    req = {'zone_id': '${ZONE_ID}',
            'name':node_name,
            'key_name':'${KEY_NAME}',
            'floatingIP':False,
            'request':{
                'name': node_name,
                'package': '${FLAVOR_ID}',
                'image': '${IMAGE_ID}'
            }
        }
    server = createServer(req)
    print server
    cluster.append(server['server_id'])
    addr = getServerDetail(server['server_id'],'private_ip_address')
    addresses.append(addr['private_ip_address'])

body = {'add':{'cluster_nodes':cluster}}
addEnv('${METADATA}', body)

seed_ip = ""
for addr in addresses:
    seed_ip = seed_ip + addr + ","
body = {'add':{'jeju':{'SEEDS':seed_ip[:-1]}}}
addEnv('${METADATA}', body)
~~~

