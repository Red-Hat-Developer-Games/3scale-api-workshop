# Lab 201

## Jenkins CICD

### Install the 3scale toolbox locally

* Duration: 15 mins
* Audience: Developers, Architects, Devops

## Overview

This section covers how to install the 3scale toolbox cli. This section will allow us to retrieve the `.3scalerc.yaml` needed for later use in our Pipelines.

### What is 3scale toolbox?

The 3scale toolbox is a Ruby client that lets you manage 3scale services from the command line.

### Environment

**URLs:**

Check with your instruction the *GUID* number of your current workshop environment. Replace the actual number on all the URLs where you find **GUID**.

Example in case of *GUID* = **1234**:

```bash
https://master.GUID.open.redhat.com
```

becomes =>

```bash
https://master.1234.open.redhat.com
```

**Credentials:**

Your username is your asigned user number. For example, if you are assigned user number **1**, your username is: 

```bash
user1
```

The password to login is always the same:

```bash
openshift
```

## Lab Instructions

### Step 1: Installing the 3scale-toolbox

#### RELEASES

Download packages from [latest release](https://github.com/3scale/3scale_toolbox_packaging/releases/latest) section.

#### PLATFORMS

* [CentOS/Fedora](#centos-fedora)
* [Ubuntu/Debian](#ubuntu-debian)
* [Mac OS X](#mac-os-x)
* [Windows](#windows)
* [Docker](#docker)
* [Kubernetes](#kubernetes)

##### Centos Fedora

1. Install the `RPM` package using the `rpm` installer utility.

```bash
rpm -Uvh `URL_RPM_PACKAGE`
```

Files will be installed in `/opt/3scale-toolbox`

2. Test executing `help` command.

```bash
3scale help
```

##### Ubuntu Debian

1. Download `DEB` package.

```bash
wget `URL_DEB_PACKAGE`
```

2. Install the package using the `dpkg` installer utility.

```bash
dpkg -i 3scale-toolbox_VERSION.deb
```

Files will be installed in `/opt/3scale-toolbox`

3. Test executing `help` command.

```bash
3scale help
```

##### Mac OS X

1. Download `pkg` package.

```bash
wget `URL_PKG_PACKAGE`
```

2. Install the package using the `installer` installer utility.

```bash
installer -pkg 3scaletoolbox-VERSION.pkg -target /
```

Files will be installed in `/opt/3scale-toolbox`

3. Create symbolic link on `PATH`.

```bash
ln -s /opt/3scale-toolbox/bin/3scale /usr/local/bin/3scale
```

4. Test executing `help` command

```bash
3scale help
```

##### Windows
Requires 64 bit Windows 8.1 or greater or 64 bit Windows Server 2012r2 or greater.

1. Download `MSI` package.

Once installed, you'll have access to the `3scale` command from your command shell

##### Docker

Docker repository

```
quay.io/redhat/3scale-toolbox
```

1. Test executing `help` command

```bash
docker run --rm quay.io/redhat/3scale-toolbox 3scale help
```

##### Kubernetes

Also working with Openshift.

1. Provision the *3scale-toolbox* secret with your 3scale credentials

```bash
3scale remote add 3scale-instance "https://123...456@MY-TENANT-admin.3scale.net/"
kubectl create secret generic 3scale-toolbox --from-file="$HOME/.3scalerc.yaml"
```

2. Deploy Job template to run your 3scale command.
In the example shown below, `3scale remote list` command will be run.

Write the following yaml template content in a file called `toolbox-job.yaml`.

```yaml
apiVersion: batch/v1
kind: Job
metadata:
  name: toolbox
spec:
  backoffLimit: 4
  activeDeadlineSeconds: 300
  template:
    spec:
      restartPolicy: Never
      containers:
        - name: toolbox-job
          image: quay.io/redhat/3scale-toolbox
          env:
            - name: HOME
              value: /var/lib/toolbox
          args:
            - 3scale
            - remote
            - list
          volumeMounts:
            - name: toolbox-config
              mountPath: /var/lib/toolbox
      volumes:
        - name: toolbox-config
          secret:
            secretName: 3scale-toolbox
```

Deploy the job

```bash
kubectl create -f toolbox-job.yaml
```

Check on the status of the Job

```bash
$ kubectl describe jobs/toolbox

Name:                     toolbox
Namespace:                test
Selector:                 controller-uid=057ec3ff-fa45-11e9-b43e-065562ed0850
Labels:                   controller-uid=057ec3ff-fa45-11e9-b43e-065562ed0850
                          job-name=toolbox
Annotations:              <none>
Parallelism:              1
Completions:              1
Start Time:               Tue, 29 Oct 2019 13:09:56 +0100
Completed At:             Tue, 29 Oct 2019 13:10:13 +0100
Duration:                 17s
Active Deadline Seconds:  300s
Pods Statuses:            0 Running / 1 Succeeded / 0 Failed
Pod Template:
  Labels:  controller-uid=057ec3ff-fa45-11e9-b43e-065562ed0850
           job-name=toolbox
  Containers:
   toolbox-job:
    Image:      quay.io/redhat/3scale-toolbox
    Port:       <none>
    Host Port:  <none>
    Args:
      3scale
      remote
      list
    Environment:
      HOME:  /var/lib/toolbox
    Mounts:
      /var/lib/toolbox from toolbox-config (rw)
  Volumes:
   toolbox-config:
    Type:        Secret (a volume populated by a Secret)
    SecretName:  3scale-toolbox
    Optional:    false
Events:
  Type    Reason            Age   From            Message
  ----    ------            ----  ----            -------
  Normal  SuccessfulCreate  24s   job-controller  Created pod: toolbox-94xsg
```

#### Troubleshooting

* There are known issues running `3scale toolbox` when `RVM` is installed and active.
You can take the following actions to resolve them:
  * Disable RVM for the current session, then run the toolbox
    ```bash
    rvm use system
    3scale help
    ```
  * Disable RVM just for running one command
    ```bash
    env -u GEM_PATH 3scale help
    ```

## Steps Beyond

So, you want more? Did you notice the link **source** when editing the *Paths* or the *Definitions*? Get back to the API editor and follow the link. What do you see? Apicurio lets you follow the form-based editor or go one step beyond and also lets you direct edit the source of your API definition.

## Summary

In this lab we have learn how to install 3scale-toolbox locally in our local machines, this will allow us to manage the 3Scale API Gateway from our local machine and retrieve the credentials file needed to use it in the pipelines.

You can now proceed to [Lab 2](../lab202/#lab-202)

## Notes and Further Reading

* 3scale-toolbox-packaging
  * [Webpage](https://github.com/3scale-labs/3scale_toolbox_packaging)