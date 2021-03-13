ARG BASE_CONTAINER=jupyter/minimal-notebook
FROM $BASE_CONTAINER
LABEL author="Anita Graser"

# Installations
USER root
RUN apt update  &&  sudo apt install --assume-yes p7zip-full #Ubuntu
RUN conda install -c conda-forge movingpandas

# Switch back to jovyan to avoid accidental container runs as root
USER $NB_UID

# Download notebook 
RUN wget https://raw.githubusercontent.com/anitagraser/EDA-protocol-movement-data/v1.0/protocol.ipynb 
RUN mv protocol.ipynb work/protocol.ipynb 

# Download data 
RUN mkdir work/data  
RUN wget https://ndownloader.figshare.com/files/26782541
RUN 7z e 26782541
RUN mv aisdk_20180101.csv work/data/aisdk_20180101.csv
RUN rm 26782541
RUN wget https://ndownloader.figshare.com/files/26782631
RUN 7z e 26782631
RUN mv aisdk_20170701.csv work/data/aisdk_20170701.csv
RUN rm 26782631
