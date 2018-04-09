---
- hosts: all
  become: yes # This means that all tasks will be executed with sudo
  tasks:
   - name: Update all packages to the latest version
     apt:
      upgrade: dist
     when:
      - ansible_os_family == "Debian"
      - ansible_distribution == "Ubuntu"
   - name: Run the equivalent of "apt-get update" as a separate step
     apt:
        update_cache: yes
     when:
       - ansible_distribution == "Ubuntu"
       - ansible_os_family == "Debian"
   - name: Set repos http://neuro.debian.net/lists/xenial.us-ca.full
     get_url:
        url: http://neuro.debian.net/lists/xenial.us-ca.full
        dest: /etc/apt/sources.list.d/neurodebian.sources.list
        mode: 644
     when:
       - ansible_distribution == "Ubuntu"
       - ansible_os_family == "Debian"
   - name: Receive repo keys hkp://pool.sks-keyservers.net:80
     command: apt-key adv --recv-keys --keyserver hkp://pool.sks-keyservers.net:80 0xA5D32F012649A5A9
     when:
       - ansible_distribution == "Ubuntu"
       - ansible_os_family == "Debian"
   - name: Check if key exists
     command: apt-key fingerprint 0xA5D32F012649A5A9
     when:
      - ansible_distribution == "Ubuntu"
      - ansible_os_family == "Debian"
   - name: Run the equivalent of "apt-get update" as a separate step
     apt:
        update_cache: yes
     when:
       - ansible_distribution == "Ubuntu"
       - ansible_os_family == "Debian"

   - name: Update repositories cache and install `singularity` package
     apt: name={{item}} state=latest
     update_cache: yes
     with_items:
       - singularity-container
       - debootstrap
       - squashfs-tools
     when:
       - ansible_distribution == "Ubuntu"
       - ansible_os_family == "Debian"

   - name: Run some self tests for singularity install
     command: singularity selftest
     when:
       - ansible_distribution == "Ubuntu"
       - ansible_os_family == "Debian"  
   - name: Build container from centos recipe file
     command: singularity build --sandbox /tmp/ubuntusingular.simg /vagrant/definitions/ubuntu.def
     when:     
       - ansible_distribution == "Ubuntu"
       - ansible_os_family == "Debian"
