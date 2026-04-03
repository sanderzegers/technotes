# govc Cheatsheet for Common ESXi / vSphere Tasks

Official documentation:

- govc command reference (USAGE.md): [https://github.com/vmware/govmomi/blob/main/govc/USAGE.md](https://github.com/vmware/govmomi/blob/main/govc/USAGE.md)

---

## 1) Connection defaults

```bash
export GOVC_URL='https://esxi-or-vcenter/sdk'
export GOVC_USERNAME='your-user'
export GOVC_PASSWORD='your-pass'
# Disable TLS cert checks
export GOVC_INSECURE=1

export GOVC_DATACENTER='Datacenter'
export GOVC_DATASTORE='datastore1'
export GOVC_NETWORK='VM Network'
export GOVC_RESOURCE_POOL='*/Resources'
export GOVC_HOST='esxi01.example.com'
```

## 2) Basic checks

```bash
govc about
govc env
govc version -l
```

## 3) Browse inventory / list VMs

```bash
# List top-level inventory
govc ls /

# Find all VMs
govc find / -type m

# Find all hosts
govc find / -type h

# Find all datastores
govc find / -type s

# Find all networks
govc find / -type n
```

## 4) VM info / status / IP

```bash
# Basic VM info
govc vm.info my-vm

# Full JSON output
govc vm.info -json my-vm | jq .

# Show VM IP
govc vm.ip my-vm

# Wait for an IP
govc vm.ip -wait 5m my-vm

# Show all IPv4 addresses
govc vm.ip -a -v4 my-vm
```

## 5) Power operations

```bash
# Power on
govc vm.power -on my-vm

# Graceful guest shutdown
govc vm.power -s my-vm

# Hard power off
govc vm.power -off -force my-vm

# Reboot guest
govc vm.power -r my-vm

# Reset power
govc vm.power -reset my-vm

# Pause/suspend
govc vm.power -suspend my-vm
```

## 6) Change CPU / memory / advanced settings

```bash
# Change CPU and memory
govc vm.change -vm my-vm -c 4 -m 8192

# Enable CPU and memory hot-add
govc vm.change -vm my-vm -cpu-hot-add-enabled -memory-hot-add-enabled

# Set an ExtraConfig key
govc vm.change -vm my-vm -e guestinfo.role=web
```

## 7) Create a VM

### Create an empty VM

```bash
govc vm.create \
  -on=false \
  -m 4096 \
  -c 2 \
  -g ubuntu64Guest \
  -disk 40G \
  -net "VM Network" \
  -net.adapter vmxnet3 \
  my-vm
```

### Create a VM with an ISO

```bash
govc vm.create \
  -on=false \
  -m 4096 \
  -c 2 \
  -g ubuntu64Guest \
  -disk 40G \
  -iso "[datastore1] iso/ubuntu.iso" \
  my-vm
```

### Create from a content library ISO

```bash
govc vm.create -iso library:/boot/linux/ubuntu.iso my-vm
```

## 8) Clone a VM

```bash
govc vm.clone -vm template-vm new-vm
govc vm.clone -vm template-vm -on=true -host=esxi01 -ds=datastore01 new-vm
govc vm.clone -vm template-vm -link new-vm
```

## 9) Create / attach disks

### Create a disk and attach it to a VM

```bash
govc vm.disk.create -vm my-vm -name data.vmdk -size 100G
```

### Attach an existing VMDK

```bash
govc vm.disk.attach -vm my-vm -disk my-vm/shared.vmdk
```

### Create a standalone VMDK on a datastore

```bash
govc datastore.disk.create -size 24G disks/data01.vmdk
```

## 10) Add a serial port

```bash
# Add a serial port to the VM
govc device.serial.add -vm my-vm

# List VM devices and find the serial port name
govc device.ls -vm my-vm | grep serialport-

# Show serial port details
govc device.info -vm my-vm serialport-*
```

### Connect the serial port

```bash
# Connect to a telnet service
govc device.serial.connect -vm my-vm -device serialport-8000 telnet://:33233

# Back the serial port with a datastore file
govc device.serial.connect -vm my-vm "[datastore1] my-vm/console.log"

# Auto-create a file-backed serial log in the VM log directory
govc device.serial.connect -vm my-vm -

# Tail the resulting log file
govc datastore.tail -f my-vm/serialport-8000.log
```

### Disconnect the serial port

```bash
govc device.serial.disconnect -vm my-vm -device serialport-8000
```

### Notes

- Add the serial port first, then connect it.
- If you omit `-device`, `govc` uses the first serial port.
- Using `-` as the URI creates a file-backed device in the VM log directory.
- `device.serial.connect` also supports `-client` and `-vspc-proxy` for client direction and vSPC setups.

## 11) VM snapshots

```bash
# Create a snapshot
govc snapshot.create -vm my-vm pre-change

# Create a memory snapshot
govc snapshot.create -vm my-vm -m=true pre-upgrade

# List snapshot tree
govc snapshot.tree -vm my-vm

# Revert to a snapshot
govc snapshot.revert -vm my-vm pre-change

# Remove one snapshot
govc snapshot.remove -vm my-vm pre-change

# Remove all snapshots
govc snapshot.remove -vm my-vm '*'
```

## 12) Transfer files to and from a datastore

```bash
# Upload local -> datastore
govc datastore.upload ./config.iso my-vm/config.iso

# Upload from stdin
genisoimage ... | govc datastore.upload - my-vm/config.iso

# Download datastore -> local
govc datastore.download my-vm/vmware.log ./local.log

# Download to stdout
govc datastore.download my-vm/vmware.log - | grep -i error

# Copy within datastore
govc datastore.cp foo/foo.vmx foo/foo.vmx.old
```

## 13) Transfer files to and from the guest OS

```bash
export GOVC_GUEST_LOGIN='user:pass'

# Upload local -> guest
govc guest.upload -vm my-vm ./app.conf /etc/app.conf

# Download guest -> local
govc guest.download -vm my-vm /var/log/app.log ./app.log
```

### Stream data

```bash
echo "hello" | govc guest.upload -vm my-vm - /tmp/hello.txt
govc guest.download -vm my-vm /etc/motd -
```

## 14) Destroy a VM

```bash
govc vm.destroy my-vm
```

## 15) CPU affinity with pyVmomi (not govc)

`govc` does not provide a direct command for setting VM CPU affinity, so use `pyVmomi` for this task.

Example host CPU layout:

Intel Core Ultra 5 225H has a total of 14 logical CPUs. Avoid using core 12 and 13 if you want to keep the VM off the LP E-cores.

| ESXi Logical ID | APICID | L2 Size | L2 Shared Count    | Core Type            |
| --------------- | ------ | ------- | ------------------ | -------------------- |
| 0, 1            | 0, 8   | 3 MB    | 1 (private)        | P-core               |
| 2–9             | 16–30  | 4 MB    | 4 (shared cluster) | E-core               |
| 10, 11          | 32, 40 | 3 MB    | 1 (private)        | P-core               |
| 12, 13          | 64, 66 | 2 MB    | 2 (shared cluster) | LP E-core (SoC tile) |

- Avoid CPU cores 12,13 at any time.
- High performance: 0,1,10,11
- Normal performance: 2-9, or 0-11\


### Example: set CPU affinity on a VM

```python
from pyVmomi import vim
from pyVim.connect import SmartConnect
import ssl

context = ssl._create_unverified_context()
si = SmartConnect(host="192.168.1.254", user="admin", pwd="password", sslContext=context)

content = si.RetrieveContent()
container = content.viewManager.CreateContainerView(content.rootFolder, [vim.VirtualMachine], True)
vm = next(v for v in container.view if v.name == "LINUX01")

spec = vim.VirtualMachineConfigSpec()
spec.cpuAffinity = vim.VirtualMachineAffinityInfo()
spec.cpuAffinity.affinitySet = list(range(2, 10))
vm.ReconfigVM_Task(spec)
```

### Notes

- `affinitySet` is a list of ESXi logical CPU IDs.
- The logical CPU IDs in the table above are host-specific and should be verified on your own ESXi host before applying affinity rules.



## 16) Useful one-liners

```bash
# Wait for an IP
govc vm.ip -wait 10m my-vm

# Create without powering on
govc vm.create -on=false my-vm

# Hard stop a stuck VM
govc vm.power -off -force my-vm

# Upload an ISO
govc datastore.upload ./ubuntu.iso iso/ubuntu.iso

# Pull a guest log
govc guest.download -vm my-vm /var/log/syslog ./syslog
```

