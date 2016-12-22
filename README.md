# Digital Ocean Ansible Artifactory
Provisions a Digital Ocean Droplet running Artifactory

The image on Droplet boot starts an Artifactory server running on port 8081.

This project assumes the base image was created by the [digital-ocean-ansible-base](https://github.com/mjtieman/digital-ocean-ansible-base) project.

I am receptive to suggestions and happy to accept pull requests. However, for pull requests there **must be an accompanying issue and commit messages must begin with: ```Issue-{issue number}```**.

## Prerequisites
* Packer, install instructions [here](https://www.packer.io/intro/getting-started/setup.html).
* A Digital Ocean API token for the account the image will be created in.
* A base image created from the [digital-ocean-ansible-base](https://github.com/mjtieman/digital-ocean-ansible-base) project.

Note: There is an outstanding [issue](https://github.com/mitchellh/packer/issues/3252) with Packer that blocks referencing private images in the builder by name. To get around this, private images need to be referenced by id. An example curl command to get a list of private images in the account, which includes the id.

```
curl -X GET -H "Content-Type: application/json" -H "Authorization: Bearer YOUR_API_TOKEN_HERE" "https://api.digitalocean.com/v2/images?page=1&per_page=10&private=true"
```

## Creating the Image
The image is created by running a single packer command. There are a few variables that need to be passed to Packer as part of the command.

|Variable Name|Required|Description|
|:------------|:------:|:----------|
|version|Yes|An arbitrary version number added to the image name.|
|api-token|Yes|The API token used by the Packer build to interact with Digital Ocean.|
|base-image|Yes|Base image to build from. This needs to be the image id, not name.|
|base-snapshot-name|Yes|The base name of the created snapshot.|
|region|No|Region to deploy the image to. Defaults to nyc1.|

### Example Command
From the packer directory execute the ```packer build``` command with the appropriate variables.
```
packer build -machine-readable -var "version=0" -var "api-token=YOUR_API_TOKEN_HERE" -var "base-image=00000000" -var "base-snapshot-name=my-artifactory" artifactory.json
```
