---
title: Compile Python from source with Debian flags using Ansible
categories: linux
tags:
    - debian
    - linux
    - install
    - python

date: 2019-11-30
---

I created an Ansible playbook to fetch Python from git, install all dependencies in Debian and compile it with the configure options used by Debian. I did that to rely less and less on external tools, like Pyenv, and to make my environments as independent as possible. The code is on [this link](https://github.com/ispmarin/python_deploy).

The playbook is below:

```yaml
---

- name: Install Debian package requirements
  become: yes
  become_user: root
  apt:
    update_cache: yes
    state: latest
    pkg:
      - zlib1g-dev 
      - libffi-dev
      - libssl-dev
      - libbz2-dev
      - libncursesw5-dev 
      - libgdbm-dev 
      - liblzma-dev 
      - libsqlite3-dev 
      - tk-dev 
      - uuid-dev 
      - libreadline-dev
      - libgdbm-compat-dev
      - libsqlite3-dev
      - libffi-dev

- name: Clone Python git repository
  git:
    repo: "git@github.com:python/cpython.git"
    dest: "{{ tmp_folder }}"
    version: "{{ python_version }}"

- name: Configure Python
  shell: "{{ item }}"
  args:
    chdir: "{{ tmp_folder }}"
  with_items:
    - ./configure --enable-shared --enable-ipv6 --enable-loadable-sqlite-extensions --with-computed-gotos --with-system-ffi --with-system-libmpdec CFLAGS='-fstack-protector-strong -Wformat -Werror=format-security' LDFLAGS='-Wl,-z,relro,-rpath="{{ dest_folder }}"/lib' CPPFLAGS='-Wdate-time -D_FORTIFY_SOURCE=2' --prefix="{{ dest_folder }}"

- name: Build target with extra arguments
  make:
    chdir: "{{ tmp_folder }}"
    params:
      NUM_THREADS: 4

- name: Run 'install' target
  make:
    chdir: "{{ tmp_folder }}"
    target: install
```

Just clone this playbook from my [github repository](https://github.com/ispmarin/python_deploy) and run it with

```bash
ansible-playbook -i hosts python-deploy.yml
```

from a place where Ansible is installed. 