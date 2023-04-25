Role Name
=========

Automatic provisioning of any number vms in kvm

Requirements
------------

create pwd.yml files vaulted

Role Variables
--------------

defaults/main.yml 
base_image_name:  NAME of the golden image created
base_image_url: Path To the golden image
base_image_sha: 
libvirt_pool_dir: pool dir
vm_name: specified in kvm_provision.yml 
vm_vcpus: 
vm_ram_mb: in MB
vm_net: default
vm_root_pass: "{{ lookup('file','YOUR ENCRYPTED FILE') }}"
vm_user: CREATE USER
vm_user_pass: "{{ lookup('file','YOUR OTHER ENCRYPTED FILE') }}"
cleanup_tmp: no
ssh_key: ssh key to import

kvm_provision.yaml
 pool_dir: "libvirt pool"
    vcpus: 2
    ram_mb: 2048
    cleanup: yes
    net: default
    ssh_pub_key:key to add
loop: (USED for vm names/hostnames)
        - vm1
        - vm2
vars:
        libvirt_pool_dir: "{{ pool_dir }}"
        vm_name: "{{ item }}"         vm1,vm2 ....
        vm_vcpus: "{{ vcpus }}"
        vm_ram_mb: "{{ ram_mb }}"
        vm_net: "{{ net }}"
        cleanup_tmp: "{{ cleanup }}"
        ssh_key: "{{ ssh_pub_key }}"


Example Playbook
----------------

---
- name: Golden image
  hosts: localhost
  gather_facts: yes
  become: yes
  vars:
    pool_dir: "/home/mitre/vm"
    vcpus: 2
    ram_mb: 2048
    cleanup: yes
    net: default
    ssh_pub_key: "/home/mitre/.ssh/id_rsa.pub"

  tasks:
    - name: KVM Provision role
      loop:
        - tst1
        - tst2
      include_role:
        name: kvm_provision
      vars:
        libvirt_pool_dir: "{{ pool_dir }}"
        vm_name: "{{ item }}"
        vm_vcpus: "{{ vcpus }}"
        vm_ram_mb: "{{ ram_mb }}"
        vm_net: "{{ net }}"
        cleanup_tmp: "{{ cleanup }}"
        ssh_key: "{{ ssh_pub_key }}"

    

License
-------

BSD


