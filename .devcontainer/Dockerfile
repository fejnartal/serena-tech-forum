# This is a multi-stage Dockerfile:
# https://docs.docker.com/build/building/multi-stage/
#
# We are using multi-stage to clearly identify where to find the strict
# versions of the external tools needed to build the working environment.
# This way, if someone wants to reproduce the same environment natively
# (without Docker) they'll have a clear description of the required tools.

FROM debian:bullseye-slim AS strictly-versioned-deps
RUN apt-get update \
 && apt-get install -y --no-install-recommends \
      ca-certificates curl
 
RUN LATEST_GO=$(curl  -L "https://go.dev/VERSION?m=text")                     \
 && curl -o go.tar.gz -L "https://go.dev/dl/${LATEST_GO}.linux-amd64.tar.gz"  \
 && tar xvf go.tar.gz -C /usr/local/



# This is the description of the final working environment.
# We also install several general-purpose tools. We don't need any strict version
# for these tools so it is ok to install them directly with the distro packmanager.
FROM debian:bullseye-slim
COPY --from=strictly-versioned-deps "/usr/local/go/" "/usr/local/go"
ENV PATH=$PATH:/usr/local/go/bin/

RUN apt-get update \
 && apt-get install -y --no-install-recommends \
      ca-certificates \
      git \
      gpg \
      ssh
 