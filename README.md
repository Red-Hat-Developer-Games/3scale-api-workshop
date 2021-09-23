# Red Hat API Integration &amp; Management Workshop

This is a subset of labs from the Red Hat API Integration &amp; Management Workshop. This workshop will guide you on your journey to create your first API managed by Red Hat 3scale API Management. We loosely base it on a very basic POC we would normally roll out.

The workshop is intended to be delivered in person, but will provide enough guidance for self-paced consumption.

## Introduction

### Products and Projects

* [Red Hat OpenShift Container Platform](https://www.redhat.com/en/technologies/cloud-computing/openshift)
* [Red Hat 3scale API Management](https://www.redhat.com/en/technologies/jboss-middleware/3scale)
* [Red Hat Fuse](https://access.redhat.com/products/red-hat-fuse)
* [Red Hat Single Sign On](https://access.redhat.com/products/red-hat-single-sign-on)
* [Apicurio](https://www.apicur.io/)
* [Microcks](http://microcks.github.io/)
* [3scale-toolbox](https://github.com/3scale/3scale_toolbox/)
* [Jenkins](https://www.jenkins.io/)

### Pre-requisites

* Knowledge of the current version of the OpenAPI specification: [OpenAPI Specification 3.0.1](https://github.com/OAI/OpenAPI-Specification/blob/master/versions/3.0.1.md)
* Internet access with no blacklist filtering on:
  * *.open.redhat.com
  * *.onlinecurl.com

### Agenda

* Red Hat 3scale API Management with Jenkins : 20 mins
* Workshop Overview: 10 mins
* Labs: 2hrs
* 
### Slides

Check the latest [Slides](https://docs.google.com/presentation/d/1Mgsn107absS273ZryZHv3ZUfbXOZqTafSmey95qZzt8/edit?usp=sharing) for delivering this workshop.

### Lab

Jenkins CICD

* [07. 3scale-toolbox Install](docs/labs/lab201/#lab-201) - Install the 3scale toolbox locally
* [08. 3scale-toolbox Connect](docs/labs/lab202/#lab-202) - Connect 3scale toolbox locally
* [09. 3scale-toolbox OpenShift Setup](docs/labs/lab203/#lab-203) - Setup 3scale-toolbox in OpenShift
* [10. Deploy NodeJS TODO App](docs/labs/lab204/#lab-204) - Deploy NodeJS TODO App
* [11. 3scale-toolbox Pipeline Setup](docs/labs/lab205/#lab-205) - Create Jenkins pipeline build and deploy the new API.

### FAQ

Having any trouble while doing the labs? Check this [troubleshooting guide](docs/troubleshooting.md#troubleshooting) to check for answers for most common problems.

### Support & Ownership

#### Workshop with 3scale 3.9 and CICD
GitHub Repo: [https://github.com/misanche/3scale-api-workshop](https://github.com/misanche/3scale-api-workshop)

Feel free to ask [Mikel Sanchez](mailto:misanche@redhat.com) if you need some support when there are any questions left or if you need some support.
#### Original Workshop with 3scale 3.2
GitHub Repo: [https://github.com/jbossdemocentral/3scale-api-workshop](https://github.com/jbossdemocentral/3scale-api-workshop)

Feel free to ask [Hugo Guerrero](mailto:hguerrero@redhat.com) if you need some support when there are any questions left or if you need some support.

### Contributing

We welcome all forms of contribution (content, issues/bugs, feedback). Please see the [Contribution Guidelines](docs/contributing.md#guides-for-contributing) for ways to help
