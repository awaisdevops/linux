# Cron Job to Run Ansible Playbook

This guide demonstrates how to schedule a cron job to run an Ansible playbook that creates a VM on Proxmox.

## Cron Job to Run Ansible Playbook

### Setting up a Cron Job for an Ansible Playbook

To schedule an Ansible playbook to run at a specific date and time, you can add a cron job. Here's an example that runs an Ansible playbook to create a VM on Proxmox at **April 30th, 2025, 3:00 PM**.

### Example Cron Job Entry:
Add the following cron job entry to run the Ansible playbook on the specified date and time:

```bash
0 15 30 4 * ansible-playbook /path/to/your/playbook/create_vm.yml
```

#### Explanation:
- `0`: Minute (0th minute)
- `15`: Hour (3 PM in 24-hour format)
- `30`: Day of the month (30th)
- `4`: Month (April)
- `*`: Day of the week (any day)
- `ansible-playbook /path/to/your/playbook/create_vm.yml`: The command to run the Ansible playbook.

### Steps to Add the Cron Job

1. Open the crontab for editing:

   ```bash
   crontab -e
   ```

2. Add the cron job entry:

   ```bash
   0 15 30 4 * ansible-playbook /path/to/your/playbook/create_vm.yml
   ```

3. Save and close the file.

4. Verify the cron job:

   ```bash
   crontab -l
   ```

## Ansible Playbook: Create VM on Proxmox

The following is an example of an Ansible playbook (`create_vm.yml`) that creates a VM on Proxmox and starts it:

### `create_vm.yml` - Ansible Playbook

```yaml
---
- name: Create a VM on Proxmox and Start it
  hosts: localhost  # Where this playbook is executed; generally your control machine
  gather_facts: no   # Don't gather system facts; speeds up execution as we define everything

  vars:
    proxmox_host: "192.168.1.100"  # Change to your Proxmox server IP or hostname
    proxmox_user: "root@pam"       # Proxmox username (e.g., root@pam, user@pve)
    proxmox_password: "your_password"  # The Proxmox user's password
    proxmox_node: "pve"            # The name of the Proxmox node where the VM will reside
    proxmox_vmid: 105               # Unique VM ID (integer).  Must be unique on the Proxmox node.
    vm_name: "my-new-vm"          # The name of the VM in the Proxmox UI
    vm_memory: 2048               # VM memory in MB
    vm_cores: 2                  # Number of CPU cores
    disk_size: "20G"             # Disk size string ("G" for GB, "T" for TB)
    storage_pool: "local-lvm"      # Proxmox storage pool where the disk will be created
    network_bridge: "vmbr0"        # The network bridge the VM will connect to (e.g., vmbr0)
    os_template: "local:vztmpl/ubuntu-22.04-standard_22.04-1_amd64.tar.gz"  # Template to clone

  tasks:
    - name: Check if VM already exists
      uri:
        url: "https://{{ proxmox_host }}:8006/api2/json/nodes/{{ proxmox_node }}/qemu/{{ proxmox_vmid }}/config"
        method: GET
        user: "{{ proxmox_user }}"
        password: "{{ proxmox_password }}"
        validate_certs: no  # Disable SSL verification (use only if you have self-signed certs, NOT recommended for production)
      register: vm_check_result
      ignore_errors: yes

    - name: Create the VM if it doesn't exist
      block:
        - name: Create VM
          uri:
            url: "https://{{ proxmox_host }}:8006/api2/json/nodes/{{ proxmox_node }}/qemu"
            method: POST
            user: "{{ proxmox_user }}"
            password: "{{ proxmox_password }}"
            validate_certs: no  # Disable SSL verification (use only if you have self-signed certs, NOT recommended for production)
            body_format: form-urlencoded
            body:
              vmid: "{{ proxmox_vmid }}"
              name: "{{ vm_name }}"
              memory: "{{ vm_memory }}"
              cores: "{{ vm_cores }}"
              net0: "virtio,bridge={{ network_bridge }}"
              ostemplate: "{{ os_template }}"
              storage: "{{ storage_pool }}"
              disk: "{{ disk_size }}"
              newnet0: "virtio,bridge={{ network_bridge }}"
            status_code: 200
          register: create_result

        - name: Clone VM from template
          uri:
            url: "https://{{ proxmox_host }}:8006/api2/json/nodes/{{ proxmox_node }}/qemu/{{ proxmox_vmid }}/clone"
            method: POST
            user: "{{ proxmox_user }}"
            password: "{{ proxmox_password }}"
            validate_certs: no
            body_format: form-urlencoded
            body:
              newid: "{{ proxmox_vmid }}"
              name: "{{ vm_name }}"
              storage: "{{ storage_pool }}"
          register: clone_result

        - name: Resize disk
          uri:
            url: "https://{{ proxmox_host }}:8006/api2/json/nodes/{{ proxmox_node }}/qemu/{{ proxmox_vmid }}/resize"
            method: PUT
            user: "{{ proxmox_user }}"
            password: "{{ proxmox_password }}"
            validate_certs: no
            body_format: form-urlencoded
            body:
              disk: "scsi0"
              size: "{{ disk_size }}"
          register: resize_result

        - name: Start VM
          uri:
            url: "https://{{ proxmox_host }}:8006/api2/json/nodes/{{ proxmox_node }}/qemu/{{ proxmox_vmid }}/status/start"
            method: POST
            user: "{{ proxmox_user }}"
            password: "{{ proxmox_password }}"
            validate_certs: no  # Disable SSL verification (use only if you have self-signed certs, NOT recommended for production)
          register: start_result
      when: vm_check_result.status is defined and vm_check_result.status == 400

    - name: Print create result
      debug:
        var: create_result
      when: vm_check_result.status is defined and vm_check_result.status == 400

    - name: Print clone result
      debug:
        var: clone_result
      when: vm_check_result.status is defined and vm_check_result.status == 400

    - name: Print resize result
      debug:
        var: resize_result
      when: vm_check_result.status is defined and vm_check_result.status == 400

    - name: Print start result
      debug:
        var: start_result
      when: vm_check_result.status is defined and vm_check_result.status == 400
```

### Notes:
- Replace `/path/to/your/playbook/create_vm.yml` in the cron job with the actual path where you store the playbook on your machine.
- Ensure that Ansible is installed and properly configured on the machine where the cron job is set to run.
  
### Verify the Cron Job
You can check if the cron job is correctly added by running:

```bash
crontab -l
```

This will list all active cron jobs for your user.
