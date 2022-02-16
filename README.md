<br />
<p align="center">
  <h3 align="center">Dockerize TensorFlow with GPU</h3>

  <p align="center">
    How to run a Tensorflow instance inside docker using GPU
    <br />
    <a href="https://www.tensorflow.org/install/docker"><strong>Read more about dockerizing Tensorflow »</strong></a>
    <br />
    <br />
    <a href="https://github.com/FernandoPerezLara/docker-tensorflow-gpu/">View Code</a>
    ·
    <a href="https://github.com/FernandoPerezLara/docker-tensorflow-gpu/issues">Report Bug</a>
  </p>
</p>
<br />

Throughout this document, it will be shown how to use TensorFlow in a Docker instance using the machine's GPUs.

The advantages of using Docker instead of using the machine directly are:
1. It is more secure, each project is inside an isolated container.
2. No machine resources are wasted, each container uses the kernel of the host machine.
3. It is not necessary to use internet for mounting libraries. An image of the machine can be created and exported as a `.tar` file for a later import on a machine without a network connection.
4. It is easier to install libraries and packages. This is done when creating the image and thus have everything mounted when executing the container.
5. Version problems between packages and libraries are eliminated. Each project will have its own version of TensorFlow, its corresponding CUDA version, and more.
6. No time wasted configuring and fixing package and library versioning.
7. If an instance is corrupted it will not affect the rest of the containers.
8. Much simpler when executing, with a simple command you have the program running.
9. It is more difficult for the computer to crash, if a problem ocurrurs, it is the container itself who stops its execution, preventing the rest of the containers to be affected.

## Installation
Note that the Docker installation must be done on the host machine using the steps taken from the [NVIDIA official website](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/install-guide.html).

The Docker daemon binds to a Unix socket instead of a TCP port. By default that Unix socket is owned by the user root and other users can only access it using sudo. The Docker daemon always runs as the root user.

If you don't want to preface the docker command with sudo, create a Unix group called docker and add users to it. When the Docker daemon starts, it creates a Unix socket accessible by members of the docker group.

```
sudo groupadd docker
sudo usermod -aG docker $USER
```

After Docker is installed, Docker Compose will be installed following the steps of the [official Docker website](https://docs.docker.com/compose/install/)

## How to use
Once Docker is installed and you want to start executing your program in a container, you will need the `Dockerfile` and `docker-compose.yml` files. These can be found in this repository.

```
folder
├── Dockerfile
├── docker-compose.yml
├── docker-compose.yml
├── requirements.txt
└── ···
```

### Installing a package
To install a package inside your container, before creating the image it will be necessary to modify the `Dockerfile` and add the corresponding package.

For example, if you want to add the `nano` text editor, add the following line:
```
RUN apt install nano -y
```

### Installation of libraries (makes use of pip)
To install the necessary libraries for your program to work correctly, you will have to import the `requirements.txt` file with the necessary libraries and versions.

It is not recommended to indicate the version due to possible conflicts.

### Create the image
The following commands will be launched being inside the path where the necessary files are found.

Once your project is set up, the image to be used will be created. To do this, it is necessary to change the `container_name` tag in the docker compose file to rename the image (for example: fernando_tensorflow:latest). Then, the following command is executed:
```
docker-compose up
```

If you want to re-import new libraries and packages, it will be necessary to delete the image and the container (in this example `fernando_tensorflow` will be used):
```
docker-compose rm
docker image rm fernando_tensorflow
```

When the container is no longer used, it is recommended to stop its execution using the following command:
```
docker-compose down
```

### Enter inside the container
To enter and execute your code inside the container, the following command will be issued (in this example `fernando_tensorflow_1` will be used):
```
docker exec -it fernando_tensorflow_1 /bin/bash
```

### Export and import an image
Once the image is built, you can export it using the followin command:
```
docker save -o image.tar fernando_tensorflow
```

If you want to import the exported image, use this command:
```
docker load -i image.tar
```
