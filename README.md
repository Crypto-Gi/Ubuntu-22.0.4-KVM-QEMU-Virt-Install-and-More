# Ubuntu-22.0.4-KVM-QEMU-Virt-Install-and-More

A comprehensive guide for setting up and managing a virtual machine (VM) environment on Ubuntu 22.04 LTS. It covers the installation of KVM/QEMU, VM management using Virt-Manager, remote management, SMB shares setup, network speed testing, and resolving a common VM flickering issue.

## Table of Contents
1. [Installing KVM/QEMU](#1-installing-kvmqemu)
2. [Multipass Installation](#2-multipass-installation)
3. [Virtual Machine Management with Virt-Manager](#3-virtual-machine-management-with-virt-manager)
4. [Remote VM Management Setup](#4-remote-vm-management-setup)
5. [Permanent SMB Share Mounting](#5-permanent-smb-share-mounting)
6. [Network Speed Testing](#6-network-speed-testing)
7. [Fixing VM Flickering Issue](#7-fixing-vm-flickering-issue)

---

## 1. Installing KVM/QEMU
**KVM/QEMU** is a popular open-source virtualization solution. This section guides you through the installation of KVM along with associated tools and libraries.

```bash
# Install necessary packages for KVM/QEMU
sudo apt -y install bridge-utils cpu-checker libvirt-clients libvirt-daemon qemu qemu-kvm libvirt-daemon-system

# Check if your CPU supports KVM
kvm-ok

# Add your user to the 'libvirt' group
sudo adduser $USER libvirt
```
- The first command installs all necessary packages to set up a virtualization environment including tools for managing network bridges and checking CPU compatibility.
- The `kvm-ok` command checks if KVM can run on your processor.
- Adding your user to the libvirt group allows managing VMs without needing root privileges.

## 2. Multipass Installation
Multipass is a lightweight VM manager that allows you to quickly create and manage VM instances.

```bash
# Install Multipass
sudo snap install multipass
```
This command installs Multipass using the snap package system, enabling you to spin up VMs on demand.

## 3. Virtual Machine Management with Virt-Manager
Virt-Manager provides a graphical interface to manage your virtual machines.

```bash
# Install Virt-Manager and dependencies
sudo apt-get install virt-manager ssh-askpass-gnome --no-install-recommends ssh-askpass

# Open Virt-Manager
virt-manager
```
This command installs Virt-Manager along with ssh-askpass-gnome for SSH passphrase input when necessary.

## 4. Remote VM Management Setup
Manage VMs from a remote system by installing Virt-Manager and configuring SSH.

```bash
# Install Virt-Manager for remote management
sudo apt-get install virt-manager ssh-askpass-gnome --no-install-recommends ssh-askpass

# Generate an SSH key pair for secure access
ssh-keygen

# Copy the public SSH key to the server
ssh-copy-id username@remote_host

# Verify that SSH is working
ssh username@remote_host

# Connect to the server with Virt-Manager
virt-manager -c 'qemu+ssh://myuser@192.168.153.151:22/system?keyfile=id_rsa'
```
This set of commands enables secure remote management of VMs using SSH and Virt-Manager.

## 5. Permanent SMB Share Mounting
Set up a permanent SMB share for network access to shared directories.

```bash
# Install CIFS utilities
sudo apt-get install cifs-utils

# Create a mount point for the SMB share
sudo mkdir /mnt/Mir-Shared

# Mount the SMB share
sudo mount -t cifs -o user=admin //192.168.254.50/Mir-Shared /mnt/Mir-Shared

# Edit fstab for automatic mounting at boot
sudo nano /etc/fstab
# Add the following line to /etc/fstab
//192.168.254.50/Mir-Shared  /mnt/Mir-Shared cifs credentials=/root/.smbpasswd,_netdev,noauto,x-systemd.automount,x-systemd.idle-timeout=30sec 0 0
```
This process involves installing utilities, creating a mount point, mounting the share, and editing fstab for boot-time mounting.

## 6. Network Speed Testing
Test the speed of your network connection using a command-line tool.

```bash
# Install speedtest-cli
sudo apt install speedtest-cli

# Run a speed test
speedtest-cli --secure
```
The `speedtest-cli` tool tests your internet bandwidth, providing insights into download and upload speeds.

## 7. Fixing VM Flickering Issue
Resolve flickering in VM displays by adjusting the GRUB configuration.

```bash
# Edit GRUB configuration file
sudo nano /etc/default/grub

# Modify the GRUB_CMDLINE_LINUX_DEFAULT line to reduce flickering
# Add: i915.enable_psr=0
# Apply changes
sudo update-grub
```
This fix involves editing the GRUB configuration and updating it to prevent screen flickering.
