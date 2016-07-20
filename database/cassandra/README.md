# Build a Cassandra cluster

# High level architecture

<img src="https://raw.githubusercontent.com/bocabaton/orchestra-books/master/database/cassandra/architecture.png">

# Deployment Architecture
We are going to deploy 3 nodes at Cloud

<img src="https://raw.githubusercontent.com/bocabaton/orchestra-books/master/database/cassandra/deployment_architecture.png">

# Workflow Process

The deployment of docker swarm cluster has following process.

<img src="https://raw.githubusercontent.com/bocabaton/orchestra-books/master/database/cassandra/workflow.png">

# Default Environment

Keyword | Value | Description
----    | ----  | ----
KEY_NAME   | my-key-name     | Keyname for access
PROJECT | myproject           | Project Name
NUM_NODES   | 3                   | Number of Cassandra nodes
ZONE_ID | 14b14664-705e-42e9-8106-240b83f9df79  | Override real zone ID of Joyent Zone
FLAVOR_ID   | g4-general-4G     | 1 vcpu, 4G memory
VER     | 30x               | Cassandra Version

# Reference
