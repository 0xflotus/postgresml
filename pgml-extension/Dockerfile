FROM ubuntu:jammy
MAINTAINER team@postgresml.com
RUN apt-get update
ARG DEBIAN_FRONTEND=noninteractive
ENV TZ=Etc/UTC
RUN apt-get install -y software-properties-common
RUN add-apt-repository ppa:apt-fast/stable --yes
RUN apt update && apt-get install -y apt-fast
RUN apt-get update && apt-fast install -y \
	libopenblas-dev \
	libssl-dev \
	bison \
	flex \
	pkg-config \
	cmake \
	libreadline-dev \
	libz-dev \
	curl \
	lsb-release \
	tzdata \
	sudo \
	cmake \
	libpq-dev \
	libclang-dev \
	wget \
	postgresql-plpython3-14 \
	postgresql-14 \
	postgresql-server-dev-14
RUN add-apt-repository ppa:deadsnakes/ppa --yes
RUN apt update && apt-fast install -y \
	python3.10 \
	python3-pip \
	libpython3.10-dev \
	python3.10-dev
RUN pip3 install xgboost scikit-learn torch lightgbm transformers datasets
RUN useradd postgresml -m -s /bin/bash -G sudo
RUN echo 'postgresml ALL=(ALL) NOPASSWD: ALL' >> /etc/sudoers
USER postgresml
RUN curl https://sh.rustup.rs -sSf | sh -s -- -y
RUN $HOME/.cargo/bin/cargo install cargo-pgx --version "0.7.1"
RUN $HOME/.cargo/bin/cargo pgx init
RUN curl https://www.postgresql.org/media/keys/ACCC4CF8.asc | gpg --dearmor | sudo tee /etc/apt/trusted.gpg.d/apt.postgresql.org.gpg >/dev/null
RUN sudo sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt $(lsb_release -cs)-pgdg main" > /etc/apt/sources.list.d/pgdg.list'
RUN sudo apt update
RUN sudo apt-get install -y postgresql-15 postgresql-13 postgresql-12 postgresql-11
RUN sudo apt install -y postgresql-server-dev-15 postgresql-server-dev-15 postgresql-server-dev-12 postgresql-server-dev-11
WORKDIR /app
