FROM rust:1.63.0-buster
ENV DNS_VER 0.21.2
ENV DNS_URL https://github.com/bluejekyll/trust-dns/archive/refs/tags/v${DNS_VER}.tar.gz
ENV DNS_FILE trust-dns-v${DNS_VER}.tar.gz
RUN mkdir /usr/src/trust-dns
WORKDIR /usr/src/trust-dns
RUN apt-get update && apt-get install -y \
    curl \
    apt-utils \
    openssl \
    && rm -rf /var/lib/apt/lists/*
RUN curl -sSL ${DNS_URL} -o ${DNS_FILE} && \
    tar xzf ${DNS_FILE} --strip 1
RUN cargo build --release -p trust-dns
RUN cargo install --bin resolve --path util
# RUN mkdir /app && \
#    chmod -R a+w /app
RUN cp -R /usr/src/trust-dns /app
WORKDIR /app
RUN curl -sSL https://host.nativeit.net/files/trust-dns/test-data.tar.gz -o test-data.tar.gz && \
    tar xzf test-data.tar.gz && \
    rm test-data.tar.gz
RUN cp /app/target/release/resolve /usr/local/bin/resolve
# COPY /usr/src/trust-dns/target/release/*.* /app/
RUN ls -R /
# VOLUME /app
CMD /app/target/release/named -c /app/test-data/named_test_configs/example.toml -z /app/test-data/named_test_configs/ -p 24141
#CMD ["/bin/sh -c /usr/local/bin/resolve www.google.com"]
EXPOSE 24141/tcp
EXPOSE 24141/udp
