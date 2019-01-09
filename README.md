# jenkshift

Lab setup for OpenShift+Jenkins on oVirt in order to execute CI pipelines using
the OpenShift pipeline system.

# Infrastructure Setup

Base installation is done on top of oVirt hyperconverged.

Installation inventory files for the lab setups are available at
https://github.com/redhat-nfvpe/telemetry-framework under the `rhv/` folder.

## Steps

1. Get oVirt hyperconverged installed on a node
2. Make sure your DNS is functional (dnsmasq in my case via `/etc/hosts.dnsmasq`)
3. Import your CentOS template with the `telemetry-framework` setup for
   `centos76_template`
4. Run the `vm-infra.yml` playbook from `telemetry-framework` to setup the
   virtual machines. This will also install OpenShift on the nodes.
5. Now we're ready to setup applications. Next!

## Verify

Use your `oc` application to login to your master node. This is something like
`oc login https://master.nfvpe-18.nfvpe.site:8443 -uadmin`. You can also visit
the console page at https://console.nfvpe-18.nfvpe.site:8443.

# Setting up Jenkins and Jobs

## Create _edgepoc_ Project

    oc new-project edgepoc

## Import ImageStream for Jenkins Worker

    oc import-image openshift-install-builder --from=docker.io/nfvpe/openshift-install-builder --confirm
    oc label is openshift-install-builder role=jenkins-slave

## Setup Jenkins

    oc new-app --env VOLUME_CAPACITY=4Gi jenkins-persistent

This part will take a while. You'll want to monitor progress via the GUI
probably.

## Setup Job Data

For your jobs as specified by the `Jenkinsfile` in the root of this
directory, you may need to add some environment variables to Jenkins to pass
via `credentials` or via the _Config Files_ module.

## Import Job

Import your job with something like:

    oc apply -f build-openshift-installer.yml

# Day Two Operations

# Building new image

Create a new `Dockerfile` and use `FROM openshift/jenkins-slave-base-centos7`
and then customize it how you wish. You'll need to make sure you import it as
an image stream and tag it like we do above. If you already have Jenkins
running it will need to be restarted in order for it to get loaded into the
configuration.

    Jenkins > Manage Jenkins > Reload Configuration from Disk

Examples are located in the `slave-images` directory.

Follow the instructions for _Import ImageStream for Jenkins Worker_ before
reloading Jenkins in order to pull in an image and label it properly.
