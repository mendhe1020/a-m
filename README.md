#FROM registry.dev.sbiepay.sbi:8443/ubi9/openjdk-21-runtime:1.23-6.1758133907
FROM registry.dev.sbiepay.sbi:8443/ubi9/openjdk-21:1.24
ARG ENV
ARG VERSION

ENV ENV=$ENV
ENV VERSION=$VERSION

COPY /build/libs/epay_communication_service-${VERSION}.jar .
ENTRYPOINT sh -c 'java -jar "epay_communication_service-$VERSION.jar"'
