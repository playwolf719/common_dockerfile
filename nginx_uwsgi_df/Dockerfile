FROM python:3.6-stretch

# add alibaba source
COPY sources.list /etc/apt/
# 更新阿里云的wheezy版本包源
#RUN echo "deb http://mirrors.aliyun.com/debian wheezy main contrib non-free" > /etc/apt/sources.list && \
#    echo "deb-src http://mirrors.aliyun.com/debian wheezy main contrib non-free" >> /etc/apt/sources.list  && \
#    echo "deb http://mirrors.aliyun.com/debian wheezy-updates main contrib non-free" >> /etc/apt/sources.list && \
#    echo "deb-src http://mirrors.aliyun.com/debian wheezy-updates main contrib non-free" >> /etc/apt/sources.list && \
#    echo "deb http://mirrors.aliyun.com/debian-security wheezy/updates main contrib non-free" >> /etc/apt/sources.list && \
#    echo "deb-src http://mirrors.aliyun.com/debian-security wheezy/updates main contrib non-free" >> /etc/apt/sources.list

RUN apt-get update && apt-get -y --allow-unauthenticated install nginx-full lua5.2 lua5.2-doc liblua5.2-dev luajit libnginx-mod-http-lua lua-cjson

# Install Supervisord
RUN apt-get install -y --allow-unauthenticated supervisor vim && rm -rf /var/lib/apt/lists/*

# Install uWSGI
RUN pip install --index-url https://pypi.douban.com/simple uwsgi


# Make NGINX run on the foreground
RUN echo "daemon off;" >> /etc/nginx/nginx.conf
# Remove default configuration from Nginx