# azure-devops-docker-agent
Dockerfile and script to build  docker image running a Linux Azure Devops build agent

The official docs can be found here: https://docs.microsoft.com/en-us/azure/devops/pipelines/agents/docker?view=azure-devops#linux  

(IMHO, you __can__ and __should__ run Linux containers on windows too)

## Creating a Personal Access Token (PAT)

You need to create a PAT (Personal Access Token, see https://dev.azure.com/yourorganizationname/_usersSettings/tokens to create one of your own). I couldn't understand all the permissions needed. I think you need a lot, to be able to actually register the agents with DevOps without any intervention.

Create an environment variable, AZP_TOKEN containing the PAT.

## Building the image
```
docker build -t azure-devops-docker-agent:latest https://github.com/erikbra/azure-devops-docker-agent.git#main
```

**NOTE: you might have to use `sudo` to build on Linux, if you don't have permissions to connect to the docker daemon as your ordinary user**

That will give you an image, **azure-devops-agent:latest** on you box. Then you can run it locally.


## Setting environment variables

(see https://docs.microsoft.com/en-us/azure/devops/organizations/accounts/use-personal-access-tokens-to-authenticate?view=azure-devops)

| Environment variable | Description                                                 |
|----------------------|-------------------------------------------------------------|
| AZP_URL              | The URL of the Azure DevOps or Azure DevOps Server instance. |
| AZP_TOKEN            | [Personal Access Token (PAT)](https://docs.microsoft.com/en-us/azure/devops/organizations/accounts/use-personal-access-tokens-to-authenticate?view=azure-devops) with **Agent Pools (read, manage)** scope, created by a user who has permission to [configure agents](pools-queues.md#creating-agent-pools), at `AZP_URL`.    |
| AZP_AGENT_NAME       | Agent name (default value: the container hostname).          |
| AZP_POOL             | Agent pool name (default value: `Default`).                  |



## Running the image
Use the included `docker-compose.yml` if you wish, and just write `docker-compose up`. 

Or, you can use just Docker, and run it manually:

```
docker run \
  -e AZP_URL=<Azure DevOps instance url> \
  -e AZP_TOKEN=<PAT token> \
  -e AZP_AGENT_NAME=mydockeragent \
  -e AZP_POOL=superduperpool
  -v /var/run/docker.sock:/var/run/docker.sock azure-devops-docker-agent:latest
```


You need to map the docker socket to be able to run docker builds inside the container. It's not pretty, and it's a security risk, so do this at your own risk :)


