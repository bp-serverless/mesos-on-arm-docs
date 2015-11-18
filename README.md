# cookbook-mesos-rpi

Dependencies needed on a Raspbian
```
sudo apt-get install dh-autoreconf 
```

Clone
```
git clone https://github.com/rpi-cloud/mesos-on-arm.git
```

Build
```
# Change working directory.
$ cd mesos

# Bootstrap (Only required if building from git repository).
$ ./bootstrap

# Configure and build.
$ mkdir build
$ cd build
$ ../configure
$ make
```


# References

Basic documentation
https://github.com/strus38/rpi-mesos

Increase swap file size
https://www.raspberrypi.org/forums/viewtopic.php?f=26&t=46472

CC Fix
https://issues.apache.org/jira/browse/MESOS-3085

Mesos on ARM Fork
https://github.com/lyda/mesos-on-arm
