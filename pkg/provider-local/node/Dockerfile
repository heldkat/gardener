FROM kindest/node:v1.33.2@sha256:c55080dc5be4f2cc242e6966fdf97bb62282e1cd818a28223cf536db8b0fddf4

RUN apt-get update -yq && \
    apt-get install -yq --no-install-recommends wget apparmor apparmor-utils jq openssh-server sudo logrotate

# remove kind's kubelet unit
RUN rm -f /etc/systemd/system/kubelet.service && \
    rm -rf /etc/systemd/system/kubelet.service.d/

# copy containerd hosts configurations for local registry mirrors
COPY containerd /etc/containerd/certs.d/
# names with colon are not allowed in the directory name checked into Git as it breaks go mod if gardener/gardener is used as a module
RUN mv /etc/containerd/certs.d/garden.local.gardener.cloud_5001 /etc/containerd/certs.d/garden.local.gardener.cloud:5001

# add our userdata executor unit
COPY run-userdata.sh /run-userdata.sh
COPY run-userdata.service /etc/systemd/system
RUN systemctl enable run-userdata.service

RUN runc --version ; containerd --version

RUN chmod 755 /etc/sysctl.d/

ENTRYPOINT ["/usr/local/bin/entrypoint", "/sbin/init"]
