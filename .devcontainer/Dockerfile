ARG VARIANT=7.0-jammy
FROM mcr.microsoft.com/devcontainers/dotnet:0-${VARIANT}

# Prevent 'Warning: apt-key output should not be parsed (stdout is not a terminal)'
ENV APT_KEY_DONT_WARN_ON_DANGEROUS_USAGE=1

# Install required packages
RUN apt-get update -yq
RUN apt-get install curl gnupg libgdiplus sudo libssl-dev -yq
RUN DEBIAN_FRONTEND=noninteractive TZ=Asia/Tehran apt-get -y install tzdata

# Install Redis 5
RUN apt-get install -y redis-server=5*

# Install MongoDB 5.0.22
# Download and install libssl1.1 manually from Ubuntu 20.04 repository
RUN wget http://archive.ubuntu.com/ubuntu/pool/main/o/openssl/libssl1.1_1.1.1f-1ubuntu2_amd64.deb && \
    dpkg -i libssl1.1_1.1.1f-1ubuntu2_amd64.deb && \
    wget -qO - https://www.mongodb.org/static/pgp/server-5.0.asc | apt-key add - && \
    echo "deb [ arch=amd64,arm64 ] https://repo.mongodb.org/apt/ubuntu focal/mongodb-org/5.0 multiverse" | tee /etc/apt/sources.list.d/mongodb-org-5.0.list && \
    apt-get update && \
    apt-get install -y mongodb-org=5*

# Install MariaDB 10.11.2
RUN apt-get install -y software-properties-common && \
    apt-key adv --fetch-keys 'https://mariadb.org/mariadb_release_signing_key.asc' && \
    add-apt-repository 'deb [arch=amd64,arm64,ppc64el] https://mirror.netcologne.de/mariadb/repo/10.11/ubuntu focal main' && \
    apt update && \
    apt install -y mariadb-server-10.11*

# install NodeJS 18.x
RUN curl -sL https://deb.nodesource.com/setup_18.x | bash -
RUN apt-get install -y nodejs

# Install libgdiplus (DNTCaptcha fix)
RUN apt-get update -yq 
RUN apt-get install libgdiplus -yq
RUN cd /usr/lib
RUN ln -s libgdiplus.so gdiplus.dll

RUN useradd -m -s /bin/bash -G sudo coder
USER coder

# Let's add a fancy prompt!
RUN echo "PS1='🐳 \[\033[1;36m\]\h \[\033[1;34m\]\W\[\033[0;35m\] \[\033[1;36m\]# \[\033[0m\]'" > ~/.bashrc
