# Copyright 2021 Google LLC
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#     https://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.
# Download and verify the integrity of the download first

FROM sethvargo/hashicorp-installer:0.2.0 AS installer
# Required to download and install Terraform
ARG TERRAFORM_VERSION
ENV TERRAFORM_VERSION ${TERRAFORM_VERSION}
RUN /install-hashicorp-tool "terraform" "$TERRAFORM_VERSION"

FROM alpine:3.15.6
RUN apk update && apk add --no-cache \
        bash \
        git \
        wget \
        python3 \
        jq \
        go

# Install Terraform
COPY --from=installer /software/terraform /usr/local/bin/terraform

# Install cloud SDK
ENV PATH /usr/local/google-cloud-sdk/bin:$PATH
ARG CLOUD_SDK_VERSION
ENV CLOUD_SDK_VERSION ${CLOUD_SDK_VERSION}
ADD developer-tools/build/install_cloud_sdk.sh /build/
RUN /build/install_cloud_sdk.sh ${CLOUD_SDK_VERSION}
RUN rm -rf /build

RUN terraform version && gcloud version

CMD ["/bin/bash"]
