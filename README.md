# Mesos on ARM

List of helpful resources on how to build and run Apache Mesos on a Raspberry Pi. The commands below are meant to be used on Raspbian. 

##Prerequisits
```
sudo apt-get install dh-autoreconf gcc-4.8 g++-4.8 cpp-4.8
```

##Clone
```
git clone https://github.com/rpi-cloud/mesos-on-arm.git
```

##Build
```
# Change working directory.
$ cd mesos-on-arm

# Bootstrap (Only required if building from git repository).
$ ./bootstrap

# Configure and build.
$ mkdir build
$ cd build
$ ../configure
$ make
```

##Run
```
# Change into build directory.
$ cd build

# Start mesos master (Ensure work directory exists and has proper permissions).
$ ./bin/mesos-master.sh --ip=127.0.0.1 --work_dir=/var/lib/mesos

# Start mesos slave.
$ ./bin/mesos-slave.sh --master=127.0.0.1:5050

# Visit the mesos web page.
$ http://127.0.0.1:5050
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
