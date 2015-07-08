# dockerize

*modulus docker talk notes*

##### good dockerfile

    FROM ubuntu:14.04.02 
    MAINTAINER Aaron Brongersma
    RUN apt-get update \ 
    apt-get install -y curl git \
    build-essential nginx=1.8.0 \ 
    imagemagick php5 && \ 
    apt-get clean
    CMD [“nginx”, “-g”, “daemon off;”] 
    EXPOSE 80

##### dockerfile

* single responsibility
* use backslash \ the separate lines
* pin down versions (docker v162, lock down so don't get stung when upgrade to v167)
* install and cleanup need to be in the same layer
* break out sections (php/imagemagick) that will be updated frequently, add a cheat line to force a rebuild

###### notes

* tend to remove apt-get upgrade as it'll break things, apt-get update is safe to do
* version pinning apt-get install -y nginx=1.8.0
* apt-get clean as last process

##### volumes

* mount volumes from host
* -v /host/files:/container/files
* Thin provisioning is essential, LVM or ZFS
* Data volume containers are still improving
* consider flocker / rancher for volume management

##### container logs

* Log to stderr and stdout => ln -sf /dev/stdout /var/log/{logfile}.log
* Phusion/baseimage-docker has syslog
* Logspout, ELK (Elasticsearch, logstash,
kibana)
* docker log plugins coming soon

##### storage

* aufs for speed
* device manager for isolation
* cheat, each container gets it's own 2gb lvm volume
* Wrap the Docker run command with a storage tool, provision storage before starting a container
* watch out for vulnerabilities (aufs / overlayfs - prevented by apparmour)
* ZFS and storage plugins look awesome!

##### memory limits

* constrain then monitor for inconsistencies
* restart memory hogs
* -m 100M --oom-kill-disable / linux oom out-of-memory killer is aggresive
* cgroup_enable=memory swapaccount=1 / passed as kernel param to grub.conf & menu.lst

##### monitoring

* Monitor hosts like any critical infrastructure
* Github.com/google/cadvisor is a good start for container stats
* “docker stats {containerId}” GET /container/{containerId}/stats
* InfluxDB or Prometheus for the DIY crowd
* Consider DataDog, Sysdig or Newrelic for hosted monitoring

##### service discovery

* treat containers like cattle not pets
* Use an ambassador container when linking => svendowideit/ambassador
* enable tls for docker
* -- etcd, consul, zookeeper, swarm

##### container versions

* tag early tag often

##### standardization

* Config, source, and log locations
* Create a base image for your Dockerfile to inherit from; shared libraries, common packages, etc
* Docker Compose for complex multi-container setups
* Each project gets its own Dockerfile
* Container versioning is important, tag early and often

##### immutable infrastructure

* No cowboy coding inside a container
* Containers should be able to be destroyed without negative impact
* Creating an immutable “slug” makes deployments and scaling easy, with no moving pieces
* Make a change? Rebuild the image, retag, pull, run

##### security

* Apparmor or SELinux is a must
* Unprivileged mode, non-root users
* Base distributions can slowly get out of date, make sure you’re rebuilding with the latest version of your base distro as possible
* Disable SUID applications
* for i in `find / -perm +6000 -type f`; do chmod a-s $i; done

##### take it slow

* Docker is still under heavy development and so are many projects in the Docker ecosystem
* Version pin your Docker engine
* Try try again, be prepared to repeat operations
* allocate_port: listen tcp 0.0.0.0:49480: bind: address already in use

#### to recap

* Single responsibility containers
* Consolidate logs
* Hosts still require monitoring, backups, security
* Immutable infrastructure
* Service discovery
* Version pinning
