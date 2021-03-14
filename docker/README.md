# Docker instructions

The Dockerfile in this directory can be used to build a docker container with a Python & Jupyter environment, including all necessary data files to run protocol.ipynb. 

In the directory containing the Dockerfile, run:

    > docker build -t anitagraser/eda-protocol-movement-data .

Once the build is finished you can run it using:

    > docker run -p 8888:8888 anitagraser/eda-protocol-movement-data

Jupyter noteboook will start up and you can access protocol.ipynb in your browser. 


## Creating an image

Usually, all changes are discarded when a container is stopped. In order to preserve changes and to be able to share a Docker image with a pre-rendered protocol.ipynb and all intermediary results, we need to create an image and export it to a file.

First, we need to find the ID of the running container:

    > docker ps
    CONTAINER ID   IMAGE                                    COMMAND                  CREATED             STATUS              PORTS                    NAMES
    d9e7390e5ada   anitagraser/eda-protocol-movement-data   "tini -g -- start-no…"   About an hour ago   Up About a minute   0.0.0.0:8888->8888/tcp   pedantic_lehmann

Using this ID and docker commit, we can create an image from the container:

    > docker commit d9e7390e5ada
    sha256:42818cd88a222dc0897140cc0514aac7607c95b64c7cd7eb613f0f79b0f4f646

Using docker images, we can now see the newly created image:

    > docker images
    REPOSITORY                               TAG       IMAGE ID       CREATED             SIZE
    <none>                                   <none>    42818cd88a22   28 seconds ago      7.98GB
    anitagraser/eda-protocol-movement-data   latest    e6813be02eb1   About an hour ago   6.26GB

Using docker tag, we can name the image we just created: 

    > docker tag 42818cd88a22 eda-protocol-movement-data-rendered

    > docker images
    REPOSITORY                               TAG       IMAGE ID       CREATED              SIZE
    eda-protocol-movement-data-rendered      latest    42818cd88a22   About a minute ago   7.98GB
    anitagraser/eda-protocol-movement-data   latest    e6813be02eb1   About an hour ago    6.26GB

Finally, we can save the image to a tar file: 

    > docker save -o eda-protocol-movement-data-1.0.tar eda-protocol-movement-data-rendered

The .tar file is quite large. Gzip it to reduce the size. 

