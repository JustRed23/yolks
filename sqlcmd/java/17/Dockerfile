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
					libfreetype6
					
RUN         curl https://packages.microsoft.com/keys/microsoft.asc | tee /etc/apt/trusted.gpg.d/microsoft.asc
RUN			curl https://packages.microsoft.com/config/ubuntu/20.04/prod.list > /etc/apt/sources.list.d/mssql-release.list
RUN			echo msodbcsql msodbcsql/ACCEPT_EULA select true | debconf-set-selections
RUN			echo mssql-tools mssql-tools/accept_eula select Y | debconf-set-selections

RUN         apt update -y \
			&& apt install -y \
					mssql-tools18 \
					unixodbc-dev

## Setup user and working directory
RUN         useradd -m -d /home/container -s /bin/bash container
USER        container
ENV         USER=container HOME=/home/container
WORKDIR     /home/container

COPY        ./../entrypoint.sh /entrypoint.sh
CMD         [ "/bin/bash", "/entrypoint.sh" ]
