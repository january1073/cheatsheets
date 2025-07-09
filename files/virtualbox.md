# VirtualBox Cheatsheet

## VM Management

| Command | Description |
|---------|-------------|
| `VBoxManage list vms` | List all registered VMs |
| `VBoxManage list runningvms` | List currently running VMs |
| `VBoxManage startvm <vm_name> --type headless` | Start VM in headless mode |
| `VBoxManage startvm <vm_name> --type gui` | Start VM with GUI |
| `VBoxManage controlvm <vm_name> pause` | Pause a running VM |
| `VBoxManage controlvm <vm_name> resume` | Resume a paused VM |
| `VBoxManage controlvm <vm_name> poweroff` | Force power off a VM |
| `VBoxManage controlvm <vm_name> acpipowerbutton` | Send ACPI power button event |
| `VBoxManage controlvm <vm_name> reset` | Reset a VM (hard reboot) |
| `VBoxManage controlvm <vm_name> savestate` | Save VM state and stop |
| `VBoxManage unregistervm <vm_name> --delete` | Delete a VM and all files |

## VM Configuration

| Command | Description |
|---------|-------------|
| `VBoxManage createvm --name <vm_name> --register` | Create new VM |
| `VBoxManage modifyvm <vm_name> --memory <MB>` | Set VM memory size |
| `VBoxManage modifyvm <vm_name> --cpus <N>` | Set number of CPUs |
| `VBoxManage modifyvm <vm_name> --nic1 nat` | Configure network adapter (NAT) |
| `VBoxManage modifyvm <vm_name> --nic1 bridged --bridgeadapter1 eth0` | Configure bridged networking |
| `VBoxManage modifyvm <vm_name> --vrde on` | Enable Remote Desktop |
| `VBoxManage modifyvm <vm_name> --vrdeport <port>` | Set RDP port |
| `VBoxManage modifyvm <vm_name> --clipboard bidirectional` | Enable shared clipboard |
| `VBoxManage modifyvm <vm_name> --draganddrop bidirectional` | Enable drag and drop |

## Storage Management

| Command | Description |
|---------|-------------|
| `VBoxManage list hdds` | List all virtual disks |
| `VBoxManage createhd --filename <file.vdi> --size <MB>` | Create new virtual disk |
| `VBoxManage storagectl <vm_name> --name "SATA" --add sata` | Add SATA controller |
| `VBoxManage storageattach <vm_name> --storagectl "SATA" --port 0 --device 0 --type hdd --medium <disk.vdi>` | Attach disk to VM |
| `VBoxManage clonehd <source.vdi> <dest.vdi>` | Clone a virtual disk |
| `VBoxManage modifymedium disk <file.vdi> --resize <MB>` | Resize a virtual disk |
| `VBoxManage openmedium disk <file.vdi>` | Register an existing disk |
| `VBoxManage closemedium disk <file.vdi>` | Unregister a disk |

## Snapshots

| Command | Description |
|---------|-------------|
| `VBoxManage snapshot <vm_name> take <snap_name>` | Create snapshot |
| `VBoxManage snapshot <vm_name> list` | List snapshots |
| `VBoxManage snapshot <vm_name> restore <snap_name>` | Restore snapshot |
| `VBoxManage snapshot <vm_name> restorecurrent` | Restore current state |
| `VBoxManage snapshot <vm_name> delete <snap_name>` | Delete snapshot |

## Networking

| Command | Description |
|---------|-------------|
| `VBoxManage list hostonlyifs` | List host-only networks |
| `VBoxManage hostonlyif create` | Create new host-only interface |
| `VBoxManage hostonlyif remove <if_name>` | Remove host-only interface |
| `VBoxManage dhcpserver add --netname <net_name> --ip <ip> --netmask <mask> --lowerip <start> --upperip <end>` | Configure DHCP server |
| `VBoxManage natnetwork add --netname <net_name> --network <network> --enable` | Create NAT network |
| `VBoxManage modifyvm <vm_name> --natpf1 "guestssh,tcp,,2222,,22"` | Set up port forwarding |

## Shared Folders

| Command | Description |
|---------|-------------|
| `VBoxManage sharedfolder add <vm_name> --name <share_name> --hostpath <host_path>` | Add shared folder |
| `VBoxManage sharedfolder remove <vm_name> --name <share_name>` | Remove shared folder |
| `VBoxManage setextradata <vm_name> "VBoxInternal2/SharedFoldersEnableSymlinksCreate/<share_name>" 1` | Enable symlinks in shared folder |

## Guest Additions

| Command | Description |
|---------|-------------|
| `VBoxManage controlvm <vm_name> usbattach <uuid>` | Attach USB device |
| `VBoxManage controlvm <vm_name> usbdetach <uuid>` | Detach USB device |
| `VBoxManage list usbhost` | List available USB devices |
| `VBoxManage guestcontrol <vm_name> execute --image "/path/to/program" --username <user> --password <pass>` | Execute command in guest |

## Import/Export

| Command | Description |
|---------|-------------|
| `VBoxManage export <vm_name> -o <file.ova>` | Export VM to OVA |
| `VBoxManage import <file.ova>` | Import VM from OVA |
| `VBoxManage import <file.ovf>` | Import VM from OVF |

## Common Command Combinations

### Basic VM Creation
```bash
# Create new VM with typical settings
VBoxManage createvm --name "Ubuntu_Server" --register
VBoxManage modifyvm "Ubuntu_Server" --memory 2048 --cpus 2 --nic1 nat
VBoxManage createhd --filename "ubuntu.vdi" --size 20000
VBoxManage storagectl "Ubuntu_Server" --name "SATA" --add sata
VBoxManage storageattach "Ubuntu_Server" --storagectl "SATA" --port 0 --device 0 --type hdd --medium "ubuntu.vdi"
VBoxManage storageattach "Ubuntu_Server" --storagectl "SATA" --port 1 --device 0 --type dvddrive --medium /path/to/ubuntu.iso
```

### Network Configuration
```bash
# Set up bridged networking with port forwarding
VBoxManage modifyvm "MyVM" --nic1 bridged --bridgeadapter1 eth0
VBoxManage modifyvm "MyVM" --natpf1 "guesthttp,tcp,,8080,,80"
```

### Backup and Restore
```bash
# Create snapshot before changes
VBoxManage snapshot "Production_VM" take "Before_Updates"

# Restore if updates fail
VBoxManage snapshot "Production_VM" restore "Before_Updates"
```

### Headless Operation
```bash
# Start VM in headless mode with RDP access
VBoxManage modifyvm "Web_Server" --vrde on --vrdeport 3389
VBoxManage startvm "Web_Server" --type headless
```

Reach out: https://guns.lol/january1073
