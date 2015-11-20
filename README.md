# Apache Mesos on ARM (Raspberry Pi on Raspbian)

List of helpful resources on how to build and run Apache Mesos on a Raspberry Pi. The commands below are meant to be used on Raspbian.

## Prerequisits

Before we start, we need to install a few packages:
```
sudo apt-get install dh-autoreconf gcc-4.8 g++-4.8 cpp-4.8 \
  libapr1-dev libsvn-dev python-dev
```

We now need to updated the alternatives to the new gcc/g++ compiler:
```
sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-4.8 20
sudo update-alternatives --install /usr/bin/g++ g++ /usr/bin/g++-4.8 20
sudo update-alternatives --install /usr/bin/cc cc /usr/bin/gcc 30
sudo update-alternatives --set cc /usr/bin/gcc
sudo update-alternatives --install /usr/bin/c++ c++ /usr/bin/g++ 30
sudo update-alternatives --set c++ /usr/bin/g++
```

Increase Swap to 1 GB (otherwise compilation will run out of RAM):
```
sudo su -c 'echo "CONF_SWAPSIZE=1024" > /etc/dphys-swapfile'
sudo dphys-swapfile setup
sudo dphys-swapfile swapon
```

Set `JAVA_HOME`:
```
export JAVA_HOME=/usr/lib/jvm/jdk-8-oracle-arm-vfp-hflt/
```

## Clone
```
git clone https://github.com/rpi-cloud/mesos-on-arm.git
```

## Build
```
# Change working directory.
cd mesos-on-arm

# Bootstrap (Only required if building from git repository).
./bootstrap

# Configure and build.
mkdir build
cd build
../configure
make
```

Since the build will take a while (> 4 hours), run the `make` command in a `screen`/`tmux` session. Detach & and enjoy life! 

## Run
```
# Change into build directory.
cd build

# Start mesos master (Ensure work directory exists and has proper permissions).
./bin/mesos-master.sh --ip=127.0.0.1 --work_dir=/var/lib/mesos

# Start mesos slave.
./bin/mesos-slave.sh --master=127.0.0.1:5050

# Visit the mesos web page.
curl http://127.0.0.1:5050
```

Unfortunatelly the running part is not working at the moment. You should see something like that when you try to start the Mesos master:

```
F1011 04:18:06.323731  7467 leveldb.cpp:160] Check failed: leveldb::BytewiseComparator()->Compare(one, two) < 0 
*** Check failure stack trace: ***
    @ 0x762f0bb8  google::LogMessage::Fail()
    @ 0x762f2bc4  google::LogMessage::SendToLog()
    @ 0x762f0760  google::LogMessage::Flush()
    @ 0x762f3528  google::LogMessageFatal::~LogMessageFatal()
    @ 0x76195b30  mesos::internal::log::LevelDBStorage::restore()
    @ 0x76205a94  mesos::internal::log::ReplicaProcess::restore()
    @ 0x76206260  mesos::internal::log::ReplicaProcess::ReplicaProcess()
    @ 0x76206440  mesos::internal::log::Replica::Replica()
    @ 0x7619a1cc  mesos::internal::log::LogProcess::LogProcess()
    @ 0x7619a518  mesos::internal::log::Log::Log()
    @    0x213fc  main
    @ 0x75228294  (unknown)
Aborted
```

# References

* Basic documentation
  https://github.com/strus38/rpi-mesos
* Increase swap file size
  https://www.raspberrypi.org/forums/viewtopic.php?f=26&t=46472
* CC Fix
  https://issues.apache.org/jira/browse/MESOS-3085
* Mesos on ARM Fork
  https://github.com/lyda/mesos-on-arm
