FROM vikramchauhan/mysqlcli:alpine as mysqlcli

ENV MYSQLCLI_DIR=/mysqlcli

RUN mkdir $MYSQLCLI_DIR \
    && mkdir $MYSQLCLI_DIR/lib \
    && cp -fv /usr/bin/mysql* $MYSQLCLI_DIR \
    && ldd /usr/bin/mysql | cut -d '>' -f2 | awk '{print $1}' | xargs -Isrc cp src $MYSQLCLI_DIR/lib

FROM python:3-alpine
MAINTAINER Vikram Chauhan <me@vkrm.ch>

ARG RUN_USER=coco
ARG RUN_GROUP=coco
ARG RUN_UID=5555
ARG RUN_GID=5555
ARG RUN_PASS=coco123@
ARG DEPS="ca-certificates \
        wget \
        unzip \
        git"
ARG MYSQLCLI_DIR=/mysqlcli
ARG MYSQLCLI_OUTPUT_DIR=/output
ARG APP_CFG_DIR=/config

ENV RUN_USER=${RUN_USER} \
    RUN_GROUP=${RUN_GROUP} \
    RUN_UID=${RUN_UID} \
    RUN_GID=${RUN_GID} \
    LD_LIBRARY_PATH="$MYSQLCLI_DIR/lib" \
    PATH="$MYSQLCLI_DIR:$LD_LIBRARY_PATH:$PATH"

COPY --from=mysqlcli $MYSQLCLI_DIR $MYSQLCLI_DIR

RUN addgroup -g $RUN_GID $RUN_GROUP \
    && adduser -G $RUN_GROUP -u $RUN_UID -D $RUN_USER \
    && chown -R $RUN_USER:$RUN_GROUP $MYSQLCLI_DIR \
    && chmod -R 755 $MYSQLCLI_DIR \
    && apk add --no-cache --update ${DEPS} \
    && cd / \
    && wget https://github.com/vikc07/mysqlbackup/archive/master.zip \
    && unzip master.zip \
    && mv mysqlbackup-master app \
    && rm master.zip \
    && pip install -r app/requirements.txt \
    && mkdir $APP_CFG_DIR \
    && cp /app/mysqlbackup/cfg/sample_mysqlbackup.json $APP_CFG_DIR/mysqlbackup.json \
    && rm -rf /app/mysqlbackup/cfg \
    && ln -s /config /app/mysqlbackup/cfg \
    && mkdir $MYSQLCLI_OUTPUT_DIR \
    && chown -R $RUN_USER:$RUN_GROUP $MYSQLCLI_OUTPUT_DIR \
    && apk del ${DEPS} \
    && rm -rf /var/cache/apk/*

USER $RUN_USER
VOLUME /config

# Set entrypoint
ENTRYPOINT ["sh", "-c"]