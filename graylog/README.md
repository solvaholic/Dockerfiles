# `./graylog`

Defines a [Graylog 3.2](https://docs.graylog.org/en/3.2/index.html) service stack, fit for deploying to a Docker Swarm.

# `./graylog/graylog-stack.yml`

Is a [Docker Compose](https://docs.docker.com/compose/compose-file/) file. It describes the [services](https://docs.docker.com/engine/swarm/how-swarm-mode-works/services/) that compose our Graylog [stack](https://docs.docker.com/engine/swarm/stack-deploy/).

## Environment variables

Before you deploy this Docker service stack, create `elasticsearch.env` and `graylog.env` in this same directory to set several important service configuration variables.

For example here is a functional default `elasticsearch.env`:

```
http.host=0.0.0.0
transport.host=localhost
network.host=0.0.0.0
"ES_JAVA_OPTS=-Xms512m -Xmx768m"
```

And here is a functional default `graylog.env`:

```
# Run 'pwgen -N 1 -s 55' to generate this secret:
GRAYLOG_PASSWORD_SECRET=jZkZV4ZCEQOJXiBDHiIp9QamkW1I9tkGlu65U5tnGn7ZeZJRGXtLVSj

# Run 'echo -n "P@ssw0rd" | sha256sum' to generate this SHA2 value:
GRAYLOG_ROOT_PASSWORD_SHA2=b03ddf3ca2e714a6548e7495e2a03f5e824eaac9837cd7f159c67b90fb4b7342

# Ensure this URI will be reachable from your browser:
GRAYLOG_HTTP_EXTERNAL_URI=http://localhost:9000/
```

## Run or deploy

Run the stack locally:

    docker-compose -f graylog-stack.yml up -d

Deploy the stack to `_swarmmgr`:

    docker-compose -f graylog-stack.yml config | \
    ssh "${_swarmmgr}" docker stack deploy -c - graylog
