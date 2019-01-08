FROM openshift/jenkins-slave-base-centos7

RUN yum -y update && yum clean all && \
        mkdir -p /go && chmod -R 777 /go && \
        yum install -y centos-release-scl epel-release && \
        yum install -y go-toolset-7-golang git gcc-c++ libvirt-devel openssl-devel wget tar ansible && yum clean all

ENV GOPATH=/go \
    BASH_ENV=/opt/rh/go-toolset-7/enable \
    ENV=/opt/rh/go-toolset-7/enable \
    PROMT_COMMAND=". /opt/rh/go-toolset-7/enable"

WORKDIR /go
