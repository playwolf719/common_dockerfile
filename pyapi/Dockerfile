FROM sznlp_pyapienv_v2:v01
# Install uWSGI
RUN pip install --index-url https://pypi.douban.com/simple pyltp torch==0.3.1 spaCy==2.0.11 thriftpy numpy==1.14.2
# 更新阿里云的wheezy版本包源
RUN echo "deb http://mirrors.aliyun.com/debian wheezy main contrib non-free" > /etc/apt/sources.list && \
    echo "deb-src http://mirrors.aliyun.com/debian wheezy main contrib non-free" >> /etc/apt/sources.list  && \
    echo "deb http://mirrors.aliyun.com/debian wheezy-updates main contrib non-free" >> /etc/apt/sources.list && \
    echo "deb-src http://mirrors.aliyun.com/debian wheezy-updates main contrib non-free" >> /etc/apt/sources.list && \
    echo "deb http://mirrors.aliyun.com/debian-security wheezy/updates main contrib non-free" >> /etc/apt/sources.list && \
    echo "deb-src http://mirrors.aliyun.com/debian-security wheezy/updates main contrib non-free" >> /etc/apt/sources.list

# install redis
COPY docker/redis-4.0.10.tar.gz redis-4.0.10.tar.gz
RUN tar zxvf redis-4.0.10.tar.gz && rm -f redis-4.0.10.tar.gz && cd redis-4.0.10 && make
# Copy the modified Nginx conf
COPY docker/nginx.conf /etc/nginx/conf.d/
# Which uWSGI .ini file should be used, to make it customizable
#ENV UWSGI_INI /${PROJ_NAME}/conf/uwsgi.ini.bak
# Copy the base uWSGI ini file to enable default dynamic uwsgi process number

# COPY conf/uwsgi.ini /etc/uwsgi/

# Custom Supervisord config
COPY docker/supervisord.conf /etc/supervisor/conf.d/supervisord.conf

COPY docker/redis.conf /etc/redis.conf

# By default, run 2 processes
#ENV UWSGI_CHEAPER 2

# By default, when on demand, run up to 16 processes
#ENV UWSGI_PROCESSES 16

# By default, allow unlimited file sizes, modify it to limit the file sizes
# To have a maximum of 1 MB (Nginx's default) change the line to:
# ENV NGINX_MAX_UPLOAD 1m
#ENV NGINX_MAX_UPLOAD 0

# By default, Nginx will run a single worker process, setting it to auto
# will create a worker for each CPU core
ENV NGINX_WORKER_PROCESSES 1
ENV OMP_NUM_THREADS 1
ENV NGINX_WORKER_CONNECTIONS 10000
#ENV NGINX_WORKER_OPEN_FILES 60000

# By default, Nginx listens on port 80.
# To modify this, change LISTEN_PORT environment variable.
# (in a Dockerfile or with an option for `docker run`)
ENV LISTEN_PORT 5038

# Copy the entrypoint that will generate Nginx additional configs
COPY docker/entrypoint.sh /entrypoint.sh
RUN chmod +x /entrypoint.sh

ENTRYPOINT ["/entrypoint.sh"]

############# start #############
ARG PROJ_NAME=QAdemo
ENV LD_LIBRARY_PATH /${PROJ_NAME}/modules/common_lib/so_lib:$LD_LIBRARY_PATH
ENV PATH="/redis-4.0.10/src:${PATH}"
ADD docker/my_requirement.txt /${PROJ_NAME}/my_requirement.txt
WORKDIR /${PROJ_NAME}

# last step
RUN pip install --index-url https://pypi.douban.com/simple -r my_requirement.txt
COPY . /${PROJ_NAME}
CMD ["/usr/bin/supervisord"]