# dockerize

*modulus docker talk notes*

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
* consider flocker / rancher for volume management

##### container logs

* logsprout, elk
* docker log plugins coming soon, in 1.8
* log in stderr / stdout

##### storage

* aufs for speed
* device manager for isolation
* cheat, each container gets it's own 2gb lvm volume
* watch out for vulnerabilities (aufs / overlayfs - prevented by apparmour)

##### memory limits

* constrain then monitor for inconsistencies
* restart memory hogs
* -m 100M --oom-kill-disable / linux oom out-of-memory killer is aggresive
* cgroup_enable=memory swapaccount=1 / passed as kernel param to grub.conf & menu.lst

