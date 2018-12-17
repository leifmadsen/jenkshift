# jenkshift
Play repo for Jenkins+OpenShift automated testing

# Setup Jenkins

    oc new-project jtest
    oc import-image base-centos7 --from=docker.io/openshift/jenkins-slave-base-centos7 --confirm
    oc label is base-centos7 role=jenkins-slave
    oc new-app jenkins-ephemeral
    oc apply -f centos-pipeline-basic.yaml
