# Install Home Assistant Supervised

As an alternative to using the images which include the Home Assistant operating-system and Docker, it is also possible to run Home Assistant on a generic system running another Linux of your choice such as Ubuntu, Debian, etc as Supervised. Because of all the various possible install options, these are more of a community supported installation choice. It follows that the more esoteric of a choice made with the OS, the less a user will find in terms of information and support from the community.

## Warning

The Supervisord system is designed to provide a full-featured environment that is comparable with Kubernetes, which is also a bad idea to run it by the side of another orchestrator on the same Host. The Supervisor is also not caring for other software they run on your Host, and it can affect things bad on both sides. You also need to know that the Home Assistant OS runs with less overhead on your Proxmox or other Hypervisor as if you install first Debian and Ubuntu. In most cases, it's not the best choice to run the Supervisord on top of a Linux, if you not 100% sure what you do. **It is not just a container inside Docker!**

## Requirements

We only support Linux distributions that follow the [FHS 3.0](https://en.wikipedia.org/wiki/Filesystem_Hierarchy_Standard)!

```
docker-ce
bash
jq
curl
avahi-daemon
dbus
```

## Optional

```
apparmor-utils
network-manager
```

**Important**: Don't only install NetworkManager, you need also use it on your system.

## Run

Run as root (sudo su):

```bash
curl -sL https://raw.githubusercontent.com/calebmah/supervised-installer/master/installer.sh | bash -s
```

### Command line arguments
| argument           | default                                                                                                                                                                             | description                                            |
|--------------------|----------------------|--------------------------------------------------------|
| -m \| --machine    |                      | On a special platform they need set a machine type use |
| -d \| --data-share | $PREFIX/share/hassio | data folder for hass.io installation                   |
| -p \| --prefix     | /usr                 | Binary prefix for hass.io installation                 |
| -s \| --sysconfdir | /etc                 | Configuration directory for hass.io installation       |

you can set these parameters by appending ` -- <parameter> <value>` like:

```bash
curl -sL https://raw.githubusercontent.com/calebmah/supervised-installer/master/installer.sh | bash -s -- -m MY_MACHINE
```

## Supported Machine types

- intel-nuc
- odroid-c2
- odroid-n2
- odroid-xu
- qemuarm
- qemuarm-64
- qemux86
- qemux86-64
- raspberrypi
- raspberrypi2
- raspberrypi3
- raspberrypi4
- raspberrypi3-64
- raspberrypi4-64
- tinker

## Troubleshooting

If somethings going wrong, use `journalctl -f` to get your system logs. If you are not familiar with Linux and how you can fix issues, we recommend do use our Home Assistant OS.

## Alternative: install Home Assistant Supervised on a generic Linux host	

You can also install Home Assistant on a Linux operating system of choice, called Home Assistant Supervised.	

Home Assistant Supervised, will still give you access to most features Home Assistant has to offer, including add-ons.	

<div class='note warning'>	

The Supervisord system is designed to provide a full-featured environment that is comparable with Kubernetes, which is also a bad idea to run it by the side of another orchestrator on the same Host. The Supervisor is also not caring for other software they run on your Host, and it can affect things bad on both sides. You also need to know that the Home Assistant OS runs with less overhead on your Proxmox or other Hypervisor as if you install first Debian and Ubuntu. In most cases, it's not the best choice to run the Supervisord on top of a Linux, if you not 100% sure what you do. **It is not just a container inside Docker!**	

</div>	

### Supported systems and limitations	

While Home Assistant Supervised can be run on practically any Linux systems,	
the Home Assistant project limits support for this installation method.	

Only the use of Debian or Ubuntu is supported. Other Linux-based system may work	
but is not part of our testing and thus not supported.	

Furthermore, if you choose to run Home Assistant Supervised, the operating	
system of your choosing (including Debian/Ubuntu) is **your** responsibility.	
Both in terms of systems upgrade and system configuration.	

Customizations to your custom operating system may interfere with Home Assistant.	
For that reason, please be sure you have to knowledge to manage, configure and	
maintain the operating system of your choosing.	

When in doubt, we highly recommend using the regular installation of Home	
Assistant as provided above. In that case, Home Assistant will manage and update	
the Home Assistant Operating System for you.	

### Preparation	

To prepare your machine for the Home Assistant installation, run the following commands:	

If you run Ubuntu, first run this command:	

```bash	
sudo add-apt-repository universe	
```	

Next run the following commands (for both Debian and Ubuntu):	

```bash	
sudo -i	
apt-get update	
apt-get install -y software-properties-common apparmor-utils apt-transport-https avahi-daemon ca-certificates curl dbus jq network-manager socat	
systemctl disable ModemManager	
systemctl stop ModemManager	
curl -fsSL get.docker.com | sh	
```	

### Installation of Home Assistant Supervised	

The following script will then install Home Assistant on a variety of operating systems and machine types.	

```bash	
curl -sL "https://raw.githubusercontent.com/calebmah/supervised-installer/master/installer.sh" | bash -s	
```	

Some installation types require flags to identify the computer type, for example, when using a Raspberry Pi 4, the flag `-- -m raspberrypi4` is required. The install script would then look like this:	

```bash	
curl -sL "https://raw.githubusercontent.com/calebmah/supervised-installer/master/installer.sh" | bash -s -- -m raspberrypi3	
```	

#### Other machine types	

- `intel-nuc`	
- `raspberrypi`	
- `raspberrypi2`	
- `raspberrypi3`	
- `raspberrypi3-64`	
- `raspberrypi4`	
- `raspberrypi4-64`	
- `odroid-c2`	
- `odroid-n2`	
- `odroid-xu`	
- `tinker`	
- `qemuarm`	
- `qemuarm-64`	
- `qemux86`	
- `qemux86-64`	

See the [installer](https://github.com/calebmah/supervised-installer) GitHub page for an up-to-date listing of supported machine types.	

If you can not find your machine type in the list, you should pick the `qemu` release. i.e., `qemux86-64` for a normal 64-bit Linux distribution, or `qemuarm-64` for most modern ARM-based target like Raspberry Pi clones, or TV boxes.

## Installing portainer

Assuming you are on a PC with Ubuntu/Debian, or a Pi with Raspbian as the base OS, you can install Portainer to manage anything running in Docker.

```bash	
docker run -d -p 9000:9000 --name=portainer --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data portainer/portainer
```

You’ll then access Portainer at YOUR_MACHINE_IP:9000