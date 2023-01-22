#FROM alpine:latest
#FROM ubuntu:latest
FROM archlinux:latest

ENV DEBIAN_FRONTEND=noninteractive \
    RUSTUP_HOME=/home/workspace/rust/rustup \
    CARGO_HOME=/home/workspace/cargo \
    PATH=/usr/local/cargo/bin:/opt/openvscode-server/bin:/usr/local/bin:/home/workspace/.cargo/bin/:/home/workspace/opt/bin:/opt/rust/bin:/opt/esprust/cargo/bin:/opt/esprust/rust/bin:$PATH \
    LANG=C.UTF-8 \
    LC_ALL=C.UTF-8 \
    EDITOR=code \
    USER=root \
    HOME=/root \
    VISUAL=code \
    GIT_EDITOR="code --wait" \
    OPENVSCODE_SERVER_ROOT=/opt/openvscode-server

RUN pacman -Syu  --noconfirm
RUN pacman -S --noconfirm espflash espup cargo-generate gcc glibc curl wget perl python python-pip openssl ninja pkgconf bash glibc make cmake tar git ncdu ca-certificates jq

#curl -L https://github.com/esp-rs/espup/releases/latest/download/espup-x86_64-unknown-linux-gnu -o espup
#chmod a+x espup
#curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh -s -- -y --profile default --default-toolchain nightly --component rust-src && \
#RUN echo "https://dl-cdn.alpinelinux.org/alpine/latest-stable/community" >> /etc/apk/repositories
#RUN apk --no-cache add \
#	ca-certificates tar git curl wget libc6-compat g++ cmake gcc make jq libc6-compat gcompat icu-data-full openssl-dev nano ninja python3 py3-pip eudev-dev pkgconfig ncdu perl bash && \
#RUN apt-get update -y
#RUN apt-get upgrade -y && apt-get dist-upgrade -y 

#RUN apt-get install -y build-essential ninja-build python3 python3-pip libc6 libicu-dev libssl-dev pkgconf nano tar ca-certificates git curl wget perl bash gcc g++ pkgconf libudev-dev
#RUN apt-get install python3 python3-pip 

RUN	mkdir /vscode_tmp && \
	cd /vscode_tmp && \
	(curl -s https://api.github.com/repos/gitpod-io/openvscode-server/releases/latest | jq -r ".assets[] | select(.name | contains(\"x64\")) | .browser_download_url"  | wget -i -) && \
	tar xvfz *tar.gz && \
	rm -rf *tar.gz && \
	mv * /opt/openvscode-server
RUN	mkdir /opt/esprust /home/workspace && \
	chown -R 1000:1000 /opt/esprust /home/workspace
USER 1000:1000

WORKDIR /opt/esprust
ENV HOME=/opt/esprust \
    USER=workspace

##RUN curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh -s -- -y --profile minimal
#RUN curl -L https://github.com/esp-rs/espup/releases/latest/download/espup-x86_64-unknown-linux-gnu -o espup && \
#	chmod a+x espup && \
#	./espup install --esp-idf-version 5.0
##RUN cargo search --limit 0
##RUN cargo install espup
#ENV RUST_BACKTRACE=1
RUN espup install --esp-idf-version 5.0
#ENTRYPOINT sh -c sh
#USER 1000:1000
#token in url = tkn
##RUN cargo install cargo-generate espflash cargo-espflash


WORKDIR /home/workspace
ENV HOME=/home/workspace \
    USER=workspace
RUN chown -R 1000:1000 /home/workspace

ENTRYPOINT bash -c ". /home/workspace/export-esp.sh; openvscode-server --host ${OPENVSCODE_HOST} --without-connection-token"
# --connection-token ${OPENVSCODE_TOKEN}
