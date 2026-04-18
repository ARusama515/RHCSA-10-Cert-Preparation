### Stage 1: Introduction to RHCSA EX200 (RHEL 10) + Vagrant Lab Environment Setup

This is **Stage 1** of my RHCSA EX200 preparation.  
This stage builds the foundation. I will understand what the RHCSA exam is, what to expect, and most importantly, set up my hands-on practice environment using Vagrant.

### 1. RHCSA EX200 Exam Overview (RHEL 10)

- **Exam Code**: EX200  
- **Version**: Based on **Red Hat Enterprise Linux 10**  
- **Duration**: 2.5 hours (150 minutes)  
- **Format**: 100% Performance-based (real tasks only, no MCQs)  
- **Passing Score**: 210 out of 300 (70%)  

**Main Topics Covered** (according to official objectives):
- Essential tools (files, directories, commands, vim, etc.)
- Software management (dnf, rpm, modules, Flatpak)
- Shell scripting (simple scripts)
- Operating running systems (processes, systemd)
- Boot process and troubleshooting
- Storage (partitions, LVM, file systems including Stratis and XFS)
- Users & Groups + sudo
- Networking (nmcli)
- Security (firewalld + SELinux)
- Logging, scheduling (cron & systemd timers), etc.

In the real exam, I will get a real RHEL 10-like system and must solve tasks. That’s why I am setting up the same environment for practice.

**Important Exam Strategy** (Remember from Stage 1):
- Time management is critical (10–15 tasks in 150 minutes).
- Always verify your work after each task (e.g., `systemctl status`, `mount | grep`, `df -h`).
- Strong troubleshooting skills are necessary because you cannot reset the system during the exam.

### 2. Vagrant Lab Environment Setup

I will create a multi-VM environment so I can practice real-world and exam-like scenarios (one control machine + managed nodes).

**Recommended Tools** (Install on my PC):
1. **VirtualBox** (latest version) + Extension Pack  
2. **Vagrant** (latest version from vagrantup.com)

**Step-by-step Setup:**

**Step 1: Create Directory**
```bash
mkdir ~/rhcsa-lab
cd ~/rhcsa-lab
```

**Step 2: Create Vagrantfile**

Create a file named `Vagrantfile` and paste the following content:

```ruby
Vagrant.configure("2") do |config|
  # Base box - Rocky Linux 10 (free RHEL 10 clone, perfect for practice)
  config.vm.box = "rockylinux/10"

  # Common settings
  config.vm.provider "virtualbox" do |vb|
    vb.memory = "2048"    # 2 GB RAM
    vb.cpus = 2
    vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
  end

  # Server1 - Main practice machine
  config.vm.define "server1" do |server1|
    server1.vm.hostname = "server1.example.com"
    server1.vm.network "private_network", ip: "192.168.56.101"
    server1.vm.provision "shell", inline: <<-SHELL
      sudo dnf update -y
      sudo dnf install -y vim net-tools
      echo "server1 is ready for RHCSA practice"
    SHELL
  end

  # Server2 - For networking and remote management practice
  config.vm.define "server2" do |server2|
    server2.vm.hostname = "server2.example.com"
    server2.vm.network "private_network", ip: "192.168.56.102"
    server2.vm.provision "shell", inline: <<-SHELL
      sudo dnf update -y
      echo "server2 is ready"
    SHELL
  end
end
```

**Step 3: Start the VMs**
```bash
vagrant up
```

It may take some time on the first run (downloading ~1-2 GB box).

Check status:
```bash
vagrant status
```

**How to Access VMs:**
- Login to server1: `vagrant ssh server1`
- Login to server2: `vagrant ssh server2`

### 3. Basic Commands Test

After logging into **server1**, run these commands and observe the output:

```bash
hostname
cat /etc/os-release
uname -r
df -h
free -h
ip addr show
whoami
sudo whoami
```

### Self-Assessment (Complete these before moving forward)

1. Did I successfully start 2 VMs using Vagrant?  
2. Can I login using `vagrant ssh server1`?  
3. Does `cat /etc/os-release` show Rocky Linux 10 or similar RHEL 10 version?  
4. Am I comfortable with the basic commands?
