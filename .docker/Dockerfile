FROM php:8-fpm-alpine as runtime

# add fast track required php extensions
RUN apk update \
  && apk add --no-cache curl make coreutils icu-libs icu-dev libzip-dev libxslt-dev php-mysqli php-pdo_mysql postgresql-dev libpng-dev
RUN NPROC=$(grep -c ^processor /proc/cpuinfo 2>/dev/null || 1) \
    && docker-php-ext-install -j${NPROC} zip intl xsl mysqli pdo_mysql pdo_pgsql gd

FROM runtime AS build

# add composer for building
RUN echo "$(curl -sS https://composer.github.io/installer.sig) -" > composer-setup.php.sig \
    && curl -sS https://getcomposer.org/installer | tee composer-setup.php | sha384sum -c composer-setup.php.sig \
    && php composer-setup.php && rm composer-setup.php* \
    && chmod +x composer.phar && mv composer.phar /usr/local/bin/composer 

FROM build AS dev

RUN apk add --no-cache $PHPIZE_DEPS 

# add xdebug, see https://xdebug.org/docs/install#source
ARG XDEBG_REL=3.1.5
RUN curl -sS https://xdebug.org/files/xdebug-${XDEBG_REL}.tgz | tar xz \
    && cd xdebug-${XDEBG_REL} \
    && phpize \
    && ./configure --enable-xdebug \
    && make && make install \
    && docker-php-ext-enable xdebug \
    && cd .. && rm -rf xdebug-${XDEBG_REL}

# add tools
RUN apk add --no-cache bash git git-bash-completion docker-cli docker-cli-compose yarn sudo
# https://docs.docker.com/compose/cli-command/#compose-switch
RUN curl -fL https://raw.githubusercontent.com/docker/compose-switch/master/install_on_linux.sh -o dc.sh
RUN chmod +x dc.sh && sed -i 's/set -e/#set -e/' dc.sh
RUN ./dc.sh > /dev/null 2>&1 && rm dc.sh

# if user with id uid doesnt exist add her
ARG userid=1000 # symfony-user-id-which-should-exist-on-host
ARG user=symfony-user-who-should-exist-on-host
RUN id -u $userid > /dev/null 2>&1 || adduser -D -s /bin/bash -u $userid $user \
    && echo "$user  ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers

# give user docker permissions
ARG dockergid=1000 # docker-gid-which-should-exist-on-host
RUN addgroup -g ${dockergid} docker && addgroup $user docker

# customize xdebugging
COPY --chown=$user:$user bashrc /home/$user/.bashrc
COPY docker-php-ext-xdebug.ini /usr/local/etc/php/conf.d
COPY docker-php-ext-opcache.ini /usr/local/etc/php/conf.d

# install symfony CLI
ARG SYM_REL=5.4.10
ARG SYM_REP=https://github.com/symfony-cli/symfony-cli/releases/download
ARG SYM_ARCH=x86_64
RUN curl -L -sS ${SYM_REP}/v${SYM_REL}/symfony-cli_${SYM_REL}_${SYM_ARCH}.apk -o /tmp/symfony.apk \
    && apk add --no-cache --allow-untrusted /tmp/symfony.apk \
    && rm /tmp/symfony.apk

USER $user
RUN cp /usr/share/bash-completion/completions/git ~/git-completion.bash && echo ". ~/git-completion.bash" >> ~/.bashrc