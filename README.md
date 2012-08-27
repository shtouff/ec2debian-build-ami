# debian "wheezy" bootstrapping script for EC2 #

This is a fork of andsens script for EC2 AMIs.  
It creates a vanilla debian wheezy machine image, no latent logfiles no .bash_history or even apt package cache.  
The machine configuration this script creates has been thoroughly tested.

*This script is only tested on debian wheezy (and not terribly well).*
*You will need an EC2 server to run this bootstrapper.*

## Usage ##

The script is started with ``./ec2debian-build-ami`` it has sensible defaults but can be configured with options and plugins. To see a list of options run ``./ec2debian-build-ami --help``.  
At the very least, the script needs to know your AWS credentials.
I recommend setting them via an [environment script](#environment-script), this way you do not need to specify the parameters for every invocation.

There are no interactive prompts, the bootstrapping can run entirely unattended from start till finish.

Some plugins are included in the [plugins directory](https://github.com/sigil66/ec2debian-build-ami/tree/master/plugins).
A list of external plugins is also provided there.  
If none of those scratch your itch, you can of course [write your own plugin](https://github.com/sigil66/ec2debian-build-ami/blob/master/plugins/HOWTO.md).

## Features ##

### AMI features ###

* EBS booted
* xfs filesystem
* Ephemeral storage is properly mapped
* Standard ec2 startup scripts
* Uses standard debian Xen kernel from apt
* update-grub creates an actual menu.lst which pvGrub can read

### Bootstrapper features ###

* EBS volume is automatically created, mounted, formatted, unmounted, "snapshotted" and deleted
* AMI is automatically registered with the right kernels for the current region of the host machine
* Plugin system to keep the bootstrapping process automated
* Custom packages
* The process is divided into simple task based scripts
* Bootstrapping server mirror depends on AWS region
* APT source mirror depends on AWS region

## Environment script ##
Include with `source env-script` for the variables to be present on the commandline.
```
export EC2_URL='https://ec2.eu-west-1.amazonaws.com'
export EC2_HOME="/root/ec2/ec2-api-tools-1.5.2.3"
export EC2_AMITOOL_HOME="/root/ec2/ec2-ami-tools-1.4.0.5"
export EC2_PRIVATE_KEY="/root/root.key"
export EC2_CERT="/root/root.crt"
export AWS_USER_ID='1234-4567-8910'
export AWS_ACCESS_KEY_ID='SOM3L0NG4CC3SSK3Y000'
export AWS_SECRET_ACCESS_KEY='SomeBase64EncodedString'
export PATH="$PATH:${EC2_HOME}/bin:${EC2_AMITOOL_HOME}/bin"
```
If you are using IAM to access AWS you may need to create the certificate first. You can use [this gist](https://gist.github.com/2629062) for that.
