################################################################################
# Copyright 2016 IBM Corporation
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#    http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.
################################################################################

FROM ppc64le/ubuntu:16.04

MAINTAINER Ray Kivisto, XL Compiler Build, Package, Install team - http://ibm.biz/xlcpp-linux-ce

ENV XLC_REPO_URL http://public.dhe.ibm.com/software/server/POWER/Linux/xl-compiler/eval/ppc64le
ENV XLC_REPO_KEY_SHA256SUM e0eba411ed1cbf48fdab9e03dfc159a280bd728e716dd408ef321e42ac3ae552
ENV XLC_VRM 13.1.4

################################################################################
# The entrypoint is a script that will interactively display the license text and then configure the compiler.
# To automatically accept the license, add "-e LICENSE=accept" to your run command
################################################################################
RUN echo "#!/bin/bash\n\
if ! grep '^Status=9$' /opt/ibm/xlC/$XLC_VRM/lap/license/status.dat >/dev/null 2>&1; then\n\
  if [ \"\$LICENSE\" = 'accept' ]; then\n\
    /opt/ibm/xlC/$XLC_VRM/bin/xlc_configure <<< 1 >/dev/null\n\
  elif [ -t 0 ]; then\n\
    /opt/ibm/xlC/$XLC_VRM/bin/xlc_configure\n\
  else\n\
    echo \"To run in non-interactive mode, accept the license with '-e LICENSE=accept'\"\n\
    exit 1\n\
  fi\n\
fi\n\
exec \"\$@\"" >/entrypoint.sh && \
  chmod 755 /entrypoint.sh

################################################################################
# verify apt-key sha256sum, add repo, install XL C/C++ for Linux, Community Edition
################################################################################
RUN apt-get update && apt-get install -y wget && \
  wget -q "$XLC_REPO_URL/ubuntu/public.gpg" && \
  echo "$XLC_REPO_KEY_SHA256SUM public.gpg"| sha256sum -c - && \
  apt-key add public.gpg && \
  rm public.gpg && \
  echo "deb $XLC_REPO_URL/ubuntu/ trusty main" > /etc/apt/sources.list.d/ibm-xl-compiler-eval.list && \
  apt-get update && apt-get install -y \
    "xlc.$XLC_VRM" \
    "xlc-license-community.$XLC_VRM"

################################################################################
# install development tools
################################################################################
RUN apt-get update && apt-get install -y \
    vim \
    make \
    autoconf \
    automake \
    cmake; \
  rm -rf /var/lib/apt/lists/* && \
  apt-get clean

ENTRYPOINT ["/entrypoint.sh"]
CMD ["bash"]
