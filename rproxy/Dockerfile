FROM nginx:1.13.2

MAINTAINER harish <harish.babu@creditsafe.com>

RUN apt-get update && apt-get install -y curl unzip

# we use consul-template to re-write our Nginx virtualhost config
RUN curl -Lo /tmp/consul_template_0.19.0_linux_amd64.zip https://releases.hashicorp.com/consul-template/0.19.0/consul-template_0.19.0_linux_amd64.zip && \
    unzip /tmp/consul_template_0.19.0_linux_amd64.zip && \
    mv consul-template /bin

# get ContainerPilot release
ENV CONTAINERPILOT_VERSION 3.1.3
RUN export CP_SHA1=0526af66371e1e54c840bcfbf42c4ded4124e026 \
    && curl -Lso /tmp/containerpilot.tar.gz \
         "https://github.com/joyent/containerpilot/releases/download/${CONTAINERPILOT_VERSION}/containerpilot-${CONTAINERPILOT_VERSION}.tar.gz" \
    && echo "${CP_SHA1}  /tmp/containerpilot.tar.gz" | sha1sum -c \
    && tar zxf /tmp/containerpilot.tar.gz -C /bin \
    && rm /tmp/containerpilot.tar.gz

# COPY ContainerPilot configuration
COPY containerpilot.json5 /etc/containerpilot.json5
ENV CONTAINERPILOT=/etc/containerpilot.json5

COPY nginx.conf /etc/nginx/nginx.conf

# add Nginx virtualhost template that we'll overwrite
COPY nginx.conf.ctmpl /etc/containerpilot/nginx.conf.ctmpl

EXPOSE 80 443

CMD ["/bin/containerpilot"]
