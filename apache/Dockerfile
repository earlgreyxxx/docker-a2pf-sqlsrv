FROM httpd:2.4

# ロケールパッケージをインストール & 日本語ロケールを有効化
RUN apt update && apt install -y locales \
    && sed -i 's/^# ja_JP.UTF-8 UTF-8/ja_JP.UTF-8 UTF-8/' /etc/locale.gen && locale-gen \
    && rm -rf /var/lib/apt/lists/*

# 環境変数を設定
ENV LANG ja_JP.UTF-8
ENV LANGUAGE ja_JP:ja
ENV LC_ALL ja_JP.UTF-8

# mod_proxy_fcgi を有効化
#RUN sed -i '/^#LoadModule proxy_fcgi_module/s/^#//g' /usr/local/apache2/conf/httpd.conf
RUN echo LoadModule proxy_module modules/mod_proxy.so >> /usr/local/apache2/conf/httpd.conf
RUN echo LoadModule proxy_fcgi_module modules/mod_proxy_fcgi.so >> /usr/local/apache2/conf/httpd.conf

# サイト設定ファイルをコンテナ内にコピー
COPY ./vhost.conf /usr/local/apache2/conf/extra/httpd-vhosts.conf

# vhost.conf を読み込むように設定
RUN echo "Include conf/extra/httpd-vhosts.conf" >> /usr/local/apache2/conf/httpd.conf

# コンテナ起動時に実行されるコマンド
CMD ["httpd-foreground"]
