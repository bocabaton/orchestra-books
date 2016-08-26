# Unitest

# Environment

Keyword | Value | Description
----    | ----  | ----
URL     | http://127.0.0.1/api/v1 | URL for request
TOKEN   | my_token              | Token for API (must be overrided)
METADATA      | http://127.0.0.1/api/v1/catalog/{stack_id}/env | Environment URL for stack
ZONE_ID | 14b14664-705e-42e9-8106-240b83f9df79  | Zone ID (must be overrided)
KEY_NAME   | aws_son    | Keypair name
STACK_ID    | xxxxxx            | Stack ID is automatically overrided by system
NAME    | jenkins             | name of jenkins container
IMAGE   | jenkins             | Jekins docker image

## Portfolio

~~~python
import requests
import json
import pprint,sys,re

NAME = '${NAME}'
IMAGE = '${IMAGE}'
ZONE_ID = '${ZONE_ID}'
KEY_NAME = '${KEY_NAME}'
STACK_ID = '${STACK_ID}'

class writer:
    def write(self, text):
        text=re.sub(r'u\'([^\']*)\'', r'\1',text)
        sys.stdout.write(text)

wrt=writer()

pp = pprint.PrettyPrinter(indent=2, stream=wrt)

def show(dic):
    print pp.pprint(dic)

def display(title):
    print "\n"
    print "################# " + '{:20s}'.format(title) + "##############"

header = {'Content-Type':'application/json', 'X-Auth-Token':'${TOKEN}'}

def makePost(url, header, body):
    r = requests.post(url, headers=header, data=json.dumps(body))
    if r.status_code == 200:
        return json.loads(r.text)
    print r.text
    raise NameError(url)

def makePut(url, header, body):
    r = requests.put(url, headers=header, data=json.dumps(body))
    if r.status_code == 200:
        return json.loads(r.text)
    print r.text
    raise NameError(url)


def makeGet(url, header):
    r = requests.get(url, headers=header)
    if r.status_code == 200:
        return json.loads(r.text)
    print r.text
    raise NameError(url)

def makeDelete(url, header):
    r = requests.delete(url, headers=header)
    if r.status_code == 200:
        return json.loads(r.text)
    print r.text
    raise NameError(url)

######################################
# Deploy Server
######################################
server_url = '${URL}/provisioning/servers'
display('Deploy Server')
request = {
    "name":"${NAME}",
    "detach":True,
    "tty":True,
    "image":IMAGE,
    "ports":[8080,50000],
}
body = {'name':NAME, 'zone_id': ZONE_ID, 'key_name':KEY_NAME, 'floatingIP':False, 'stack_id':STACK_ID, 'request':request}
server = makePost(server_url, header, body)
show(server)
~~~

