FROM debian:stretch-slim

RUN apt-get update && apt-get install -y --no-install-recommends \
		bzip2 \
		unzip \
		xz-utils \
	&& rm -rf /var/lib/apt/lists/*

ENV LANG C.UTF-8

RUN { \
		echo '#!/bin/sh'; \
		echo 'set -e'; \
		echo; \
		echo 'dirname "$(dirname "$(readlink -f "$(which javac || which java)")")"'; \
	} > /usr/local/bin/docker-java-home \
	&& chmod +x /usr/local/bin/docker-java-home

RUN ln -svT "/usr/lib/jvm/java-8-openjdk-$(dpkg --print-architecture)" /docker-java-home
ENV JAVA_HOME /docker-java-home/jre
ENV JAVA_VERSION 8u162
ENV JAVA_DEBIAN_VERSION 8u162-b12-1~deb9u1
ENV CA_CERTIFICATES_JAVA_VERSION 20170531+nmu1

RUN set -ex; \
	\
	if [ ! -d /usr/share/man/man1 ]; then \
		mkdir -p /usr/share/man/man1; \
	fi; \
	\
	apt-get update; \
	apt-get install -y \
		openjdk-8-jre-headless="$JAVA_DEBIAN_VERSION" \
		ca-certificates-java="$CA_CERTIFICATES_JAVA_VERSION" \
	; \
	rm -rf /var/lib/apt/lists/*; \
	\
	[ "$(readlink -f "$JAVA_HOME")" = "$(docker-java-home)" ]; \
	\
	update-alternatives --get-selections | awk -v home="$(readlink -f "$JAVA_HOME")" 'index($3, home) == 1 { $2 = "manual"; print | "update-alternatives --set-selections" }'; \
	update-alternatives --query java | grep -q 'Status: manual'

RUN /var/lib/dpkg/info/ca-certificates-java.postinst configure
RUN /var/lib/dpkg/info/ca-certificates-java.postinst configure
COPY ./notes-1.0-SNAPSHOT.jar /
CMD ["java","-jar","notes-1.0-SNAPSHOT.jar"]