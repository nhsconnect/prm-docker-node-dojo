FROM node:14.17.1-alpine

# Install common Dojo scripts
ENV DOJO_VERSION=0.10.2
RUN echo "http://dl-cdn.alpinelinux.org/alpine/edge/community" >> /etc/apk/repositories && \
  apk add --no-cache tini bash shadow sudo git && \
  userdel node &&\
  git clone --depth 1 -b ${DOJO_VERSION} https://github.com/kudulab/dojo.git /tmp/dojo_git &&\
  /tmp/dojo_git/image_scripts/src/install.sh && \
  rm -r /tmp/dojo_git &&\
  echo 'dojo ALL=(ALL) NOPASSWD: ALL' >> /etc/sudoers

ENV AWS_CLI_VERSION=1.16.238 BOTO3_VERSION=1.9.228
RUN apk add --no-cache make python py-pip curl groff &&\
  pip install awscli==${AWS_CLI_VERSION} boto3==${BOTO3_VERSION}

# Install sequelize postgress native dependencies
RUN apk add --no-cache postgresql-dev g++ make

# Install jq to parse json in bash
RUN wget -O ./jq-linux64 https://github.com/stedolan/jq/releases/download/jq-1.5/jq-linux64 &&\
  chmod +x ./jq-linux64 &&\
  mv ./jq-linux64 /usr/bin/jq

# To run postgres migrations in dojo
RUN apk add --no-cache postgresql-client

# Needed by gp2gp-adaptor dependencies
RUN apk add --no-cache make python3

# Needed for debugging
RUN apk add --no-cache curl

COPY profile /home/dojo/.profile
COPY bashrc /home/dojo/.bashrc
RUN chown dojo:dojo /home/dojo/.profile /home/dojo/.bashrc

ENTRYPOINT ["/sbin/tini", "-g", "--", "/usr/bin/entrypoint.sh"]
CMD ["/bin/bash"]
