FROM alpine:3.8

RUN set -ex \
            && apk --no-cache add su-exec

ENV BITCOIN_ABC_VERSION=0.22.7
ENV BITCOIN_ABC_DIR="bitcoin-abc-${BITCOIN_ABC_VERSION}"
ENV BITCOIN_ABC_DIST="bitcoin-abc-${BITCOIN_ABC_VERSION}-x86_64-linux-gnu.tar.gz"
# https://download.bitcoinabc.org/0.22.7/fabien-sha256sums.0.22.6.asc
ENV BITCOIN_ABC_SHASUM="35e89781de6569763e5b8b395cc68c0c883fb79ccd2facf11b13f2409fa4d9d4  ${BITCOIN_ABC_DIST}"
ENV BITCOIN_ABC_DATA=/root/.bitcoin

RUN wget https://download.bitcoinabc.org/bchn/${BITCOIN_ABC_VERSION}/linux/${BITCOIN_ABC_DIST} \
            && echo "$BITCOIN_ABC_SHASUM" | sha256sum -c \
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
