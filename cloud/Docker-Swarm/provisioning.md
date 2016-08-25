
# Environment

Keyword | Value | Description
----    | ----  | ----
URL     | http://127.0.0.1/api/v1 | URL for request
TOKEN   | my_token              | Token for API (must be overrided)
METADATA      | http://127.0.0.1/api/v1/catalog/{stack_id}/env | Environment URL for stack
ZONE_ID | 14b14664-705e-42e9-8106-240b83f9df79  | Zone ID (must be overrided)
AMI_ID   | ami-7172b611 | Amazon Linux AMI 2016.03.3 (HVM), SSD Volume Type (us-west-2)
SWARM_NODES   | 2       | Number of Docker swarm nodes
KEY_NAME   | aws_son    | Keypair name
MGMT_INSTANCE_TYPE | t2.small   | Instance type of Management node
SWARM_INSTANCE_TYPE | t2.medium  | Instance type of Swarm node
STACK_ID    | xxxxxx            | Stack ID is automatically overrided by system

# Create Server

~~~python
import requests
import json
import time

# convert Jeju keyword to python variable

ZONE_ID = '${ZONE_ID}'

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

def getRegionID(zone_id):
    url = '${URL}/provisioning/zones/${ZONE_ID}'
    try:
        r = requests.get(url, headers=hdr)
        if (r.status_code == 200):
            result = json.loads(r.text)
            return r.region_id
    except requests.exception.ConnectionError:
        print "Failed to connect"


def createZone(region_id, name, platform):
    url = '${URL}/provisioning/zones'
    try:
        req = {'name':name, 'region_id':region_id, 'zone_type':platform}
        r =  requests.post(url, headers=hdr, data=json.dumps(req))
        if r.status_code == 200:
            result = json.loads(r.text)
            return result
    except requests.exception.ConnectionError:
        print "Failed to connect"

def createZoneDetail(zone_id, docker_url):
    dic = {'create':[{'key':'DOCKER_HOST','value':docker_url}]}
    url = '${URL}/provisioning/zones/%s' % zone_id
    try:
        r =  requests.post(url, headers=hdr, data=json.dumps(dic))
        if r.status_code == 200:
            result = json.loads(r.text)
            return result
    except requests.exception.ConnectionError:
        print "Failed to connect"
   


# Node name
mgmt01 = []
mgmt02 = []
swarm_nodes = []
all = []

# Create mgmt01
print "Create mgmt01"
boto_request = {'ImageId':'${AMI_ID}',
        'KeyName':'${KEY_NAME}',
        'MinCount':1,
        'MaxCount':1,
        'InstanceType':'${MGMT_INSTANCE_TYPE}'
        }

req = {'zone_id': '${ZONE_ID}',
        'name': 'mgmt01',
        'key_name': '${KEY_NAME}',
        'stack_id':'${STACK_ID}',
        'floatingIP':True,
        'request':boto_request}

server = createServer(req)
mgmt01.append(server['server_id'])
all.append(server['server_id'])

# Add environment of mgmt01
body = {'add':{'mgmt01':mgmt01}}
addEnv('${METADATA}', body)

time.sleep(30)
# Get IP of mgmt01
addr = getServerDetail(server['server_id'],'private_ip_address')
body = {'add':{'jeju':{'MGMT01':addr['private_ip_address']}}}
addEnv('${METADATA}', body)
mgmt01_ip = getServerDetail(server['server_id'],'floatingip')

# Create Mgmt02
print "Create mgmt02"
req = {'zone_id': '${ZONE_ID}',
        'name': 'mgmt02',
        'key_name': '${KEY_NAME}',
        'stack_id':'${STACK_ID}',
        'floatingIP':True,
        'request':boto_request}

server = createServer(req)
mgmt02.append(server['server_id'])
all.append(server['server_id'])

# Add environment of mgmt02
body = {'add':{'mgmt02':mgmt02}}
addEnv('${METADATA}', body)

# Create Swarm nodes
print "Create Swarm nodes"
boto_request = {'ImageId':'${AMI_ID}',
        'KeyName':'${KEY_NAME}',
        'MinCount':1,
        'MaxCount':1,
        'InstanceType':'${SWARM_INSTANCE_TYPE}'
        }


for i in range(${SWARM_NODES}):
    node_name = "swarm-%.2d" % (i+1)
    req = {'zone_id': '${ZONE_ID}', 
            'name':node_name,
            'key_name':'${KEY_NAME}',
            'stack_id':'${STACK_ID}',
            'floatingIP':True,
            'request': boto_request
        }
    server = createServer(req)
    print server
    swarm_nodes.append(server['server_id'])
    all.append(server['server_id'])

body = {'add':{'swarm_nodes':swarm_nodes}}
addEnv('${METADATA}', body)

body = {'add':{'all':all}}
addEnv('${METADATA}', body)

# Register Docker-Swarm Zone
# Get Region ID
region_id = getRegionID('${ZONE_ID}')
createZone(region_id, 'docker-swarm', 'docker')

# Register Docker API
docker_url = 'tcp://%s:4000' % mgmt01_ip
createZoneDetail(ZONE_ID, docker_url)
~~~
