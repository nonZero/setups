# Setting up `uwsgi` and `nginx`

## Create a uwsgi Configuration File

Instead of using `runserver` we will be using `uwsgi` which runs our django app via WSGI.

`uwsgi` is already installed on your server.  It requires a configuration file to detect and run your django app.   Add this to your fabfile:

    from io import StringIO

    UWSGI_CONF = """
    [uwsgi]
    plugin = python3
    virtualenv = {env.venv_path}
    chdir = {env.code_dir}
    wsgi-file = {env.wsgi_file}
    processes = 4
    threads = 1
    stats = 127.0.0.1:{env.stats_port}
    """

    env.app_name = "myproject"
    env.wsgi_file = "myproject/wsgi.py"
    env.stats_port = 9000


    @task
    def create_uwsgi_conf():
        conf = UWSGI_CONF.format(env=env)
        filename = f"/etc/uwsgi/apps-available/{env.app_name}.ini"
        enabled = f"/etc/uwsgi/apps-enabled/{env.app_name}.ini"
        put(StringIO(conf), filename, use_sudo=True, )
        sudo(f"ln -sf {filename} {enabled}")
        sudo("service uwsgi stop")
        sudo("service uwsgi start")

and run:

    fab create_uwsgi_conf

Your uwsgi is now running, and could be accessed via the unix socket file `/run/uwsgi/app/myproject/socket` .

## Create An nginx Configuration File
`nginx` is a fast webserver that interacts with uwsgi and serves static files.
Add this


    NGINX_CONF = """
    server {{
        listen      80;
        server_name {host};
        charset     utf-8;

        location /static/ {{
            alias {env.static_path};
        }}

        location / {{
            uwsgi_pass  unix://{env.uwsgi_socket};
            include     uwsgi_params;
        }}
    }}"""

    env.uwsgi_socket = f"/run/uwsgi/app/{env.app_name}/socket"
    env.static_path = f"{env.code_dir}/collected_static/"


    @task
    def create_nginx_conf():
        conf = NGINX_CONF.format(
            host=env.hosts[0],
            env=env,
        )
        filename = f"/etc/nginx/sites-available/{env.app_name}.conf"
        enabled = f"/etc/nginx/sites-enabled/{env.app_name}.conf"
        put(StringIO(conf), filename, use_sudo=True, )
        sudo(f"ln -sf {filename} {enabled}")

        sudo("rm -vf /etc/nginx/sites-enabled/default")

        sudo("nginx -t")

        sudo("service nginx reload")

and run:

    fab create_nginx_conf


## Troubleshooting
Add a task to show latest nginx log entries:

    @task
    def nginx_log():
        sudo("tail /var/log/nginx/error.log")

