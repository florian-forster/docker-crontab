# docker-crontab

Fork of the fine [willfarrell/docker-crontab](https://github.com/willfarrell/docker-crontab), which allows executing cron jobs in a dockerized environment.

The added value of this fork is that it retrieves the configuration dynamically from docker container labels, so there is no need to maintain a dedicated config file.

## How to use

Build the container from scratch (no public docker image available yet)

```
# clone the github project, then execute from within the project root directory:
docker build -t florianforster/docker-crontab .
```

Start the cron container

```
docker run --rm --name=cron -v /var/run/docker.sock:/var/run/docker.sock:ro florianforster/docker-crontab

```

While running, the cron container will check if there are any containers with cron jobs defined via labels.

Add a label `cron.enabled=true` to a container to make it visible for the cron.

Then, add the following lavels to define cron jobs:

`cron.<jobname>.name`: The display name of the cron job in the cron logs
`cron.<jobname>.schedule`: The cron schedule, defining how often the job will be executed
`cron.<jobname>.command`: Comnmand(s) to be executed in the container. The commands get executed inside of the cron container in a bash. You can also run docker commands that will be executed in the context of the host, because the docker socket is mapped as volume.
`cron.<jobname>.name`: The display name of the cron job in the cron logs

You can assign several jobs to one container. You cannot have the same jobname twice on the same container. However, you can have the same jobname on different containers. The jobs will get the container name assigned by the script to avoid ambiguity in this case.

All available configuration options can be found in the parent project https://github.com/willfarrell/docker-crontab. Note that triggers are not supported yet.
