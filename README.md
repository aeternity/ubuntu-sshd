# ubuntu-sshd

Dockerized SSH service, built on top of [official Ubuntu](https://registry.hub.docker.com/_/ubuntu/) images.

## Image tags

- aeternity/ubuntu-sshd:18.04 (bionic)
- aeternity/ubuntu-sshd:22.04 (jammy)

## Installed packages

Base:

- [Bionic (18.04) minimal](http://packages.ubuntu.com/bionic/ubuntu-minimal)
- [Jammy (22.04) minimal](http://packages.ubuntu.com/jammy/ubuntu-minimal)

Image specific:
- [openssh-server](https://help.ubuntu.com/community/SSH/OpenSSH/Configuring)

Config:

  - `PermitRootLogin yes`
  - `UsePAM no`
  - exposed port 22
  - default command: `/usr/sbin/sshd -D`
  - root password: `root`

## Run example

```bash
$ docker run -d -p 2222:22 --name test_sshd aeternity/ubuntu-sshd:18.04
$ ssh root@localhost -p 2222
# The password is `root`
root@test_sshd $
```

## Security

If you are making the container accessible from the internet you'll probably want to secure it bit.
You can do one of the following two things after launching the container:

- Change the root password: `docker exec -ti test_sshd passwd`
- Don't allow passwords at all, use keys instead:

```bash
$ docker exec test_sshd passwd -d root
$ docker cp file_on_host_with_allowed_public_keys test_sshd:/root/.ssh/authorized_keys
$ docker exec test_sshd chown root:root /root/.ssh/authorized_keys
```
