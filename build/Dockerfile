FROM docker:stable AS builder
MAINTAINER Jan Boonen <jan.boonen@geodan.nl>

ARG VERSION="latest"
ARG OS="linux-gnu"
ARG MACHINE="x86_64"

RUN apk add --no-cache curl jq && \
    if [ $VERSION = 'latest' ]; then \
      echo "Downloading latest version of Operator SDK from Github..." && \
      curl -sSf https://api.github.com/repos/operator-framework/operator-sdk/releases/latest | \
          jq -r --arg MACHINE "$MACHINE" --arg OS "$OS" '.assets[] | select(.name | test("^operator-sdk-[v0-9.]+-" + $MACHINE + "-" + $OS + "$")) | .browser_download_url' | \
          xargs curl -sSfOL; \
   else \
      echo "Downloading version $VERSION of Operator SDK from Github..." && \
      curl -sSfOL "https://github.com/operator-framework/operator-sdk/releases/download/v${VERSION}/operator-sdk-v${VERSION}-${MACHINE}-${OS}"; \
   fi && \
   apk del curl jq && \
   mv operator-sdk-* /usr/local/bin/operator-sdk && \
   chmod +x /usr/local/bin/operator-sdk

FROM golang:1.14

COPY --from=builder /usr/local/bin/operator-sdk /go/bin/operator-sdk
WORkDIR /source
ENTRYPOINT ["/go/bin/operator-sdk"]
