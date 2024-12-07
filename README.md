# AreaDetector Core Modules Configuration for EPICS-env

## The follow modules will be installed within EPICS-env

```
ADCore
ADSimDetector
ADViewers
```

## How to 

```
echo "INSTALL_LOCATION=/home/jeonglee/epics/1.1.0/debian-12/7.0.7/base" > configure/CONFIG_SITE.local
make init
make conf
make build
make install
```

