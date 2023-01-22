#FROM alpine:latest
#FROM ubuntu:latest
FROM archlinux:latest

ENV DEBIAN_FRONTEND=noninteractive \
    LANG=C.UTF-8 \
    LC_ALL=C.UTF-8 \
    EDITOR=code \
    VISUAL=code \
    GIT_EDITOR="code --wait" \
    USER=root \
    HOME=/root \
    OPENVSCODE_SERVER_ROOT=/opt/openvscode-server \
    PATH=$PATH:/opt/openvscode-server/bin:/home/esprust/.cargo/bin

RUN pacman -Syu  --noconfirm
RUN pacman -S --noconfirm espflash espup cargo-generate gcc glibc curl wget perl python python-pip openssl ninja pkgconf bash glibc make cmake tar git ncdu ca-certificates jq
#RUN apk add ca-certificates tar git curl wget libc6-compat g++ cmake gcc make jq libc6-compat gcompat icu-data-full openssl-dev nano ninja python3 py3-pip eudev-dev pkgconfig ncdu perl bash python3 python3-pip
#RUN apt-get install -y build-essential ninja-build python3 python3-pip libc6 libicu-dev libssl-dev pkgconf nano tar ca-certificates git curl wget perl bash gcc g++ pkgconf libudev-dev

RUN	mkdir /vscode_tmp && \
	cd /vscode_tmp && \
	(curl -s https://api.github.com/repos/gitpod-io/openvscode-server/releases/latest | jq -r ".assets[] | select(.name | contains(\"x64\")) | .browser_download_url"  | wget -i -) && \
	tar xvfz *tar.gz && \
	rm -rf *tar.gz && \
	mv * /opt/openvscode-server
RUN	mkdir /home/esprust && \
	ln -s /home/esprust/work/.openvscode-server /home/esprust/.openvscode-server && \
	chown -R 1000:1000 /home/esprust
USER 1000:1000

WORKDIR /home/esprust
ENV HOME=/home/esprust \
    USER=esprust

##RUN curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh -s -- -y --profile minimal
#RUN curl -L https://github.com/esp-rs/espup/releases/latest/download/espup-x86_64-unknown-linux-gnu -o espup && \
#	chmod a+x espup && \
#	./espup install --esp-idf-version 5.0
##RUN cargo search --limit 0
##RUN cargo install espup

RUN espup install --esp-idf-version 5.0
RUN cargo install cargo-generate espflash cargo-espflash
RUN cargo install cargo-cache cargo-update
RUN cargo cache -e

ENV CARGO_HOME=/home/esprust/work/.cargo
ENTRYPOINT bash -c ". /home/esprust/export-esp.sh; openvscode-server --host ${OPENVSCODE_HOST} --without-connection-token"
# --connection-token ${OPENVSCODE_TOKEN}
