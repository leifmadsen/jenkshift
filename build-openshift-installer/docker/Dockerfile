# This Dockerfile is a used by CI to publish openshift/origin-v4.0:installer
# It builds an image containing only the openshift-install.

FROM fedora:29 AS builder
RUN dnf install gcc-c++ libvirt-devel openssl-devel golang -y && dnf clean all
WORKDIR /go/src/github.com/openshift/installer
COPY . .
RUN TAGS=libvirt hack/build.sh


FROM registry.svc.ci.openshift.org/openshift/origin-v4.0:base
COPY --from=builder /go/src/github.com/openshift/installer/bin/openshift-install /bin/openshift-install
RUN mkdir /output && chown 1000:1000 /output
USER 1000:1000
ENV PATH /bin
ENV HOME /output
WORKDIR /output
ENTRYPOINT ["/bin/openshift-install"]
