FROM alpine:3.8

RUN set -ex \
            && apk --no-cache add su-exec

ENV BITCOIN_ABC_VERSION=23.0.0
ENV BITCOIN_ABC_DIR="bitcoin-cash-node-${BITCOIN_ABC_VERSION}"
ENV BITCOIN_ABC_DIST="bitcoin-cash-node-${BITCOIN_ABC_VERSION}-x86_64-linux-gnu.tar.gz"
ENV BITCOIN_ABC_DATA=/root/.bitcoin

# https://github.com/bitcoin-cash-node/bitcoin-cash-node/releases/tag/v23.0.0
RUN wget https://github.com/bitcoin-cash-node/bitcoin-cash-node/releases/download/v23.0.0/bitcoin-cash-node-23.0.0-x86_64-linux-gnu.tar.gz \
            && tar -xzf $BITCOIN_ABC_DIST \
            && mv ${BITCOIN_ABC_DIR}/bin/bitcoind ${BITCOIN_ABC_DIR}/bin/bitcoin-cli ${BITCOIN_ABC_DIR}/bin/bitcoin-tx /usr/local/bin/ \
            && rm -r $BITCOIN_ABC_DIR \
            && rm $BITCOIN_ABC_DIST \
            && addgroup -S bitcoin && adduser -S -G bitcoin bitcoin

# Install glibc binaries needed by bitcoin-abc
RUN apk --no-cache --allow-untrusted -X https://apkproxy.herokuapp.com/sgerrand/alpine-pkg-glibc add glibc glibc-bin

COPY docker-entrypoint.sh /entrypoint.sh
RUN chmod +x /entrypoint.sh

VOLUME ["/root/.bitcoin"]

EXPOSE 8332 8333 18332 18333 18444

ENTRYPOINT ["/entrypoint.sh"]

CMD ["bitcoind"]
