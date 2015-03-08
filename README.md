Foorge Runner
=============

Ansible role responsible for activating the "run"-stage of the Build, Release, Run
lifecycle of applications.

It uses ansible-encoded
[Procfile](https://devcenter.heroku.com/articles/procfile) information and
registers all the web, worker and cron processes on the application servers.

This role can be used to re-prioritize workers and jobs during the lifecycle of
a release, for example increasing the number of workers.

The ansible groups ``web`` and ``worker`` can be used to specialize the
deployment instead of using a server grouped with ``app`` for both web and
worker.

Minimal Example
---------------

This example shows directly embedding the processes into the runner, it makes
sense to load them from a dedicated ``Procfile.yml`` and include them using
``vars_files``.

    ---
    - hosts: app:&foo
      roles:
        - name: "foorge.runner"
          runner_procfile:
            - { name: 'app1', type: 'fastcgi', document_root: 'components/app1/src/htdocs' }
            - { name: 'app2', type: 'fastcgi', document_root: 'components/app2/web', index: 'app.php' }
            - { name: 'webpack', type: 'web', command: 'webpack-dev-server -p $PORT', dir: 'components/app1', development: true }
            - { name: 'job1', type: 'worker', command: 'php src/components/app1/src/bin/workerJob' }
            - { name: 'job2', type: 'cron', command: 'php src/components/app1/src/bin/cron', minute: '*/10' }

License
-------

MIT
