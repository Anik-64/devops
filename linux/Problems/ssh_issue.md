# 🔐 SSH Issue with VM After Network Change

## 📄 Problem Description

I set up a virtual machine (VM) using **VMware** with the **Ubuntu 24.04.1 LTS** ISO file. The VM was initially configured with a static IP address of `192.168.40.128` to avoid conflicts within my router's DHCP range of `192.168.0.0/24`.

I enabled **passwordless SSH** from my Windows machine to the VM using the following command:

```bash
ssh-keygen -t rsa
```

This generated the following files in the `.ssh` directory on my Windows machine:

- `id_rsa` (private key)
- `id_rsa.pub` (public key)
- `known_hosts`
- `known_hosts.old`

With this setup, I could SSH into the VM without entering a password.

Later, I decided to create a **Kubernetes (k8s) cluster** on the VM. Since k8s uses the `192.168.0.0/24` CIDR range by default—which conflicted with my router—I reconfigured my router’s CIDR to `172.17.17.0/24`.

I also updated the VM’s static IP to `172.17.17.50` using **Netplan**. The network update was successful, and the VM was reachable at the new IP internally.

However, I could no longer SSH into the VM from my Windows machine, even though the IP address and network settings appeared to be correct.

---

## 🕵️ Root Cause

After hours of debugging, I discovered that the issue was caused by the `known_hosts` and `known_hosts.old` files in the `.ssh` directory on my Windows machine.

These files still referenced the **old IP address** `192.168.40.128`. Since the `known_hosts` file stores the IP address and public key fingerprint for verification, the mismatch between the stored key and the new IP (`172.17.17.50`) led to the SSH failure.

---

## ✅ Solution

### 1. Locate the `.ssh` Directory

```bash
C:\Users\<YourUsername>\.ssh
```

### 2. Backup the `known_hosts` File

```bash
copy known_hosts known_hosts.bak
```

### 3. Edit or Remove the Old IP Entry

- Open the `known_hosts` file in a text editor
- **Option A**: Replace `192.168.40.128` with `172.17.17.50`
- **Option B**: Delete the line containing the old IP

### 4. Reconnect via SSH

```bash
ssh <username>@172.17.17.50
```

- You will be prompted to accept the new host key.
- The new IP will then be added to the `known_hosts` file.

### 5. Verify the SSH Connection

```bash
ssh <username>@172.17.17.50
```

You should now be able to connect **without entering a password**.

### 6. Clean Up Old Files (Optional)

If no longer needed:

```bash
del known_hosts.old
```

---

## 🛡️ Prevention Tips

- ✅ **Check `known_hosts` After IP Changes**: Always inspect or clear `known_hosts` entries after network reconfiguration.
- ⚙️ **Automate Host Key Updates**: Use `ssh-keyscan` to add or refresh host keys automatically:

```bash
ssh-keyscan -H 172.17.17.50 >> ~/.ssh/known_hosts
```

- 📝 **Document Network Changes**: Keep a log of CIDR ranges, IP addresses, and changes.
- 🌐 **Use Hostnames**: Instead of static IPs, configure DNS or `/etc/hosts` to map a name like `myvm.local` to your VM’s IP.

---

## 🧾 Conclusion

The SSH failure was caused by an outdated entry in the `known_hosts` file after the VM’s network configuration was changed. By updating or removing the old entry and allowing SSH to add the new one, I was able to restore passwordless SSH access.

This document serves as a future reference for resolving similar SSH issues quickly and efficiently.
