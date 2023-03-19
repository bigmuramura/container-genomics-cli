FROM debian:stable as builder

WORKDIR /tmp

RUN apt-get update && apt-get install -y \
	curl \
	unzip \
	wget

# AWS CLI v2（ARM版） のソースコードダウンロード
RUN curl "https://awscli.amazonaws.com/awscli-exe-linux-aarch64.zip" -o "awscliv2.zip"
RUN unzip awscliv2.zip
# Amazon Genomics CLI のソースコードダウンロード
RUN wget "https://github.com/aws/amazon-genomics-cli/releases/download/v1.6.0/amazon-genomics-cli-1.6.0.zip" -O "agc.zip"
RUN unzip agc.zip
RUN rm *.zip

FROM node:19.8.1-buster-slim

COPY --from=builder /tmp/ /tmp/
RUN /tmp/aws/install
RUN /tmp/amazon-genomics-cli/install.sh
RUN apt-get update && apt-get install -y \
	git \
	jq \
	fish \
	vim \
	&& apt-get clean \
	&& rm -rf /var/lib/apt/lists/*

ENV PATH $PATH:/root/bin
RUN echo $PATH
