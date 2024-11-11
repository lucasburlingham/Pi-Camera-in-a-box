# Set Base Image
FROM balenalib/rpi-raspbian

# Maintainer
LABEL maintainer="Lucas Burlingham <lucas@lucasburlingham.me>"
LABEL description="Docker container for NGINX RTMP Server with RTSP Support. Based on benjaminmaynard/nginx-rtsp"

# Install Essential Packages
# Setup our build environment in /root then compile NGINX with RTMP Module
# Cleanup Build Dependencies and Cache

RUN apt-get update && DEBIAN_FRONTEND=noninteractive apt-get install -y build-essential libpcre3 libpcre3-dev libssl-dev build-essential make gcc libpcre++-dev zlib1g-dev libbz2-dev libxslt1-dev libxml2-dev libgd-dev libgeoip-dev libgoogle-perftools-dev libperl-dev wget libcurl4-openssl-dev git && cd /root && git clone https://github.com/sergey-dryabzhinsky/nginx-rtmp-module.git && wget https://nginx.org/download/nginx-1.26.2.tar.gz -O nginx.tar.gz && mkdir nginx && tar xf nginx.tar.gz -C nginx --strip-components 1 && cd /root/nginx && ./configure --with-http_ssl_module --add-module=../nginx-rtmp-module && make -j $(nprocs) && make install && rm -rf /root/nginx && rm -rf /root/nginx-rtmp-module && rm /root/nginx.tar.gz && apt-get remove --purge -y git build-essential make wget gcc libpcre++-dev zlib1g-dev libbz2-dev libxslt1-dev libxml2-dev libgd-dev libgeoip-dev libgoogle-perftools-dev libperl-dev libcurl4-openssl-dev && apt-get autoremove --purge -y && rm -rf /var/lib/apt/lists/ && mkdir -p /var/lib/apt/lists

# Copy NGINX Configuration
COPY resources/nginx.conf /usr/local/nginx/conf/nginx.conf

# Setup Web UI
RUN mkdir -p /var/www/html
COPY resources/web-ui /var/www/html

# Set EntryPoint Script
COPY resources/entrypoint.sh /
RUN chmod +x /entrypoint.sh
ENTRYPOINT ["/entrypoint.sh"]
