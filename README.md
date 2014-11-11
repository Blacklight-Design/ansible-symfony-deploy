# Symfony Deploy

Brought to you by [Blacklight](http://www.blacklight.co.za).

Deployment role for Symfony 2 apps. Deployment can be done via Git, SVN, Mercurial, and Rsync. Tries to imitate a similar
structure to what you would see with other deployment tools such as [Capistrano](http://capistranorb.com/). The final
directory structure will look something like this:

```
project
    releases
        // All releases will be going here
    shared
        app
            config
                parameters.yml
            logs
        web
            uploads
    current // This will be a symlink to the latest release
```

The role also has error checking in place. If any of the steps fail the role will delete the newly created release folder
and stop execution. If the deploy was successful the role will remove old releases.

Other Deployment Roles
----------------------

Laravel4: [laravel4-deploy](https://galaxy.ansible.com/list#/roles/2146)

Laravel5: [laravel5-deploy](https://galaxy.ansible.com/list#/roles/2145)

Requirements
------------

The only requirement is Ansbile >= 1.2.

Installation
------------

```
ansible-galaxy install blacklight.symfony-deploy
```

Role Variables
--------------

### symfony_root_dir (Required)

The root directory of the project.

### symfony_repo (Required)

The URL to the repo containing the application code.

### symfony_branch (Defaults: master)

The branch that you would like to deploy.

### symfony_strategy (Defaults: git)

The deployment strategy to use. Available options: git, svb, mercurial, rsync

### symfony_local_root (Defaults: /)

This option is only used when deploying via Rsync. It defines the path to the local folder to upload to the server.

*Important Note!* The path is relative to your playbook file.

### symfony_env (Defaults: prod)

Sets the Symfony environment. During the process this is used in composer commands as follows:

```bash
SYMFONY_ENV=prod composer...
```

### symfony_composer_path (Defaults: false)

The path to an existing composer installation. If set to false, the role will automatically download composer into the
projects root directory.

### symfony_composer_options (Defaults: --no-dev --no-interaction --optimize-autoloader)

Flags to add to the composer install command.

### symfony_asset_options: (Defaults: '')

Flags to add to the assetic:dump command.

### symfony_php_path (Defaults: /usr/bin/php)

The path to PHP. This is only used when the role has to download composer.

### symfony_releases (Defaults: 5)

The amount of releases to keep in the releases directory.

Example Playbook
----------------

```yml
---
- hosts: web
  vars:
    symfony_root_dir: /var/www/example
    symfony_repo: git@github.com/example/example-project.git
    symfony_composer_options: '--no-dev --optimize-autoloader --no-interaction'
    ansible_ssh_user: root

  roles:
    - blacklight.symfony-deploy
```

License
-------

MIT
