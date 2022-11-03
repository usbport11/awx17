Need web access to https://pypi.org/ (on install time)<br>
Need web access to https://hub.docker.com/ (on install time)<br>
Need web access to https://github.com/ (on install time)<br>

<pre>
nano /etc/networks/interfaces
iface enp0s3 inet static
  address 192.168.1.200
  netmask 255.255.255.0
  gateway 192.168.1.1
  dns-nameservers 192.168.1.1
</pre>

<pre>
ifdown enp0s3
ifup enp0s3
</pre>

<pre>
nano bash.bashrc
alias ls='ls --color=auto'
alias grep='grep --color=auto'
</pre>

<pre>
nano /etc/apt/sources.list
deb http://ftp.ru.debian.org/debian bullseye main contrib
deb http://ftp.ru.debian.org/debian bullseye-updates main contrib
<s>deb [arch=amd64] https://download.docker.com/linux/debian bullseye stable</s>
</pre>

<pre>
apt update
apt upgrade
</pre>

Is it need to install gcc?
<pre>
apt install acl
apt install nginx
apt install ansible
apt install postgresql
apt install redis
apt install git
apt install python3-pip
apt install python3-psycopg2
apt install python3-ldap
apt install rsync
apt install subversion
apt install sudo
apt install unzip
apt install pwgen
apt install net-tools
apt install supervisord
apt install gcc
apt install libpcre3-dev
apt install libpython3.9-dev
apt install pkg-config libxml2-dev libxmlsec1-dev libxmlsec1-openssl
</pre>

<pre>
pip3 install python-ldap
pip3 install psutil
pip3 install cffi
pip3 install psycopg2
pip3 install supervisor
pip3 install virtualenv
pip3 install uwsgi
pip3 install xmlsec
</pre>

<pre>
rm -rf /root/.cache
rm -rf /tmp/*
</pre>

<pre>
cd /tmp
wget https://github.com/ansible/awx/archive/17.1.0.zip
unzip 17.1.0.zip
</pre>

<pre>
mkdir -p /var/lib/awx
mkdir -p /etc/tower
mkdir -p /etc/tower/conf.d
mkdir -p /var/lib/awx/rsyslog
</pre>

Seach layer with size 115 mb. Extract it and copy to /var/lib/awx
<pre>
mkdir -p /tmp/awx-17.1.0
cd /tmp
wget https://raw.githubusercontent.com/moby/moby/master/contrib/download-frozen-image-v2.sh
chmod 755 download-frozen-image-v2.sh
./download-frozen-image-v2.sh /tmp/awx-17.1.0 ansible/awx:17.1.0
</pre>

<pre>
cp /tmp/awx-17.1.0/installer/roles/image_build/files/rsyslog.conf /var/lib/awx/rsyslog/rsyslog.conf
cp /tmp/awx-17.1.0/installer/roles/image_build/files/launch_awx.sh /usr/bin/launch_awx.sh
cp /tmp/awx-17.1.0/installer/roles/image_build/files/launch_awx_task.sh /usr/bin/launch_awx_task.sh
cp /tmp/awx-17.1.0/installer/roles/image_build/files/settings.py /etc/tower/settings.py
cp /tmp/awx-17.1.0/installer/roles/image_build/files/supervisor.conf /etc/supervisord.conf
cp /tmp/awx-17.1.0/installer/roles/image_build/files/supervisor_task.conf /etc/supervisord_task.conf
cp /tmp/awx-17.1.0/tools/scripts/config-watcher /usr/bin/config-watcher
ln -s /var/lib/awx/venv/awx/bin/awx-manage /usr/bin/awx-manage
</pre>

<pre>
chmod 755 /usr/bin/config-watcher
chmod 755 /var/lib/awx/venv/awx/bin/awx-manage
chmod 755 /usr/bin/launch_awx.sh
chmod 755 /usr/bin/launch_awx_task.sh
</pre>

<pre>
nano /etc/tower/conf.d/environment.sh
DATABASE_USER=awx
DATABASE_NAME=awx
DATABASE_HOST=localhost
DATABASE_PORT=5432
DATABASE_PASSWORD=awx
AWX_ADMIN_USER=admin
AWX_ADMIN_PASSWORD=admin
</pre>

<pre>
chmod 600 /etc/tower/conf.d/environment.sh
</pre>

<pre>
nano /etc/tower/conf.d/credentials.py
DATABASES = {
    'default': {
        'ATOMIC_REQUESTS': True,
        'ENGINE': 'django.db.backends.postgresql',
        'NAME': "awx",
        'USER': "awx",
        'PASSWORD': "awx",
        'HOST': "localhost",
        'PORT': "5432",
    }
}
BROADCAST_WEBSOCKET_SECRET = "JikvTXBoeUZ9dShpWUJMK0R0dXh3eENQL0ZdLStNM1RFZkhKUXIoLWpjMU1zWj14KDsmPWElLztCVjRuIUhlI2t4PmNjeXM/MXJHJWExZm47OUwjanl2SkFiVCZDYW9dNSlROnNEO2NtX0lpVjB5V1VeaVl6eHd0UnFCSWs9ZC0="
</pre>

<pre>
chmod 600 /etc/tower/conf.d/credentials.py
</pre>

<pre>
pwgen -N 1 -s 30 > /etc/tower/SECRET_KEY
</pre>

<pre>
chgrp -R root /var/lib/awx/vendor/awx_ansible_collections     
chmod -R g+rw /var/lib/awx/vendor/awx_ansible_collections
</pre>

<pre>
touch /tmp/dirs.sh
chmod /tmp/755 dirs.sh
nano dirs.sh
dirs=(/var/lib/awx /var/lib/awx/rsyslog /var/lib/awx/rsyslog/conf.d /var/run/awx-rsyslog /var/log/tower /var/run/supervisor)
for dir in ${dirs[@]};
do mkdir -m 0775 -p $dir ; chmod g+rw $dir ; chgrp root $dir ;
done
/tmp/dirs.sh
</pre>

<pre>
chmod 755 /var/lib/awx/venv/awx/bin/*
chmod 644 /var/lib/awx/venv/awx/bin/activ*
</pre>

<pre>
mv /var/lib/awx/venv/awx/bin/python3 /var/lib/awx/venv/awx/bin/python3.old
ln -s /usr/bin/python3.9 /var/lib/awx/venv/awx/bin/python3
mv /var/lib/awx/venv/awx/bin/uwsgi /var/lib/awx/venv/awx/bin/uwsgi.old
ln -s /usr/local/bin/uwsgi /var/lib/awx/venv/awx/bin/uwsgi
</pre>

Not sure about this. Need check
<pre>
mv /var/lib/awx/venv/awx/lib/python /var/lib/awx/venv/awx/lib/python3.9
cd /var/lib/awx/venv
virtualenv awx -p python3.9
</pre>

<pre>
cd /var/lib/awx/venv/awx/lib/python3.9/site-packages
rm -fr ldap*
rm -fr _ldap.cpython-39-x86_64-linux-gnu.so
rm -fr psutil*
rm -fr cffi*
rm -fr _cffi_backend.cpython-36m-x86_64-linux-gnu.so
rm -fr psycopg2*
rm -fr xmlsec*
rm -fr lxml*
</pre>

<pre>
cp -r /usr/local/lib/python3.9/dist-packages/ldap* /var/lib/awx/venv/awx/lib/python3.9/site-packages
cp /usr/local/lib/python3.9/dist-packages/_ldap.cpython-39-x86_64-linux-gnu.so /var/lib/awx/venv/awx/lib/python3.9/site-packages
cp /usr/local/lib/python3.9/dist-packages/ldapurl.py
cp -r /usr/local/lib/python3.9/dist-packages/psutil* /var/lib/awx/venv/awx/lib/python3.9/site-packages
cp -r /usr/local/lib/python3.9/dist-packagescffi* /var/lib/awx/venv/awx/lib/python3.9/site-packages
cp /usr/local/lib/python3.9/_cffi_backend.cpython-36m-x86_64-linux-gnu.so /var/lib/awx/venv/awx/lib/python3.9/site-packages
cp -r /usr/lib/python3/dist-packages/psycopg2* /var/lib/awx/venv/awx/lib/python3.9/site-packages
cp -r /usr/local/lib/python3.9/dist-packages/xmlsec* /var/lib/awx/venv/awx/lib/python3.9/site-packages
cp /usr/local/lib/python3.9/dist-packages/xmlsec.cpython-39-x86_64-linux-gnu.so /var/lib/awx/venv/awx/lib/python3.9/site-packages
cp -r /usr/local/lib/python3.9/dist-packages/lxml* /var/lib/awx/venv/awx/lib/python3.9/site-packages
</pre>

Check that awx-manage at least starting
<pre>
./awx-manage --help
</pre>

<pre>
nano /etc/postgresql/13/main/pg_hba.conf
local   all             postgres                                trust
local   all             all                                     trust
</pre>

<pre>
systemctl restart postgresql
psql -U postgres
postgres=# create database awx;
postgres=# create user awx with encrypted password 'awx';
postgres=# grant all privileges on database awx to awx;
</pre>

<pre>
cp /etc/nginx/nginx.conf /etc/nginx/nginx.conf.old
> /etc/nginx/nginx.conf
nano /etc/nginx/nginx.conf

worker_processes  1;
pid        /run/nginx.pid;

events {
    worker_connections  1024;
}

http {
    include       /etc/nginx/mime.types;
    default_type  application/octet-stream;
    server_tokens off;

    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';

    access_log /var/log/nginx/access.log;
    error_log /var/log/nginx/error.log;

    map $http_upgrade $connection_upgrade {
        default upgrade;
        ''      close;
    }

    sendfile        on;

    upstream uwsgi {
        server 127.0.0.1:8050;
    }

    upstream daphne {
        server 127.0.0.1:8051;
    }

    server {
        listen 8052 default_server;
        server_name _;
        keepalive_timeout 65;
        add_header Strict-Transport-Security max-age=15768000;
        add_header X-Frame-Options "DENY";

        location /nginx_status {
            stub_status on;
            access_log off;
            allow 127.0.0.1;
            deny all;
        }

        location /static/ {
            alias /var/lib/awx/public/static/;
        }

        location /favicon.ico { alias /var/lib/awx/public/static/favicon.ico; }

        location /websocket {
            proxy_pass http://daphne;
            proxy_http_version 1.1;
            proxy_buffering off;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header X-Forwarded-Proto https;
            proxy_set_header Host $http_host;
            proxy_redirect off;
            proxy_set_header Upgrade $http_upgrade;
            proxy_set_header Connection $connection_upgrade;
        }

        location / {
            rewrite ^(.*)$http_host(.*[^/])$ $1$http_host$2/ permanent;
            uwsgi_read_timeout 120s;
            uwsgi_pass uwsgi;
            include /etc/nginx/uwsgi_params;
			proxy_set_header X-Forwarded-Port 443;
            uwsgi_param HTTP_X_FORWARDED_PORT 443;
        }
    }
}
</pre>

<pre>
systemctl disable supervisor
systemctl stop supervisor
systemctl disable nginx
systemctl stop nginx
</pre>

Directoties deleted after restart server!
<pre>
mkdir /var/run/supervisor
mkdir /var/run/awx-rsyslog
</pre>

<pre>
nano /etc/redis/redis.conf
unixsocket /var/run/redis/redis.sock
unixsocketperm 700
</pre>

<pre>
nano t /usr/bin/launch_awx.sh

#!/usr/bin/env bash
if [ `id -u` -ge 500 ]; then
    echo "awx:x:`id -u`:`id -g`:,,,:/var/lib/awx:/bin/bash" >> /tmp/passwd
    cat /tmp/passwd > /etc/passwd
    rm /tmp/passwd
fi

source /etc/tower/conf.d/environment.sh

ANSIBLE_REMOTE_TEMP=/tmp ANSIBLE_LOCAL_TEMP=/tmp ansible -i "127.0.0.1," -c local -v -m wait_for -a "host=$DATABASE_HOST port=$DATABASE_PORT" all
ANSIBLE_REMOTE_TEMP=/tmp ANSIBLE_LOCAL_TEMP=/tmp ansible -i "127.0.0.1," -c local -v -m postgresql_db --become-user $DATABASE_USER -a "name=$DATABASE_NAME owner=$DATABASE_USER login_user=$DATABASE_USER login_host=$DATABASE_HOST login_password=$DATABASE_PASSWORD port=$DATABASE_PORT" all

awx-manage collectstatic --noinput --clear

if [ -z "$AWX_SKIP_MIGRATIONS" ]; then
    awx-manage migrate --noinput
fi

if [ -z "$AWX_SKIP_PROVISION_INSTANCE" ]; then
    awx-manage provision_instance --hostname=$(hostname)
fi

if [ -z "$AWX_SKIP_REGISTERQUEUE" ]; then
    awx-manage register_queue --queuename=tower --instance_percent=100
fi

unset $(cut -d = -f -1 /etc/tower/conf.d/environment.sh)

supervisord -c /etc/supervisord.conf

</pre>

<pre>
awx-manage migrate --no-input
awx-manage createsuperuser --username admin
awx-manage create_preload_data (not needed)
</pre>

<pre>
nano /etc/supervisord.conf

[supervisord]
nodaemon = True
umask = 022
logfile = /dev/stdout
logfile_maxbytes = 0
pidfile = /var/run/supervisor/supervisor.web.pid

[program:nginx]
command = nginx -g "daemon off;"
autostart = true
autorestart = true
stopwaitsecs = 5
stdout_logfile=/dev/stdout
stdout_logfile_maxbytes=0
stderr_logfile=/dev/stderr
stderr_logfile_maxbytes=0

[program:uwsgi]
command = /var/lib/awx/venv/awx/bin/uwsgi --chdir=/var/lib/awx --home=/var/lib/awx/venv/awx --socket 127.0.0.1:8050 --module=awx.wsgi:application --vacuum --processes=5 --harakiri=120 --no-orphans --master --max-requests=1000 --master-fifo=/var/lib/awx/awxfifo --lazy-apps -b 32768
directory = /var/lib/awx
autostart = true
autorestart = true
stopwaitsecs = 15
stopsignal = INT
stdout_logfile=/dev/stdout
stdout_logfile_maxbytes=0
stderr_logfile=/dev/stderr
stderr_logfile_maxbytes=0

[program:daphne]
command = /var/lib/awx/venv/awx/bin/daphne -b 127.0.0.1 -p 8051 --websocket_timeout -1 awx.asgi:channel_layer
directory = /var/lib/awx
autostart = true
autorestart = true
stopwaitsecs = 5
stdout_logfile=/dev/stdout
stdout_logfile_maxbytes=0
stderr_logfile=/dev/stderr
stderr_logfile_maxbytes=0

[program:wsbroadcast]
command = awx-manage run_wsbroadcast
directory = /var/lib/awx
autostart = true
autorestart = true
stopwaitsecs = 5
stdout_logfile=/dev/stdout
stdout_logfile_maxbytes=0
stderr_logfile=/dev/stderr
stderr_logfile_maxbytes=0

[program:awx-rsyslogd]
command = rsyslogd -n -i /var/run/awx-rsyslog/rsyslog.pid -f /var/lib/awx/rsyslog/rsyslog.conf
autostart = true
autorestart = true
stopwaitsecs = 5
stopsignal=TERM
stopasgroup=true
killasgroup=true
redirect_stderr=true
stdout_logfile=/dev/stderr
stdout_logfile_maxbytes=0

[program:dispatcher]
command = awx-manage run_dispatcher
directory = /var/lib/awx
autostart = true
autorestart = true
stopwaitsecs = 5
stdout_logfile=/dev/stdout
stdout_logfile_maxbytes=0
stderr_logfile=/dev/stderr
stderr_logfile_maxbytes=0

[program:callback-receiver]
command = awx-manage run_callback_receiver
directory = /var/lib/awx
autostart = true
autorestart = true
stopwaitsecs = 5
stdout_logfile=/dev/stdout
stdout_logfile_maxbytes=0
stderr_logfile=/dev/stderr
stderr_logfile_maxbytes=0

[group:tower-processes]
programs=nginx,uwsgi,daphne,wsbroadcast,awx-rsyslogd,dispatcher,callback-receiver
priority=5

[eventlistener:awx-config-watcher]
command=/usr/bin/config-watcher
stderr_logfile=/dev/stdout
stderr_logfile_maxbytes=0
stdout_logfile=/dev/stdout
stdout_logfile_maxbytes=0
events=TICK_60
priority=0

[unix_http_server]
file=/var/run/supervisor/supervisor.web.sock

[supervisorctl]
serverurl=unix:///var/run/supervisor/supervisor.web.sock ; use a unix:// URL  for a unix socket

[rpcinterface:supervisor]
supervisor.rpcinterface_factory = supervisor.rpcinterface:make_main_rpcinterface
</pre>

<pre>
Compare cluster id and uuid

nano /etc/tower/settings.py

CLUSTER_HOST_ID = "awx"
SYSTEM_UUID = '00000000-0000-0000-0000-000000000000'

su - postgres
psql
\c awx
select * from main_instance;
</pre>

<pre>
launch_awx.sh
</pre>

After check web - need check tasks run <br>

<pre>
Delete pending

awx-manage shell_plus

Inventory.objects.filter(pending_deletion=True).update(pending_deletion=False)
Inventory.objects.filter(id=ID).delete()
</pre>

<b>LDAP</b><br>

Web - Settings - Authorization - LDAP Settings - Default - Edit<br>

LDAP Server URI <br>
LDAP Bind Password <br>
LDAP Bind DN <br>
LDAP Require Group <br>

Save
