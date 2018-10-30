# Instructions for creating a Jetstream image

We've installed software on an image for you to load, so you don't have to waste time during the workshop installing. If you would like to install this same set of software on your own hpcc or on another machine, some of these steps may work. We have tested and know the software works on an Ubuntu 18.04 base installation machine. We cannot guarantee that these lessons will work on other operating systems.

Launch instance: “Ubuntu 18.04 Devel and Docker” base image (Oct 1, 2018 by jfischer), m1.large (CPU: 10, Mem: 30 GB, Disk: 60 GB)

Allocation Source: TG-MCG180142

https://angus.readthedocs.io/en/2018/jetstream-bioconda-config.html

Install in `/opt`, per [Jetstream installation instructions](https://iujetstream.atlassian.net/wiki/spaces/JWT/pages/17465521/Imaging+Guidelines):

```
sudo chmod a+w /opt
mkdir /opt/rnaseq
```
Download and install [miniconda](https://conda.io/docs/user-guide/install/linux.html):
```
cd /opt/rnaseq
curl -O -L https://repo.continuum.io/miniconda/Miniconda3-latest-Linux-x86_64.sh
bash Miniconda3-latest-Linux-x86_64.sh #install in /opt/miniconda3
echo export PATH=$PATH:/opt/miniconda3/bin >> ~/.bashrc
source ~/.bashrc
conda config --add channels defaults
conda config --add channels conda-forge
conda config --add channels bioconda
```

Install R Studio:

```
sudo apt-get update && sudo apt-get -y install gdebi-core r-base
```
Then:
```
wget https://download2.rstudio.org/rstudio-server-1.1.453-amd64.deb
sudo gdebi -n rstudio-server-1.1.453-amd64.deb
```
Trimmomatic and other software:
```
source ~/.bashrc
conda install -y fastqc multiqc trimmomatic trinity time osfclient salmon khmer
sudo apt-get install tree sl
```
