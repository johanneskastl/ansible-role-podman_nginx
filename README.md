![Ansible Lint](https://github.com/johanneskastl/ansible-role-podman_nginx/workflows/Ansible%20Lint/badge.svg)

# podman_nginx

Run a simple nginx container with Podman. You can either run it rootless (aka
without root privileges) as a systemd user service or rootful as a systemd
system service.

## Requirements

None.

## Role Variables

### Role behaviour

To decide whether to create a rootless or rootful container, set the following
variable to `true` (rootful) or `false` (rootless). The default is to use a
rootless container.

- `podman_nginx_rootful_container`: Boolean to enable a rootful container
  (Default: `false`)

### Tweaking the behaviour

In case you want to have the Nginx container listen on a different port, you can
set the desired port for rootful or rootless containers using the following
variables. Please be aware that rootless containers cannot listen on ports <
1024 by default due to security reasons. This could be changed, but that is not
scope of this role.

- `podman_nginx_rootful_container_port`: (Integer) Port used for a rootful
  container, defaults to `80`
- `podman_nginx_rootless_container_port`: (Integer) Port used for a rootless
  container, defaults to `8080`

The role creates a directory for the nginx files and mounts it into the
container. You can change the location of this directory as well as its owner
and group:

- `podman_nginx_data_directory`: (String) Directory mounted into the container,
  defaults to `/opt/nginx_Podman`
- `podman_nginx_default_user`: (String) Owner of the directory (default: `root`)
- `podman_nginx_default_group`: (String) Group of the directory (default:
  `root`)

This role creates a container using the official Nginx image from Dockerhub. If
you want to change this, feel free to change the variable `podman_nginx_image`
to point to another image and tag.

- `podman_nginx_image`: (String) Image to be used for the container, default:
  `docker.io/library/nginx:stable`

## Dependencies

None

## Example Playbook for a rootless container

```yaml
- hosts: server
  become: false
  roles:
    - role: 'johanneskastl.podman_nginx'
      podman_nginx_data_directory: '/home/tux/nginx_Podman'
      podman_nginx_default_user: 'tux'
      podman_nginx_default_group: 'tux'
```

After the role is finished, place your HTML files in `/home/tux/nginx_Podman`.

## Example Playbook for a rootful container

```yaml
- hosts: server
  roles:
    - role: 'johanneskastl.podman_nginx'
      podman_nginx_rootful_container: true
```

After the role is finished, place your HTML files in `/opt/nginx_Podman`
(requires root permissions).

## License

BSD-3-Clause

## Author Information

I am Johannes Kastl, reachable via kastl@b1-systems.de.
