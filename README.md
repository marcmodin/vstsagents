# Azure Devops Build Agent Container

this repository contains the dockerfiles and scripts needed to create a selfhosted azp Ubuntu 18.04 build-agent.

Modified from microsofts documentation on how to contianerize a build agent.

_The original instructions can be found here as of 2020 https://docs.microsoft.com/en-us/azure/devops/pipelines/agents/docker?view=azure-devops_

### How to build:

- Generate an Azure Devops access token in your organisation and add it to a file `mysecret.txt` in the `/install` directory

- build the Dockerfile in `/install` first. tag it :scratch or whatever. Replace AZP_URL

  ```
  docker build --no-cache --progress=plain --secret id=mysecret,src=mysecret.txt --squash --build-arg AZP_URL=<server address> -t <repository username>/<respository>:bionic-scratch_<version> .
  ```

- push the scratch image to your container repository

  ```
  docker push <repository username>/<respository>:bionic-scratch_<version>
  ```

- edit the FROM statement in the Dockerfile in `/startagent` to pull the image from your repository

- build the Dockerfile in `/startagent` first. tag it :<version> corresponding with the scratch version

  ```
  docker build -t <repository username>/<respository>:bionic_<version>.
  ```

- push the agent image to your container repository
  ```
  docker push <repository username>/<respository>:bionic_<version>
  ```

### How to use the container

- Run the container. Replace AZP_URL, AZP_TOKEN with your own
  ```
  docker run -it -e AZP_URL=<server address> -e AZP_TOKEN=<your toke> --rm <respository>:bionic_<version>
  ```
### Backlog
- add ability to support other distibutions that are supported
- enable self removal of agent and clearn shutdown 
  
