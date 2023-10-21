---
uid: InstallUsingDocker
---

# Installation using Docker

Docker is a set of tools that you can use on Linux to manage application deployments. This topic provides examples of how to create a Docker container with the adapter.

**Note:** The use of Docker is only recommended if your environment requires it. Only users proficient with Docker should use it to install the adapter. Docker is not required to use the adapter.

## Create a startup script

To create a startup script for the adapter, follow the instructions below.

1. Use a text editor to create a script similar to one of the following examples:

    **Note:** The script varies slightly by processor.

    <!-- PRERELEASE REMINDER: Update {adapter} and {version} placeholders. Example: bacnet, 1.1.0.192 -->
    
    **ARM32**

    ```bash
    #!/bin/sh
    if [ -z $portnum ] ; then
<<<<<<< HEAD
        exec /PI-Adapter-for-StructuredDataFiles_1.0.0.138-arm_/OSIsoft.Data.System.Host
    else
        exec /PI-Adapter-for-StructuredDataFiles_1.0.0.138-arm_/OSIsoft.Data.System.Host --port:$portnum
=======
        exec /OpcUa_linux-arm_/OSIsoft.Data.System.Host
    else
        exec /OpcUa_linux-arm_/OSIsoft.Data.System.Host --port:$portnum
>>>>>>> 61fa1904844108ba597ef7bf338ec2f9a8d4c241
    fi
    ```

    **ARM64**

    ```bash
    #!/bin/sh
    if [ -z $portnum ] ; then
<<<<<<< HEAD
        exec /PI-Adapter-for-StructuredDataFiles_1.0.0.138-arm64_/OSIsoft.Data.System.Host
    else
        exec /PI-Adapter-for-StructuredDataFiles_1.0.0.138-arm64_/OSIsoft.Data.System.Host --port:$portnum
=======
        exec /OpcUa_linux-arm64_/OSIsoft.Data.System.Host
    else
        exec /OpcUa_linux-arm64_/OSIsoft.Data.System.Host --port:$portnum
>>>>>>> 61fa1904844108ba597ef7bf338ec2f9a8d4c241
    fi
    ```

    **AMD64**
            
    ```bash
    #!/bin/sh
    if [ -z $portnum ] ; then
<<<<<<< HEAD
        exec /PI-Adapter-for-StructuredDataFiles_1.0.0.138-x64_/OSIsoft.Data.System.Host
    else
        exec /PI-Adapter-for-StructuredDataFiles_1.0.0.138-x64_/OSIsoft.Data.System.Host --port:$portnum
=======
        exec /OpcUa_linux-x64_/OSIsoft.Data.System.Host
    else
        exec /OpcUa_linux-x64_/OSIsoft.Data.System.Host --port:$portnum
>>>>>>> 61fa1904844108ba597ef7bf338ec2f9a8d4c241
    fi
    ```

2. Name the script `sdfdockerstart.sh` and save it to the directory where you plan to create the container.


## Create a Docker container

To create a Docker container that runs the adapter, follow the instructions below.

1. Create the following `Dockerfile` in the directory where you want to create and run the container.

    **Note:** `Dockerfile` is the required name of the file. Use the variation according to your operating system:

    **ARM32**
    
    ```dockerfile
    FROM ubuntu:20.04
    WORKDIR /
    RUN apt-get update && DEBIAN_FRONTEND=noninteractive apt-get install -y ca-certificates libicu66 libssl1.1 curl
<<<<<<< HEAD
    COPY sdfdockerstart.sh /
    RUN chmod +x /sdfdockerstart.sh
    ADD ./PI-Adapter-for-StructuredDataFiles_1.0.0.138-arm_.tar.gz .
    ENTRYPOINT ["/sdfdockerstart.sh"]
=======
    COPY {adapter}dockerstart.sh /
    RUN chmod +x /{adapter}dockerstart.sh
    ADD ./OpcUa_linux-arm_.tar.gz .
    ENTRYPOINT ["/{adapter}dockerstart.sh"]
>>>>>>> 61fa1904844108ba597ef7bf338ec2f9a8d4c241
    ```

    **ARM64**

    ```dockerfile
    FROM ubuntu:20.04
    WORKDIR /
    RUN apt-get update && DEBIAN_FRONTEND=noninteractive apt-get install -y ca-certificates libicu66 libssl1.1 curl
<<<<<<< HEAD
    COPY sdfdockerstart.sh /
    RUN chmod +x /sdfdockerstart.sh
    ADD ./PI-Adapter-for-StructuredDataFiles_1.0.0.138-arm64_.tar.gz .
    ENTRYPOINT ["/sdfdockerstart.sh"]
=======
    COPY {adapter}dockerstart.sh /
    RUN chmod +x /{adapter}dockerstart.sh
    ADD ./OpcUa_linux-arm64_.tar.gz .
    ENTRYPOINT ["/{adapter}dockerstart.sh"]
>>>>>>> 61fa1904844108ba597ef7bf338ec2f9a8d4c241
    ```
    
	**AMD64 (x64)**

    ```dockerfile
    FROM ubuntu:20.04
    WORKDIR /
    RUN apt-get update && DEBIAN_FRONTEND=noninteractive apt-get install -y ca-certificates libicu66 libssl1.1 curl
<<<<<<< HEAD
    COPY sdfdockerstart.sh /
    RUN chmod +x /sdfdockerstart.sh
    ADD ./PI-Adapter-for-StructuredDataFiles_1.0.0.138-x64_.tar.gz .
    ENTRYPOINT ["/sdfdockerstart.sh"]
=======
    COPY {adapter}dockerstart.sh /
    RUN chmod +x /{adapter}dockerstart.sh
    ADD ./OpcUa_linux-x64_.tar.gz .
    ENTRYPOINT ["/{adapter}dockerstart.sh"]
>>>>>>> 61fa1904844108ba597ef7bf338ec2f9a8d4c241
    ```

2. Copy the <code>[!include[installer](../_includes/inline/installer-name.md)]-<var>platform</var>_.tar.gz</code> file to the same directory as the `Dockerfile`.

3. Copy the <code>[!include[startup-script](../_includes/inline/startup-script.md)]</code> script to the same directory as the `Dockerfile`.

4. Run the following command line in the same directory (`sudo` may be necessary):

    ```bash
    docker build -t sdfadapter .
    ```

## Docker container startup

The following procedures contain instructions on how to run the adapter inside a Docker container with different options enabled.

### Run the Docker container with REST access enabled

To run the adapter inside a Docker container with access to its REST API from the local host, complete the following steps:

1. Use the docker container image <code>[!include[docker-image](../_includes/inline/docker-image.md)]</code> created previously.

2. Type the following in the command line (`sudo` may be necessary):

    ```bash
    docker run -d --network host sdfadapter
    ```

Port `5590` is accessible from the host and you can make REST calls to the adapter from applications on the local host computer. In this example, all data stored by the adapter is stored in the container itself. When you delete the container, the stored data is also deleted.

### Run the Docker container with persistent storage

If you have a file share directory `/sdf/InputDirectory` and you want to move the files to `/sdf/OutputDirectory` after processing, for the Docker container to access these directories and the storage on the host machine, complete the following steps to run the container:

1. Use the docker container image <code>[!include[docker-image](../_includes/inline/docker-image.md)]</code> created previously.

2. Enter the following command line (you may need to use the `sudo` command):

    ```bash
    docker run -d --network host -v /sdf:/usr/share/OSIsoft/ sdfadapter
    ```

3. Update the `InputDirectory` and `OutputDirectory` of your data source configuration to following settings:

    ```json
    {
      "InputDirectory": "/usr/share/OSIsoft/InputDirectory",
      "OutputDirectory": "/usr/share/OSIsoft/OutputDirectory"
    }
    ```

    **Note:** `/sdf` is replaced by `/usr/share/OSIsoft`, the target directory inside the container.

The default port `5590` is accessible from the host and you can make REST calls to the adapter from applications on the local host computer. The data is written to a host directory on the local machine `/sdf` rather than the container.

### Change port number

To use a different port other than `5590`, you can specify a `portnum` variable on the `docker run` command line. For example, to start the adapter using port `6000` instead of `5590`, use the following command:

```bash
docker run -d -e portnum=6000 --network host sdfadapter
```

This command accesses the REST API with port `6000` instead of port `5590`. The following `curl` command returns the configuration for the container.

```bash
curl http://localhost:6000/api/v1/configuration
```

### Remove REST access

If you remove the `--network host` option from the docker run command, REST access is not possible from outside the container. This may be of value where you want to host an application in the same container as the adapter but do not want to have external REST access enabled.
