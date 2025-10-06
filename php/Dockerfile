FROM php:8.4-fpm

RUN apt update && apt upgrade -y && apt install -y \
    curl locales \
    libzip-dev libonig-dev libpng-dev libjpeg-dev \
    libmariadb-dev libxml2-dev libcurl4-openssl-dev

# 日本語ロケールを有効化
RUN sed -i 's/^# ja_JP.UTF-8 UTF-8/ja_JP.UTF-8 UTF-8/' /etc/locale.gen && locale-gen

# 環境変数を設定
ENV LANG ja_JP.UTF-8
ENV LANGUAGE ja_JP:ja
ENV LC_ALL ja_JP.UTF-8

RUN docker-php-ext-install pdo_mysql
RUN docker-php-ext-install xml
RUN docker-php-ext-install curl
RUN docker-php-ext-install zip

# ビルドに必要な依存関係をインストール
# これらは拡張機能によって異なる場合があります

# Download the package to configure the Microsoft repo
RUN curl -sSL -O https://packages.microsoft.com/config/debian/12/packages-microsoft-prod.deb

# Install the package
RUN dpkg -i ./packages-microsoft-prod.deb
# Delete the file
RUN rm ./packages-microsoft-prod.deb

RUN apt update
RUN ACCEPT_EULA=Y apt install -y msodbcsql18 mssql-tools18 unixodbc-dev libgssapi-krb5-2 && apt clean && rm -rf /var/lib/apt/lists/*

# peclコマンドで拡張機能をインストールし、有効化する
RUN pecl install sqlsrv pdo_sqlsrv

COPY ./ini/*.ini /usr/local/etc/php/conf.d/
COPY ./conf/www.conf /usr/local/etc/php-fpm.d/zzz-docker.conf

# コンテナ起動時に実行されるコマンド
CMD ["php-fpm"]
