FROM ubuntu:14.04

ENV DEBIAN_FRONTEND noninteractive

RUN apt-get update \
  && apt-get install -y -q software-properties-common \
  php5-common \
  php5-cli \
  php5-fpm \
  php5-dev \
  php5-gd \
  php5-curl \
  php5-mcrypt \
  libpcre3-dev \
  build-essential \
  curl \
  git \
  && rm -rf /var/lib/apt/lists/*

###########################################################################
# php-fpm setting
###########################################################################

RUN php5enmod mcrypt \
  && sed -ie 's/listen = \/var\/run\/php5-fpm.sock/listen = 9000/' /etc/php5/fpm/pool.d/www.conf \
  && sed -ie 's/;access.log = log\/$pool.access.log/access.log = \/proc\/self\/fd\/2/' /etc/php5/fpm/pool.d/www.conf \
  && sed -ie 's/;php_flag\[display_errors\] = off/php_flag\[display_errors\] = on/' /etc/php5/fpm/pool.d/www.conf \
  && sed -ie 's/;php_admin_value\[error_log\] = \/var\/log\/fpm-php.www.log/php_admin_value\[error_log\] = \/proc\/self\/fd\/2/' /etc/php5/fpm/pool.d/www.conf \
  && sed -ie 's/;php_admin_flag\[log_errors\] = on/php_admin_flag\[log_errors\] = on/' /etc/php5/fpm/pool.d/www.conf

###########################################################################
# php.ini setting
###########################################################################

RUN sed -ri 's/;date.timezone =/date.timezone = Asia\/Tokyo/' /etc/php5/cli/php.ini \
  && sed -ri 's/short_open_tag = Off/short_open_tag = On/' /etc/php5/cli/php.ini \
  && sed -ri 's/serialize_precision = 17/serialize_precision = 100/' /etc/php5/cli/php.ini \
  && sed -ri 's/expose_php = On/expose_php = Off/' /etc/php5/cli/php.ini \
  && sed -ri 's/html_errors = On/html_errors = Off/' /etc/php5/cli/php.ini \
  && sed -ri 's/;session.entropy_length = 32/session.entropy_length = 0/' /etc/php5/cli/php.ini \
  && sed -ri 's/;error_log = syslog/error_log = \/dev\/stderr/' /etc/php5/cli/php.ini

###########################################################################
# composer install
###########################################################################

COPY --from=composer /usr/bin/composer /usr/bin/composer

###########################################################################
# ruby install
###########################################################################
ARG RUBY_VERSION

RUN apt-get update \
  && apt-get install -y libffi-dev libreadline-dev \
  && git clone https://github.com/sstephenson/rbenv.git ~/.rbenv \
  && git clone https://github.com/sstephenson/ruby-build.git ~/.rbenv/plugins/ruby-build \
  && sh ~/.rbenv/plugins/ruby-build/install.sh \
  && echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.bashrc \
  && echo 'eval "$(rbenv init -)"' >> ~/.bashrc \
  && . ~/.bashrc \
  && rbenv install ${RUBY_VERSION} \
  && rbenv rehash \
  && rbenv global ${RUBY_VERSION} \
  && ln -s /root/.rbenv/shims/gem /usr/local/bin/gem

###########################################################################
# bundler install
###########################################################################
RUN gem install bundler \
  && ln -s /root/.rbenv/shims/bundle /usr/local/bin/bundle

###########################################################################
# node install
###########################################################################
ARG NODE_VERSION

RUN curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.11/install.sh | bash \
  && . ~/.nvm/nvm.sh \
  && nvm install ${NODE_VERSION} \
  && nvm use ${NODE_VERSION} \
  && nvm alias ${NODE_VERSION}

###########################################################################
# set node path (can be executed with 'docker-compose exec')
###########################################################################

RUN echo "" >> ~/.bashrc \
  && echo 'export NVM_DIR="$HOME/.nvm"' >> ~/.bashrc \
  && find ~/.nvm -type f -name node -exec ln -s {} /usr/local/bin/node \; \
  && NODE_MODS_DIR="/root/.nvm/versions/node/$(node -v)/bin" \
  && ln -s $NODE_MODS_DIR/npm /usr/local/bin/npm

WORKDIR /var/www/html/current

EXPOSE 9000

ENTRYPOINT ["/usr/sbin/php5-fpm", "-F"]
