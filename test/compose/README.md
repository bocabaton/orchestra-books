# Docker Compose Test

This is an example code for docker compose deployment

## Code

We execute docker-compose.yml file

~~~yml
# test
test-node:
  image: postgres
  restart: never
  env_file: .env
  ports:
    - 8000

~~~
