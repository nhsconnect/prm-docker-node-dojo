FROM node:23.9-alpine

# Install common Dojo scripts
ENV DOJO_VERSION=0.11.0
RUN echo "http://dl-cdn.alpinelinux.org/alpine/edge/community" >> /etc/apk/repositories && \
  apk add --no-cache tini bash shadow sudo git && \
  userdel node &&\
  git clone --depth 1 -b ${DOJO_VERSION} https://github.com/kudulab/dojo.git /tmp/dojo_git &&\
  /tmp/dojo_git/image_scripts/src/install.sh && \
  rm -r /tmp/dojo_git &&\
  echo 'dojo ALL=(ALL) NOPASSWD: ALL' >> /etc/sudoers

RUN apk add --no-cache make python3 py3-pip curl groff aws-cli

# Install sequelize postgress native dependencies
RUN apk add --no-cache postgresql-dev g++ make

# Install jq to parse json in bash
RUN wget -O ./jq-linux-arm64 https://github.com/jqlang/jq/releases/download/jq-1.7.1/jq-linux-arm64 &&\
  chmod +x ./jq-linux-arm64 &&\
  mv ./jq-linux-arm64 /usr/bin/jq

# To run postgres migrations in dojo
RUN apk add --no-cache postgresql-client

COPY profile /home/dojo/.profile
COPY bashrc /home/dojo/.bashrc
RUN chown dojo:dojo /home/dojo/.profile /home/dojo/.bashrc

ENTRYPOINT ["/sbin/tini", "-g", "--", "/usr/bin/entrypoint.sh"]
CMD ["/bin/bash"]