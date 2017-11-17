FROM ubuntu:16.04
#FROM ubuntu-nginx-aptget-install-req
ENV DEBIAN_FRONTEND noninteractive

ENV TZ=Europe/Kiev
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

RUN apt-get update \
    && apt-get upgrade -y \
    && apt-get install -y --no-install-recommends \
	software-properties-common git unzip libxml2-dev \
	libbz2-dev libcurl4-openssl-dev libmcrypt-dev libmhash2 \
	libmhash-dev libpcre3 libpcre3-dev make build-essential \
	libxslt1-dev git unzip libgd2-xpm-dev libgeoip-dev \
	libpam-dev libgoogle-perftools-dev lua5.1 liblua5.1-0 \
	liblua5.1-0-dev checkinstall wget libssl-dev  \
        mercurial meld \
    && rm -rf /var/lib/apt/lists/* \
    && rm -rf /usr/share/doc/* \
    && rm -rf /usr/share/man/* \

    && git clone http://luajit.org/git/luajit-2.0.git /usr/src/luajit-2.0 \
    && git clone https://github.com/simpl/ngx_devel_kit.git /usr/src/ngx_devel_kit \
    && git clone https://github.com/openresty/lua-nginx-module.git /usr/src/lua-nginx-module \
    && git clone https://github.com/openresty/echo-nginx-module.git /usr/src/echo-nginx-module \

    && cd /usr/src/luajit-2.0 && make && make install && cd .. \
    && export LUAJIT_LIB=/usr/local/lib \
    && export LUAJIT_INC=/usr/local/include/luajit-2.0 \

    && ldconfig \

    && cd /usr/src/ \

    && hg clone http://hg.nginx.org/nginx \
    && cd /usr/src/nginx && cp ./auto/configure . && ./configure --with-http_xslt_module --with-http_ssl_module --with-http_mp4_module --with-http_flv_module \
        --with-http_secure_link_module --with-http_dav_module \
        --with-http_geoip_module --with-http_image_filter_module \
        --with-mail --with-mail_ssl_module --with-google_perftools_module \
        --with-debug --with-pcre-jit --with-ipv6 --with-http_stub_status_module --with-http_realip_module \
        --with-http_addition_module --with-http_gzip_static_module --with-http_sub_module \
        --add-module=/usr/src/ngx_devel_kit \
        --add-module=/usr/src/lua-nginx-module \
        --add-module=/usr/src/echo-nginx-module \


    && make -j8 && make install \
    && rm -rf /usr/src/*

EXPOSE 80

ADD ./conf /usr/local/nginx/conf

#RUN wget https://github.com/jwilder/dockerize/releases/download/v0.5.0/dockerize-linux-amd64-v0.5.0.tar.gz \
#    && tar -C /usr/local/bin -xvzf dockerize-linux-amd64-v0.5.0.tar.gz

#ADD default.tmpl /usr/local/nginx/conf/conf/default.tmpl

RUN rm /usr/local/nginx/conf/nginx.conf
RUN wget https://raw.githubusercontent.com/dburianov/opsworksio/master/docker/nginx.conf -O /usr/local/nginx/conf/nginx.conf
RUN wget https://raw.githubusercontent.com/dburianov/opsworksio/master/index.html -O /usr/local/nginx/html/index.html


#CMD ["dockerize", "-template", "/usr/local/nginx/conf/conf/default.tmpl:/usr/local/nginx/conf/conf/default.conf", "-stdout", "/usr/local/nginx/logs/access.log", "-stderr", "/usr/local/nginx/logs/error.log", "/usr/local/nginx/sbin/nginx"]
#CMD ["dockerize", "-stdout", "/usr/local/nginx/logs/access.log", "-stderr", "/usr/local/nginx/logs/error.log", "/usr/local/nginx/sbin/nginx"]

#CMD dockerize -stdout /varlog/access.log -stderr /var/log/error.log /usr/local/nginx/sbin/nginx
#CMD dockerize /usr/local/nginx/sbin/nginx /bin/bash
#CMD /usr/local/nginx/sbin/nginx -g 'daemon off;'

#EXPOSE 1935
#EXPOSE 443
#EXPOSE 22

VOLUME /usr/local/nginx/conf
VOLUME /usr/local/nginx/html
VOLUME /usr/local/nginx/lua
VOLUME /usr/local/nginx/logs
VOLUME /usr/local/nginx/cache

#COPY
ADD . /scripts 

CMD ["/scripts/run.sh"]

RUN chmod +x /scripts/run.sh


#    && git clone https://github.com/arut/nginx-rtmp-module.git /usr/src/nginx-rtmp-module \
#    && git clone https://github.com/arut/nginx-ts-module.git /usr/src/nginx-ts-module \
#        --add-module=/usr/src/nginx-ts-module \
#        --add-module=/usr/src/nginx-rtmp-module \
