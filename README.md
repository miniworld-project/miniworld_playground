# MiniWorld Playground

The name of the repository already mentions it: this is the `playground` for MiniWorld.

## What's inside ?

- Docker-Compose
- Examples VM images
- Example scenario files

## Get started

Start MiniWorld inside Docker:

```bash
BRANCH=master docker-compose pull
BRANCH=master docker-compose up                # -d for detached mode
```

This starts the MiniWorld container. It is ready if you see `core_1  | INFO __main__ <module>: rpc server running` printed to stdout.

Download the VM images:

```bash
(cd examples/ && ./get_images.sh)
```

## Run a MiniWorld Scenario

Enter the docker container:
```bash
docker-compose exec core bash
```

Run a scenario listed in the examples/ directory as `*.json`:

```bash
mwcli start examples/batman_adv.json
mwcli step
mwcli exec --node-id 1 'ping -c 1 10.0.0.2'
mwcli stop
mwcli start examples/nb_bridged_wifi.json
mwcli shell 1
```



## Clean up

Press `CTRL-C` inside the docker-compose terminal or run `docker-compose down`.

To delete all resources, run `docker-compose rm`.


## Go distributed

Start one coordinator and two server processes:

```bash
BRANCH=master docker-compose -f docker-compose-distributed.yml pull
BRANCH=master docker-compose -f docker-compose-distributed.yml up coordinator
BRANCH=master docker-compose -f docker-compose-distributed.yml up server1 server2
```

Run B.A.T.M.A.N. advanced in the distributed mode with servers connected by GRE (layer 2) tunnels:

```bash
docker-compose -f docker-compose-distributed.yml exec coordinator bash
mwcli start examples/batman_adv_distributed.json
mwcli step
mwcli --distributed exec --node-id 1 ping 10.0.0.5
mwcli --distributed exec ping 10.0.0.5
mwcli stop
```