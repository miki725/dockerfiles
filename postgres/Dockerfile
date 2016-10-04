FROM ubuntu:16.04
MAINTAINER "Miroslav Shubernetskiy"

ENV POSTGRES_VERSION 9.6
ENV GOSU_VERSION 1.10
ENV PGDATA /var/lib/postgresql/data
ENV PATH $PATH:/usr/lib/postgresql/$POSTGRES_VERSION/bin

RUN groupadd -r postgres && useradd -r -g postgres postgres

# Add the PostgreSQL PGP key to verify their Debian packages
# It should be the same key as https://www.postgresql.org/media/keys/ACCC4CF8.asc
RUN apt-key adv \
        --keyserver keyserver.ubuntu.com \
        --recv-keys B97B0AFCAA1A47F044F244A07FCC7D46ACCC4CF8 && \
    echo "deb http://apt.postgresql.org/pub/repos/apt/ trusty-pgdg main $POSTGRES_VERSION" \
        > /etc/apt/sources.list.d/pgdg.list && \
    apt-get update

RUN apt-get upgrade -y

RUN apt-get install -y \
    curl \
    libpq5 \
    postgresql-$POSTGRES_VERSION \
    postgresql-client-$POSTGRES_VERSION \
    postgresql-contrib-$POSTGRES_VERSION

# cleanup
RUN apt-get clean && \
    rm -rf /var/cache/apt

# install gosu
RUN curl -o /usr/local/bin/gosu -sSL "https://github.com/tianon/gosu/releases/download/$GOSU_VERSION/gosu-$(dpkg --print-architecture)" \
    && chmod +x /usr/local/bin/gosu

ADD entrypoint.sh /usr/local/bin/entrypoint.sh
RUN chmod +x /usr/local/bin/entrypoint.sh

ENTRYPOINT ["/usr/local/bin/entrypoint.sh"]

WORKDIR /

EXPOSE 5432
CMD ["postgres"]
