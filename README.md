# jenkshift

Play repo for Jenkins+OpenShift automated testing

# Setup Jenkins

    oc new-project edge-poc
    oc import-image base-centos7 --from=docker.io/openshift/jenkins-slave-base-centos7 --confirm
    oc import-image jenkins-agent-ansible-274-centos7 --from=docker.io/nfvpe/jenkins-agent-ansible-274-centos7 --confirm
    oc label is base-centos7 role=jenkins-slave
    oc label is jenkins-agent-ansible-274-centos7 role=jenkins-slave
    oc new-app jenkins-ephemeral
    oc apply -f centos-pipeline-basic.yaml

# Building new image

Create a new `Dockerfile` and use `FROM openshift/jenkins-slave-base-centos7`
and then customize it how you wish. You'll need to make sure you import it as
an image stream and tag it like we do above. If you already have Jenkins
running it will need to be restarted in order for it to get loaded into the
configuration.

    Jenkins > Manage Jenkins > Reload Configuration from Disk

Examples are located in the `slave-images` directory.
