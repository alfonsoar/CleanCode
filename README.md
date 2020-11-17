# CleanCode

Notes for Clean Code

## Local set-up

1. Ensure you ave docker installed. If not checkout the installation
   [steps](https://docs.docker.com/get-docker/).
2. Install the GitPitch Trial mode docker image. More details [here](https://docs.gitpitch.com/#/free-trial)
   ```bash
   docker pull gitpitch/trial
   ```
3. Run the Docker image using the command below. More details [here](https://docs.gitpitch.com/#/desktop/launch?id=use-docker-run)
   ```bash
   docker run -it -v <INSERT A LOCAL DIR HERE>:/repo -p 9000:9000 gitpitch/trial
   ```
