# build

This is a guide to build the custom enviroment of supported OPTEE-OS and virtualization on imx8mqevk.

# Requirement

- Google repo
- Yocto prerequirement

# Build

`repo init -u URL -b BRANCH -m FILE (--reference EXIST-REPO)`
> Option `--reference` is for accelerating build sequence based on exist repository. Will copy the common repositories from local instead of fetching them from server.

# Example

Let's take a existing project as example.

```
mkdir imx8-yocto-bsp && cd imx8-yocto-bsp
# Fetch the manifest script
repo init -u https://github.com/T-REE/imx-manifest -b dev/imx-linux-thud -m imx-4.19.35-1.0.0_optee_virt.xml`

#Fetch the projcet according to `imx-4.19.35-1.0.0_optee_virt.xml`
repo sync -j `nproc`

# Setup Yocto enviroment
MACHINE=imx8mqevk DISTRO=fsl-imx-wayland source fsl-setup-release.sh -b out

# Build image for imx8mqevk
bitbake fsl-image-validation-imx
```

# How to customize it

First of all, please take a glance of manifest, `imx-4.19.35-1.0.0_optee_virt.xml`.

```
<?xml version="1.0" encoding="UTF-8"?>
<manifest>

  <default sync-j="2"/>
  ...
  <remote fetch="https://github.com/T-REE" name="T-REE"/>
  ...
  <project remote="T-REE" revision="dev/optee" name="meta-fsl-bsp-release" path="sources/meta-fsl-bsp-release" >
     <linkfile src="imx/tools/fsl-setup-release.sh" dest="fsl-setup-release.sh"/>
     <linkfile src="imx/README" dest="README-IMXBSP"/>
  </project>

</manifest>
```

Choose the remote repository in server (GitHub, Bitbucket ...).

`<remote fetch="URL" name="REMOTE-NAME">.`
* `URL`: The link to remote server. Here is for organizer on GitHub.
* `REMOTE-NAME`: give a name to refer.

Choose the project in remote server, checkout to specified revision.

`<project remote="REMOTE-NAME" revision="REV" name="PROJECT-NAME" path="PATH">`

* `REMOTE-NAME`: Link to the name assigned in file.
* `REV`: It could be branch, tag or hash value.
* `PROJECT-NAME`: The projcet name in remote URL
* `PATH`: the path specify the project fetched remotely to place in directory

You could copy or modify the manifest file to link to your customize project with specify branch.