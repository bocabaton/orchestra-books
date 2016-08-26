# Create DockerSwarm Catalog

## Environment

Keyword | Value         | Description
----    | ----          | ----
URL     | http://127.0.0.1/api/v1   | Orchestra API enpoint
REPO    | https://raw.githubusercontent.com/bocabaton/orchestra-books/master   | Repository if Orchestra books
USER_ID | admin                     | User ID
PASSWORD| password                  | Password

## Portfolio

~~~python
import requests
import json
import pprint,sys,re

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

header = {'Content-Type':'application/json'}

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

display('Auth')
url = '${URL}/token/get'
user_id='${USER_ID}'
password='${PASSWORD}'

body = {'user_id':user_id, 'password':password}
token = makePost(url, header, body)
token_id = token['token']
header.update({'X-Auth-Token':token_id})

url = '${URL}/catalog/portfolios?name=DevOps'
display('List Portfolios')
p1 = makeGet(url, header)
show(p1)
if p1['total_count'] > 0:
    p_id = p1['results'][0]['portfolio_id']
else:
    display('Create Portfolio')
    body = {'name':'DevOps', 'description':'DevOps Tool',
            'owner':'choonho.son'}
    portfolio = makePost(url, header, body)
    show(portfolio)
    p_id = portfolio['portfolio_id']

######################################
# Product
######################################
product_url = '${URL}/catalog/products'
display('List Product')
show(makeGet(product_url, header))

display('Create Product')
body = {'portfolio_id':p_id, 'name':'devtool', 'short_description':'Docker for devtool',
        'description':'Docker Private Registry, GitLab, and Jenkins', 'provided_by':'BocaBaton',
        'vendor':'PyEngine, Inc'}
product = makePost(product_url, header, body)
show(product)
product_id = product['product_id']
product_url2 = '${URL}/catalog/products/%s' % product_id

######################################
# Product Detail
######################################
detail_url = '${URL}/catalog/products/%s/detail' % product_id
display('Create Product detail')
body = {'email':'choonho.son@gmail.com', 'support_link':'${REPO}/devops/devtools','support_description':'This is builded by Orchestra'}
show(makePost(detail_url, header, body))

######################################
# Package
######################################
package_url = '${URL}/catalog/packages'
body = {'product_id':product_id, 'pkg_type':'bpmn', 'template':'${REPO}/devops/devtools/workflow.bpmn', 'version':'0.1', 'description':'https://raw.githubusercontent.com/bocabaton/orchestra-books/master/devops/devtools/README.md'}
display('Create Package')
package = makePost(package_url, header, body)
package_id = package['package_id']
show(package)

######################################
# Register Workflow
######################################
display('Register Workflow')
workflow_url = '${URL}/catalog/workflows'
body = {'template':'${REPO}/cloud/Docker-Swarm/docker-swarm.bpmn', 'template_type':'bpmn'}
workflow = makePost(workflow_url, header, body)
workflow_id = workflow['workflow_id']
show(workflow)

######################################
# Map Task
######################################
display('Map Task #1')
task_url = '${URL}/catalog/workflows/%s/tasks' % workflow_id
body = {'map': {'name':'Create Docker Private Registry', 'task_type':'jeju', 'task_uri':'${REPO}/devops/devtools/registry.md'}}
task = makePost(task_url, header, body)
task_id = task['task_id']
show(task)

display('Map Task #2')
task_url = '${URL}/catalog/workflows/%s/tasks' % workflow_id
body = {'map': {'name':'Create Gitlab', 'task_type':'jeju', 'task_uri':'${REPO}/devops/devtools/gitlab.md'}}
task = makePost(task_url, header, body)
task_id = task['task_id']
show(task)


display('Map Task #3')
task_url = '${URL}/catalog/workflows/%s/tasks' % workflow_id
body = {'map': {'name':'Create Jenkins', 'task_type':'jeju', 'task_uri':'${REPO}/devops/devtools/jenkins.md'}}
task = makePost(task_url, header, body)
task_id = task['task_id']
show(task)

~~~
