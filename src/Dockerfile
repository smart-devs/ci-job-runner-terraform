# hadolint ignore=DL3007
FROM smartdevs/debian-stretch:latest

ARG DEBIAN_FRONTEND=noninteractive

WORKDIR /tmp/

ARG TERRAFORM_RUNTIME_REQUIREMENTS="openssh-client"
ARG TERRAFORM_BUILD_REQUIREMENTS="curl"
ARG TERRAFORM_VERSION=0.11.10
ARG TERRAFORM_SHA256SUM=43543a0e56e31b0952ea3623521917e060f2718ab06fe2b2d506cfaa14d54527
ARG KUBERNETES_CLI_VERSION="1.10.3/2018-07-26"
ARG AWS_IAM_AUTHENTICATOR_VERSION="1.10.3/2018-07-26"

# hadolint ignore=DL4006,DL3008,SC2086
RUN set -Eeu; \
    apt-get update -y; \
    apt-get install --no-install-recommends -y ${TERRAFORM_RUNTIME_REQUIREMENTS} ${TERRAFORM_EXTRA_RUNTIME_REQUIREMENTS:-} ${TERRAFORM_BUILD_REQUIREMENTS} ${TERRAFORM_EXTRA_BUILD_REQUIREMENTS:-}; \
    apt-mark auto ${TERRAFORM_BUILD_REQUIREMENTS} ${TERRAFORM_EXTRA_BUILD_REQUIREMENTS:-}; \
    curl -s -L -o  "terraform_${TERRAFORM_VERSION}_linux_amd64.zip" "https://releases.hashicorp.com/terraform/${TERRAFORM_VERSION}/terraform_${TERRAFORM_VERSION}_linux_amd64.zip"; \
    echo  "${TERRAFORM_SHA256SUM} terraform_${TERRAFORM_VERSION}_linux_amd64.zip" > terraform_SHA256SUMS; \
    sha256sum -c terraform_SHA256SUMS; \
    unzip "terraform_${TERRAFORM_VERSION}_linux_amd64.zip" -d /usr/local/bin; \
	curl -o /usr/local/bin/kubectl -s https://amazon-eks.s3-us-west-2.amazonaws.com/${KUBERNETES_CLI_VERSION}/bin/linux/amd64/kubectl; \
    chmod +x /usr/local/bin/kubectl; \
	curl -o  /usr/local/bin/aws-iam-authenticator -s https://amazon-eks.s3-us-west-2.amazonaws.com/${AWS_IAM_AUTHENTICATOR_VERSION}/bin/linux/amd64/aws-iam-authenticator; \
    chmod +x /usr/local/bin/aws-iam-authenticator; \
    apt-get purge --auto-remove -y; \
    dpkg -la | awk '{print $2}' | grep '\-dev' || true | xargs --no-run-if-empty apt-get remove -y; \
    apt-get clean; \
    apt-get --yes --quiet autoremove --purge; \
    find /var/log -type f -print0 | xargs -0 truncate -s0; \
    find /tmp/ -mindepth 1 -exec rm -rf {} +; \
    find /var/tmp/ -mindepth 1 -exec rm -rf {} +; \
    rm -rf \
        /var/lib/apt/lists/* \
        /usr/share/doc/* \
        /usr/share/groff/* \
        /usr/share/info/* \
        /usr/share/linda/* \
        /usr/share/lintian/* \
        /usr/share/man/*/* \
        ;
