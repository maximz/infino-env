This is a development environment for infino.

# Install

1. Clone the code into your home directory: `git clone git@github.com:hammerlab/immune-infiltrate-explorations.git`
2. Set up a shared model cache that is world-readable: `sudo mkdir -p /data/modelcache_new && sudo chmod -R 777 /data/modelcache_new`
3. Pull docker image: `docker pull hammerlab/infino-env:latest`

# Use

1. Run: `docker run -d --name [name your container here] -v $HOME/immune-infiltrate-explorations:/home/jovyan/work -v /data/modelcache_new:/home/jovyan/modelcache -p [put port that you have forwarded here]:8888 --user root -e NB_UID=$(id -u) -e NB_GID=$(id -g) hammerlab/infino_env:latest`
2. Navigate to that port that you have set up to forward -- you will see a jupyter notebook server.
3. To commit code, use `git` from the host (as opposed to from inside the docker container).

This command mounts your personal code directory and the shared model cache, and acts as your user account for all editing purposes.

# More options available

You can get a shell into the container in the following ways:

* `docker run -it --name [name your container here] -v $HOME/immune-infiltrate-explorations:/home/jovyan/work -v /data/modelcache_new:/home/jovyan/modelcache -p [put port that you have forwarded here]:8888 --user root -e NB_UID=$(id -u) -e NB_GID=$(id -g) hammerlab/infino_env:latest bash` (if you are starting a new container)
* `docker exec -it [name your container here] bash` (shell into an existing container)

Teardown:

```
docker stop [name of your container here] # restart with "docker start"
docker rm [name of your container here]
```

Mounted directories will be unaffected because they live on the host.


# How the image works

This is derived from the Jupyter "data science notebook" image:

* [Docker hub page](https://hub.docker.com/r/jupyter/datascience-notebook/)
* [Dockerfile](https://github.com/jupyter/docker-stacks/blob/master/datascience-notebook/Dockerfile)

You can use any options from that image, e.g. you can grant sudo access by adding `-e GRANT_SUDO=yes`.

Our image installs pip and conda requirements from the primary repository into a python3 environment. Here is how to get those if you want to update the image:

```
wget https://raw.githubusercontent.com/hammerlab/immune-infiltrate-explorations/master/model-single-origin-samples/biokepi/conda_requirements.txt
wget https://raw.githubusercontent.com/hammerlab/immune-infiltrate-explorations/master/model-single-origin-samples/biokepi/pip_requirements.txt
```

To build: `docker build -t hammerlab/infino_env:latest .`

TODOs:

* Fetch the requirements files during docker build.
* the Dockerfile does not currently install pyensembl requirements (`pyensembl install --release 79 --species homo_sapiens`).

