FROM        debian:buster

RUN         apt update && apt upgrade -y \
				&& apt install -y gcc g++ libgcc1 lib32gcc1 libc++-dev gdb libc6 git wget curl tar zip unzip binutils xz-utils liblzo2-2 cabextract iproute2 net-tools netcat telnet libatomic1 libsdl1.2debian libsdl2-2.0-0 \
    			libfontconfig libicu63 icu-devtools libunwind8 libssl-dev sqlite3 libsqlite3-dev libmariadbclient-dev libduktape203 locales ffmpeg gnupg2 apt-transport-https software-properties-common ca-certificates tzdata \
    			liblua5.3 libz-dev rapidjson-dev \
				&& update-locale lang=en_US.UTF-8 \
				&& dpkg-reconfigure --frontend noninteractive locales \
				&& useradd -m -d /home/container -s /bin/bash container


ENV 		WINEARCH=win32
ENV 		XVFB=1
ENV 		AUTO_UPDATE=1
ENV 		DISPLAY_DEPTH=16
ENV 		DISPLAY_HEIGHT=768
ENV 		DISPLAY_WIDTH=1024
ENV 		DISPLAY=:0
ENV 		WINEPREFIX=/home/container/.wine
ENV 		HOME=/home/container
RUN 		wget https://packages.microsoft.com/config/debian/10/packages-microsoft-prod.deb -O packages-microsoft-prod.deb && dpkg -i packages-microsoft-prod.deb && apt-get update && apt-get install -y apt-transport-https && apt-get update && apt-get install -y aspnetcore-runtime-3.1 dotnet-sdk-3.1
RUN 		wget -q -O /usr/sbin/winetricks https://raw.githubusercontent.com/Winetricks/winetricks/master/src/winetricks && chmod +x /usr/sbin/winetricks && echo "en_US.UTF-8 UTF-8" >> /etc/locale.gen && locale-gen && useradd -m -d /home/container container
RUN 		apt install -y --no-install-recommends iproute2 cabextract lib32gcc1 libntlm0 ca-certificates winbind xvfb tzdata locales xauth python3
RUN 		apt install -y --install-recommends winehq-stable winbind xvfb
RUN 		apt-get update
RUN 		echo "deb http://download.opensuse.org/repositories/Emulators:/Wine:/Debian/Debian_10 ./" | tee /etc/apt/sources.list.d/wine-obs.list
RUN 		wget -O- -q https://download.opensuse.org/repositories/Emulators:/Wine:/Debian/Debian_10/Release.key | apt-key add -
RUN 		apt-add-repository https://dl.winehq.org/wine-builds/debian/
RUN 		wget -qO - https://dl.winehq.org/wine-builds/winehq.key | apt-key add -
RUN 		apt install -y --no-install-recommends gnupg2 wget curl software-properties-common
RUN 		dpkg --add-architecture i386 && apt update && apt upgrade -y
ENV 		DEBIAN_FRONTEND=noninteractive

USER        container
ENV         USER=container HOME=/home/container
ENV         DEBIAN_FRONTEND noninteractive

WORKDIR     /home/container

COPY        ./entrypoint.sh /entrypoint.sh
CMD         [ "/bin/bash", "/entrypoint.sh" ]
