FROM nginx:alpine

ENV HUGO_VERSION="0.59.1"
ENV GITHUB_USERNAME="hhoover"
ENV DOCKER_IMAGE_NAME="hugo-workflow"

USER root

RUN apk add --update \
    wget \
    git \
    ca-certificates

RUN wget --quiet https://github.com/gohugoio/hugo/releases/download/v${HUGO_VERSION}/hugo_${HUGO_VERSION}_Linux-64bit.tar.gz && \
    tar -xf hugo_${HUGO_VERSION}_Linux-64bit.tar.gz && \
    mv hugo /usr/local/bin/hugo && \
    rm -rf hugo_${HUGO_VERSION}_Linux-64bit.tar.gz

RUN git clone https://github.com/${GITHUB_USERNAME}/${DOCKER_IMAGE_NAME}.git

RUN hugo -s ${DOCKER_IMAGE_NAME}/site -d /usr/share/nginx/html/

CMD nginx -g "daemon off;"