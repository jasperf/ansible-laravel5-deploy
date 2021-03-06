# Laravel 5 Deploy

Based on [Blacklight](http://www.blacklight.co.za) and [Cubesytem](https://github.com/cubesystems/ansible-laravel5-deploy)'s packages.

Deployment role for Laravel 5 apps. Deployment can be done via Git, SVN, Mercurial, and Rsync. It tries to imitate a similar
structure to what you would see with other deployment tools such as [Capistrano](http://capistranorb.com/). 

## Structure 

The final directory structure will look something like this:

```
project
    releases
        // All releases will be going here
    shared
        storage
            logs
            framework
                sessions
        public
            uploads
    current // This will be a symlink to the latest release
```

The role also has error checking in place. If any of the steps fail the role will delete the newly created release folder
and stop execution. If the deploy was successful the role will remove old releases.


## Requirements

The only requirement is Ansbile >= 2.5.

## Installation

```
ansible-galaxy install jasperf.laravel5deploy
```

or 

```
ansible-galaxy install jasperf.laravel5deploy --roles-path ~/path/to/project/
```

## Role Variables

- **laravel_root_dir** (Required) - The root directory of the project.

- **laravel_repo** (Required) - The URL to the repo containing the application code.

- **laravel_branch** (Defaults: master)- The branch that you would like to deploy.

- **laravel_strategy** (Defaults: git)- The deployment strategy to use. Available options: git, svb, mercurial, rsync, archive

- **laravel_local_root** (Defaults: /) This option is only used when deploying via Rsync. It defines the path to the local folder to upload to the server.

**NB** The path is relative to your playbook file.

- **laravel_composer_path** (Defaults: false) -The path to an existing composer installation. If set to false, the role will automatically download composer into the
projects root directory.

- **laravel_composer_options** (Defaults: --no-dev --no-interaction --optimize-autoloader) -Flags to add to the composer install command.

- **laravel_php_path** (Defaults: /usr/bin/php) -The path to PHP. This is only used when the role has to download composer.

- **laravel_releases** (Defaults: 5) The amount of releases to keep in the releases directory.

## Example Playbook

```yml
---
- hosts: web
  vars:
    laravel_root_dir: /var/www/example
    laravel_repo: git@github.com/example/example-project.git
    laravel_composer_options: '--no-dev --optimize-autoloader --no-interaction'
    ansible_ssh_user: root

  roles:
    - jasperf.laravel5deploy
```

## License


MIT
