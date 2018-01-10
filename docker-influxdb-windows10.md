# InfluxDB on Windows 10
## Windows 10 Build 1709 or higher

Most of this was pulled from this [Docker Hub link](https://hub.docker.com/_/influxdb/).

### Install Docker for Windows, Configure for Linux Containers
After install you should be able to run ```docker version``` in PowerShell and see a result similar to this:

![alt text](docker_version.png?raw=true "Docker Version results")

### Create the Directories for Your Container
In my build I'm persisting the config file and the data files to directories on my Windows file system. I used this layout:

```
DockerShared
-- influxdb
    -- data
    -- config
```

### Create a New Config File
Persisting the config file to your host's file system will make updating it much less of a hassle.

Using PowerShell, navigate to your config directory and run the command:
```
docker run --rm influxdb influxd config > influxdb.conf
```
This will generate a influxdb.conf file in your host file system. You can skip this step if you have an existing influxdb.conf file you want to use isntead of creating a new, factory default influxdb.conf file.

### Run the Container
There are several option flags used, here's what each of them do:

-p maps host port to container port like this ```-p host:container```

-v (first one) maps host config file to container config file

-v (second one) maps host data directory to container directory

--name specifies the container "friendly" name

-d tells docker to run the container in detached mode instead of interactive mode

-config tells InfluxDB to use the config file at the container's mapped location

```docker run -p 8086:8086 -v /D/DockerShared/influxdb/config/influxdb.conf:/etc/influxdb/influxdb.conf:ro -v /D/DockerShared/influxdb/data:/var/lib/influxdb --name influxdb -d influxdb -config /etc/influxdb/influxdb.conf```
