FROM registry.access.redhat.com/ubi7/ubi:latest
MAINTAINER Sematext Group Inc. <redhat@sematext.com>
### Atomic/OpenShift Labels - https://github.com/projectatomic/ContainerApplicationGenericLabels
LABEL name="sematext/agent" \
      vendor="Sematext Group Inc." \
      version="1.5.1" \
      release="1.5.1" \
      summary="Sematext Agent" \
      description="Sematext Agent is a modern, container-native metric collection agent for containers. It runs as a tiny container on every  host and provides automatic collection and processing of host and container metrics for all cluster nodes and all auto-discovered containers. It works with Open Shift/Kubernetes or any other platform supporting containers." \
### Required labels above - recommended below
      url="https://sematext.com/docker" \
      run='docker run --privileged --name ${NAME} -e INFRA_TOKEN=${INFRA_TOKEN} ${IMAGE}' \
      io.k8s.description="Sematext Agent collects metrics and events" \
      io.k8s.display-name="Sematext Agent" \
      io.openshift.tags="sematext,monitoring"


COPY ./LICENSE /licenses/LICENSE
COPY ./help.1 /help.1
COPY ./st-agent /opt/sematext/bin/st-agent 
RUN chmod +x /opt/sematext/bin/st-agent

# Sematext Agent ENV 
ENV PROCFS_ROOT=/host/proc
ENV ETC_ROOT=/host/etc
ENV SYS_ROOT=/host/sys
ENV LIB_ROOT=/host/usr/lib

RUN \
  yum install -y ca-certificates 

ENV APP_ROOT=/opt/sematext 
#ENV USER_NAME=default \
#    USER_UID=10001 

ENV APP_HOME=${APP_ROOT}/src  PATH=$PATH:${APP_ROOT}/bin


### Setup user for build execution and application runtime
#RUN chmod -R ug+x ${APP_ROOT}/bin && sync && \
#    useradd -l -u ${USER_UID} -r -g 0 -d ${APP_ROOT} -s /sbin/nologin -c "${USER_NAME} user" ${USER_NAME} && \
#    chown -R ${USER_UID}:0 ${APP_ROOT} && \
#    chmod -R g=u ${APP_ROOT} && \
#    chown -R ${USER_UID}:0 ${APP_ROOT}

### Containers should NOT run as root as a good practice
#USER 10001

ENTRYPOINT ["/opt/sematext/bin/st-agent", "--"]
CMD ["/opt/sematext/bin/st-agent"]
