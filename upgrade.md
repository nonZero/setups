# Automating upgrades

To upgrade your project to the latest version from git, add this to your fabfile:

    @task
    def git_pull():
        with virtualenv():
            run("git pull", pty=False)

    @task
    def collect_static():
        m('collectstatic --noinput')

    @task
    def reload_app():
        sudo('systemctl reload uwsgi.service')

    @task
    def upgrade():
        git_pull()
        migrate()
        collect_static()
        reload_app()

and run:

    fab upgrade
