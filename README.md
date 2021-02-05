# azure-devops-docker-agent
Dockerfile and script to build  docker image running a Linux Azure Devops build agent

The official docs can be found here: https://docs.microsoft.com/en-us/azure/devops/pipelines/agents/docker?view=azure-devops#linux  (you can and should run Linux containers on windows too)

You need to create a PAT (Personal Access Token, see https://dev.azure.com/yourorganizationname/_usersSettings/tokens to create one of your own). I couldn't understand all the permissions needed. I think you need a lot, to be able to actually register the agents with DevOps without any intervention.

Create an environment variable, AZP_TOKEN containing the PAT.

## Usage:

```
docker build -t azure-devops-agent:latest https://github.com/erikbra/azure-devops-docker-agent.git
```

That will give you an image, **azure-devops-agent:latest** on you box. Then you can run it locally.
Use the included `docker-compose.yml` if you wish, and just write `docker-compose up`. 

Or, you can use just Docker, and run it manually:

```
docker run -e AZP_URL=<Azure DevOps instance url> \
  -e AZP_TOKEN=<PAT token> \
  -e AZP_AGENT_NAME=mydockeragent \
  -v /var/run/docker.sock:/var/run/docker.sock azure-devops-agent:latest
```

You need to map the docker socket to be able to run docker builds inside the container. It's not pretty, and it's a security risk, so do this at your own risk :)


