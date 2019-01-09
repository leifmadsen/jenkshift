FROM openshift/jenkins-slave-base-centos7

RUN yum -y update && yum clean all && \
        mkdir -p /workspace && chmod -R 777 /workspace && \
        yum install -y centos-release-scl epel-release https://resources.ovirt.org/pub/yum-repo/ovirt-release42.rpm && \
        yum install -y git wget tar ansible python-ovirt-engine-sdk4 && yum clean all

WORKDIR /workspace
