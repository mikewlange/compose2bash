compose2bash
========

Toll that converts docker-compose.yml (former fig.yml) files to bash scripts.


## Download

[Releases](https://github.com/dockito/compose2bash/releases)


## Usage

```bash
compose2bash -yml=examples/docker-compose.yml -output=examples/output -app=myapp
```

## Example
**docker-compose.yml**

```yml
api:
  command: npm start
  image: docker.mydomain.com/api:latest
  ports:
    - 3000
  environment:
    VIRTUAL_PORT: 3000
    VIRTUAL_HOST: api.mydomain.com
    NODE_ENV: development
    MONGO_DATABASE: develop_api
  volumes:
    - .:/src
  privileged: True
```


**output: myapp-api.1.sh**
```bash
#!/bin/bash
/usr/bin/docker pull docker.mydomain.com/api:latest
/usr/bin/docker rm -f myapp-api_1
/usr/bin/docker run \
    --privileged=true  \
    --restart=always \
    -d \
    --name myapp-api_1 \
    -v .:/src  \
    -e develop_api -e development -e api.mydomain.com -e 3000  \
    -p 3000  \
    docker.mydomain.com/api:latest  npm start
```

## Options

- **-app**: Application name
- **-output**: Output directory (default `.`)
- **-yml**: Compose file path (default `docker-compose.yml`)
