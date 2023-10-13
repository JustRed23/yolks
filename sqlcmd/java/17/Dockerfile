FROM        --platform=$TARGETOS/$TARGETARCH eclipse-temurin:17-jdk-jammy

LABEL       author="JustRed23" maintainer="parker@pterodactyl.io"

LABEL       org.opencontainers.image.source="https://github.com/parkervcp/yolks"
LABEL       org.opencontainers.image.licenses=MIT

RUN         apt update -y \
			&& apt install -y \
					curl \
					lsof \
					ca-certificates \
					openssl \
					git \
					tar \
					sqlite3 \
					fontconfig \
					tzdata \
					iproute2 \
					libfreetype6 \
					apt-transport-https \
					debconf-utils \
					gnupg2 

# adding custom MS repository
RUN 		curl https://packages.microsoft.com/keys/microsoft.asc | apt-key add -
RUN 		curl https://packages.microsoft.com/config/ubuntu/20.04/prod.list > /etc/apt/sources.list.d/mssql-release.list

# install SQL Server drivers and tools
RUN 		apt-get update && ACCEPT_EULA=Y apt-get install -y msodbcsql18 mssql-tools18
RUN 		echo 'export PATH="$PATH:/opt/mssql-tools18/bin"' >> ~/.bashrc
RUN 		/bin/bash -c "source ~/.bashrc"

RUN 		apt-get -y install locales \
			&& rm -rf /var/lib/apt/lists/*
			
RUN 		locale-gen en_US.UTF-8
RUN 		update-locale LANG=en_US.UTF-8

## Setup user and working directory
RUN         useradd -m -d /home/container -s /bin/bash container
USER        container
ENV         USER=container HOME=/home/container
WORKDIR     /home/container

COPY        ./../entrypoint.sh /entrypoint.sh
CMD         [ "/bin/bash", "/entrypoint.sh" ]
