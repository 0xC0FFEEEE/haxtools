# Haxtools [![Docker](https://github.com/0xC0FFEEEE/haxtools/actions/workflows/docker-ci.yml/badge.svg)](https://github.com/0xC0FFEEEE/haxtools/actions/workflows/docker-ci.yml)

***A highly customised CTF, bug bounty hunting & penetration testing environment based on the official kali-rolling docker image.***

Haxtools provides a wrapper for the [haxtools docker image](https://hub.docker.com/repository/docker/infosux/haxtools/general) (and any other docker image for that matter) by sourcing itself within your shells rc file.

Installation is straightforward and doesn't require any prior experience with docker (though this will help if you wish to further customize Haxtools or add your own custom commands).

# Get Started
**1.** Install docker

**2.** Clone this repository:
```
git clone https://github.com/0xC0FFEEEE/haxtools ~/.haxtools
```

**3.** Add the following to your shells RC file:
```
# Haxtools
export HAXTOOLS_VERSION=cli # Set this to 'gui' to use the larger image equipped with GUI apps
export HAXTOOLS_DIR=$HOME/.haxtools
if [[ -s $HAXTOOLS_DIR/.haxrc ]]; then
  source $HAXTOOLS_DIR/.haxrc
fi
```

# Usage

Run the following command To launch the haxtools container within your terminal and start hacking:

```
hax
```

To keep your container and haxtools home environment up to date, run:

```
hax update
```

Additional resources such as seclists, priv-esc detection scripts and other tools and frameworks intended to be dropped on a remote target are not included by default to keep the image size to a minimum.

You can install these optional extras in to your haxtools instances home directory by running:

```
hax [-ioe|--install-optional-extras]
```
*This will occupy roughly an additional 2GB*.

## OpenVPN Support

Connecting to OpenVPN at runtime is supported. Just rename your ovpn file to `config.ovpn` and place it in the `.haxtools` directory.

## GUI Apps

In order to keep the default image size as small as possible, the default image does not come with any GUI tools installed. To change this, replace `HAXTOOLS_VERSION=cli` with `HAXTOOLS_VERSION=gui` in your shells rc file.

Try running a GUI application from the haxtools CLI, or by calling `hax <app>` from the host. On Linux & Windows via WSL it should 'Just work™'. For example:

```
hax burpsuite
```

*For GUI apps on macOS; some setup using XQuartz to provide the X window system will be required. See [macOS](docs/macOS.md) for more info.*

## Customization

You can customise the shell environment to your liking from within haxtools. The containers `/home/hax` directory is mounted to the `$HOME/.haxtools/home` directory on your host, so any changes to the Haxtools home directory are persistent.

### Mount additional directories

Add additional volume mounts to the `HAXTOOLS_VOLUMES` environment variable within the `.haxrc` file to mount additional host directories in the container at runtime.


# Multi-Architecture Support

Haxtools has native support for the following architectures:

* amd64
* arm64

Docker will pull the appropriate build for whatever you're running, whether its a Raspberry Pi 4, MacOS (Intel or Apple Silicon), Windows or Linux.

### Regular Builds

Fresh builds are produced on a weekly basis.

From time to time something breaks. Shit happens, I'll do my best to keep the wheels spinning.


# Contributions & Requests

Requests for tools and contributions to improve haxtools and the associated docker container are welcome.
