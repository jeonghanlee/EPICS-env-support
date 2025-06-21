# AreaDetector Modules Configuration for EPICS-env

In order to use this repository, one must setup the EPICS environment according to the following site.

https://github.com/jeonghanlee/EPICS-env

## Packages

Only the following Linux System Packages are used. I don't use the `ADSupport`.
The following packages will be updated according to our application requirements.

```
apt install libxml2-dev libhdf5-dev libjpeg-dev libtiff-dev libz-dev libusb-1.0-0-dev

```


## How to install

```
echo "INSTALL_LOCATION=/home/jeonglee/epics/1.1.1/debian-12/7.0.7/base" > configure/CONFIG_SITE.local
make init
make conf
make build
make symlinks
```

## The follow modules will be installed within EPICS-env

```
ADCore
ADSimDetector
ADGenICam
ADVimba
```
