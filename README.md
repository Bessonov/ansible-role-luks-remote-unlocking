luks-remote-unlocking
=====================

Prepare encrypted system to be unlocked remotely.

Highly inspired by and credits to:

- https://hamy.io/post/0009/how-to-install-luks-encrypted-ubuntu-18.04.x-server-and-enable-remote-unlocking/
- https://github.com/martin-v/ansible-sshpreluks

There are multiple ways to unlock. For example:

```
ssh root@your.server -p 2244
cryptroot-unlock
```

And then type your passphrase. Or:
```
ssh root@your.server -p 2244 'plymouth --quit; echo -ne "your LUKS passphrase" > /lib/cryptsetup/passfifo'
```

`plymouth --quit` is needed because of not fixed bug: https://askubuntu.com/questions/66785/how-can-one-unlock-a-fully-encrypted-ubuntu-11-10-system-over-ssh-at-boot

Requirements
------------

System encrypted with LUKS. Tested only on Ubuntu 18.04.

Role Variables
--------------

See `defaults/main.yml`

Dependencies
------------

No dependencies.

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: servers
      roles:
        - role: Bessonov.ansible-role-luks-remote-unlocking
          become: yes

          # optional, eth0 is used by default
          luks_remote_bind_to_network_device: enp0s8

          # insert your keys
          luks_remote_public_keys:
            # use file
            - "{{ lookup('file', 'your/key.pub') }}"
            # or inline
            - ssh-rsa AAAAQ [...]

License
-------

MIT License

Copyright (c) 2019 Anton Bessonov

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
