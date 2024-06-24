FROM registry.access.redhat.com/ubi9/go-toolset:1.21.10-1@sha256:bee74f192b88c76f4b1a41f9892cff62b346ce6224fdbf167e225918f8e9d216 AS builder

WORKDIR /app
COPY . .
RUN go build -buildvcs=false -mod=vendor -o /app/opc main.go

FROM registry.access.redhat.com/ubi9/ubi-minimal:latest@sha256:2636170dc55a0931d013014a72ae26c0c2521d4b61a28354b3e2e5369fa335a3
LABEL name="opc" \
      com.redhat.component="opc" \
      io.k8s.display-name="opc" \
      summary="A CLI for OpenShift Pipeline" \
      description="opc makes it easy to work with Tekton resources in OpenShift Pipelines. It is built on top of tkn and tkn-pac and expands their capablities to the functionality and user-experience that is available on OpenShift." \
      io.k8s.description="opc makes it easy to work with Tekton resources in OpenShift Pipelines. It is built on top of tkn and tkn-pac and expands their capablities to the functionality and user-experience that is available on OpenShift." \
      io.openshift.tags="pipelines,tekton"
COPY --from=builder /app/opc /usr/bin
RUN mkdir /licenses
COPY LICENSE /licenses/.
RUN microdnf install -y shadow-utils
RUN groupadd -r -g 65532 nonroot && useradd --no-log-init -r -u 65532 -g nonroot nonroot
USER 65532
