# k8s-quagga

Alpine linux running quagga zebra and ospfd.
Config done by small go binary that translates environment to files

## Usage ##
### Global flags ###
```K8S_QUAGGA_OUTPUT``` - Directory to put files to defaults to ```/etc/quagga```

```K8S_QUAGGA_PASSWORD``` - Password to put in config defaults to ```changeme```

### Subcommands ###
```zebra``` - Print out zebra.conf

```ospfd``` - Print out ospfd.conf

### Ospfd Flags ###
```K8S_QUAGGA_INTERFACE``` - Interface to use

```K8S_QUAGGA_ROUTERID``` - RouterId to use

```K8S_QUAGGA_PORTALNET``` - Net 1 to announce

```K8S_QUAGGA_CONTAINERNET``` - Net 2 to announce

### Example Execution ###
In this example `10.254.0.0/24` is the k8s service network and `192.168.4.0/24` is the worker node network.

```
sudo docker run -d --privileged --net=host -e="K8S_QUAGGA_INTERFACE=enp0s25" \
-e="K8S_QUAGGA_PORTALNET=10.254.0.0/16" -e="K8S_QUAGGA_CONTAINERNET=10.254.0.0/16" \
-e="K8S_QUAGGA_PORTALGW=192.168.4.4" -e="K8S_QUAGGA_HOMENET=192.168.4.0/24" \
-e="K8S_QUAGGA_ROUTERID=192.168.4.4" -e="K8S_QUAGGA_PASSWORD=opsf1" \
--name=ospf docker.io/schwarzm/k8s-quagga:latest
```
