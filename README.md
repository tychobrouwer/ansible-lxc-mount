Configures mounts on LXC containers and the host
=========

The role configures mounts on LXC containers and the host. It creates the mount points, mounts the folders and sets the permissions.

The role should be run on the host and on the LXC containers with the ```role_is_lxc``` variable set to false or true respectively.

Role Variables
--------------

The ```lxc_mount_is_lxc``` should be set to true if the role is being run on an LXC container and should be false when run on the (Proxmox) host.

The ```lxc_mount_mounts``` should be set to a list of dictionaries containing the ```src``` (mount point on the host) and the ```dest``` (mount point on the container).

```lxc_mount_lxc_id``` should be set to the ID of the container which the mount should be mounted on.

The ```lxc_mount_users``` list should contain the users that should be be added to the mount group (and thus have permissions to access the mount).

```lxc_mount_user_uid``` and ```lxc_mount_user_gid``` can be set to the UID and GID of the user that should own the mount points.

```lxc_mount_user_name``` and ```lxc_mount_user_group``` can be set to the name of the user and group that should own the mount points.

Example Playbook
----------------

```yaml
    - hosts: host
      vars:
        lxc_mount_mounts:
          - { src: /media/file-share, dest: /share/file-share }
          - { src: /rpool-main/media-share, dest: /share/media-share }
        lxc_mount_users: [ "backup" ]

      roles:
         - { role: lxc_mount, lxc_mount_is_lxc: false, lxc_mount_mounts: "{{ lxc_mounts }}", lxc_mount_lxc_id: 104 }

    - hosts: lxc
      vars:
        lxc_mount_mounts:
          - { src: /media/file-share, dest: /share/file-share }
          - { src: /rpool-main/media-share, dest: /share/media-share }
        lxc_mount_users: [ "sambauser" ]

      roles:
         - { role: lxc_mount, lxc_mount_is_lxc: true, lxc_mount_mounts: "{{ lxc_mounts }}", lxc_mount_lxc_id: 104 }
```

License
-------

BSD

Author Information
------------------

An optional section for the role authors to include contact information, or a website (HTML is not allowed).
