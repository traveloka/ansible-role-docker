# Docker role #
Install docker daemon

there is some convention when using this role
```
/var/container_state (can be configured) contain state for every container,
host can mount this directory from separate device
how to use it:
create folder /var/container_state/[container_name]/[volume_name]
e.g.
- Running jenkins container with jenkins_home volume
  docker run --name jenkins -v /var/container_state/jenkins/jenkins_home:/var/jenkins_home jenkinsci/jenkins
- Running static_web container with config and content volume
  docker run --name static_web -v /var/container_state/static_web/config:/etc/nginx -v /var/container_state/static_web/content:/srv/www/html nginx
```


## Requirements ##
```
- apt-get like package manager (debian, ubuntu, mint, etc)
- overlay2 storage driver need kernel >= 4.0
- overlay2 storave driver need docker_version >= 1.12.0
- journald log driver need journald to be started before
```

## Role Variables ##
```
defaults:
  - name: docker_version
    desc: docker daemon version to be installed
    value: latest

  - name: docker_storage_driver
    desc: Storage driver name for docker daemon
    value: overlay2

  - name: docker_log_driver
    desc: Storage driver name for docker daemon
    value: journald

  - name: docker_skip_restart
    desc: when configuration changes detected, should it restart the docker?
    value: false

  - name: docker_stateful_device
    desc: which device is to be mounted to {{ docker_container_state_path }}
    value: ''

vars:
  - name: docker_container_state_path
    desc: path for persistent storage for container
    value: /var/container_state
```

## Dependencies ##
