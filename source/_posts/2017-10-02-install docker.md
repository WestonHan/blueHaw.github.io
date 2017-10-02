---
layout: post
title: install docker
date: 2017-10-02 19:34:58
category: [docker]
tags: [docker]
---

### 安装docker：
#### 1. Uninstall old versions
```bash
$ sudo yum remove docker \
                  docker-common \
                  docker-selinux \
                  docker-engine
```
<!--more -->
#### 2. Install Docker CE
You can install Docker CE in different ways, depending on your needs:

* Most users `set up Docker’s repositories` and install from them, for ease of installation and upgrade tasks. This is the recommended approach.
* Some users download the RPM package and `install it manually` and manage upgrades completely manually. This is useful in situations such as installing Docker on air-gapped systems with no access to the internet.
* In testing and development environments, some users choose to use automated `convenience scripts` to install Docker.

##### Install using the repository
Before you install Docker CE for the first time on a new host machine, you need to set up the Docker repository. Afterward, you can install and update Docker from the repository.
###### Set up the repository
1. Install required packages.
```
$ sudo yum install -y yum-utils device-mapper-persistent-data lvm2
```
2. Use the following command to set up the **stable** repository. You always need the **stable** repository, even if you want to install builds from the **edge** or **test** repositories as well.
```
$ sudo yum-config-manager \
    --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo
```
3. **Optional**: Enable the **edge** and **test** repositories. These repositories are included in the docker.repo file above but are disabled by default. You can enable them alongside the stable repository.
```
$ sudo yum-config-manager --enable docker-ce-edge
```
```
$ sudo yum-config-manager --enable docker-ce-test
```
You can disable the **edge** or **test** repository by running the yum-config-manager command with the `--disable` flag. To re-enable it, use the `--enable` flag. The following command disables the **edge** repository.
```
$ sudo yum-config-manager --disable docker-ce-edge
```
>**Note**: Starting with Docker 17.06, stable releases are also pushed to the **edge** and **test** repositories.
###### Install Docker CE
1. Update the `yum` package index.
```
$ sudo yum makecache fast
```
2. Install the latest version of Docker CE, or go to the next step to install a specific version.
```
sudo yum install docker-ce
```
><span style="color:#CE4844">Warning: If you have multiple Docker repositories enabled, installing or updating without specifying a version in the `yum install` or `yum update` command will always install the highest possible version, which may not be appropriate for your stability needs.</span>

3. On production systems, you should install a specific version of Docker CE instead of always using the latest. List the available versions. This example uses the `sort -r` command to sort the results by version number, highest to lowest, and is truncated.
>**Note**: This yum list command only shows binary packages. To show source packages as well, omit the .x86_64 from the package name.
```
$ yum list docker-ce.x86_64  --showduplicates | sort -r

docker-ce.x86_64  17.06.0.el7                              docker-ce-stable
```
The contents of the list depend upon which repositories are enabled, and will be specific to your version of CentOS (indicated by the .el7 suffix on the version, in this example). 

Choose a specific version to install. The second column is the version string. The third column is the repository name, which indicates which repository the package is from and by extension its stability level. To install a specific version, append the version string to the package name and separate them by a hyphen (-):
```
$ sudo yum install docker-ce-<VERSION>
```
4. Start Docker.
```
$ sudo systemctl start docker
```
5. Verify that docker is installed correctly by running the hello-world image.
```
$ sudo docker run hello-world
```
This command downloads a test image and runs it in a container. When the container runs, it prints an informational message and exits.

**Upgrade Docker CE**

To upgrade Docker CE, first run sudo yum makecache fast, then follow the installation instructions, choosing the new version you want to install.

##### Install from a package
If you cannot use Docker’s repository to install Docker, you can download the `.rpm` file for your release and install it manually. You will need to download a new file each time you want to upgrade Docker.

1. Go to [https://download.docker.com/linux/centos/7/x86_64/stable/Packages/](https://download.docker.com/linux/centos/7/x86_64/stable/Packages/) and download the `.rpm` file for the Docker version you want to install.
    >Note: To install an edge package, change the word stable in the > URL to edge. 

2. Install Docker CE, changing the path below to the path where you downloaded the Docker package.
```
$ sudo yum install /path/to/package.rpm
```
3. Start Docker.
```
$ sudo systemctl start docker
```
4. Verify that docker is installed correctly by running the hello-world image.
```
$ sudo docker run hello-world
```
##### Install using the convenience script
reference [https://docs.docker.com/engine/installation/linux/docker-ce/centos/#install-using-the-convenience-script](https://docs.docker.com/engine/installation/linux/docker-ce/centos/#install-using-the-convenience-script)
### Uninstall Docker CE
1. Uninstall the Docker package:
```
$ sudo yum remove docker-ce
```
2. Images, containers, volumes, or customized configuration files on your host are not automatically removed. To delete all images, containers, and volumes:
```
$ sudo rm -rf /var/lib/docker

```
You must delete any edited configuration files manually.