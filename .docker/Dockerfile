FROM php:8.0-cli AS base

WORKDIR /usr/src/app

FROM base AS tools
COPY --from=composer:2 /usr/bin/composer /usr/local/bin/

RUN set -xe \
    && curl https://github.com/phar-io/phive/releases/download/0.15.0/phive-0.15.0.phar --location --output phive \
    && chmod +x phive \
    && mv phive /usr/local/bin/

RUN set -xe \
    && apt-get update \
    && apt-get install -y --no-install-recommends \
        gpg \
        unzip \
    && rm -rf /var/lib/apt/lists/*

FROM tools AS dev
ARG user=dev
ARG user_id
ARG group_id

RUN set -xe \
    && addgroup --gid $group_id $user \
    && adduser \
        --disabled-password \
        --gecos '' \
        --ingroup $user \
        --uid $user_id \
        $user \
    && apt-get update \
    && apt-get install -y --no-install-recommends sudo \
    && rm -rf /var/lib/apt/lists/* \
    && echo "$user ALL=(ALL) NOPASSWD: ALL" > /etc/sudoers.d/$user

USER $user

CMD [ "tail", "-f", "/dev/null" ]
