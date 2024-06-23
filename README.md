# mqbrd


## Why multi queue?

https://dl.acm.org/doi/abs/10.1145/2485732.2485740

single queue(left) & multi queue(right)



<img src="https://github.com/MinkiJo/mqbrd/assets/62167266/c455876f-d1a2-4a34-942a-3831d9ba71b3" alt="image" style="zoom:80%;" />



## Environment 

Linux kernel 4.4.1



## Development



1. package install

```bash
sudo apt-get update
sudo apt-get install build-essential libncurses5 libncurses5-dev bin86 kernel-package libssl-dev bison flex libelf-dev dwarves
sudo reboot
```

2. kernel build

```bash
sudo wget https://cdn.kernel.org/pub/linux/kernel/v4.x/linux-4.4.tar.xz
sudo tar -xvf linux-4.4.tar.xz

sudo mv linux-4.4.tar.xz /usr/src/

#config
sudo cp linux-headers-5.11.0-41-generic/.config linux-4.4.tar.xz
```

```bash
sudo vi .config
//CONFIG_SYSTEM_TRUSTED_KEYS="debian/canonical-certs.pem" -> CONFIG_SYSTEM_TRUSTED_KEYS = "" 
//CONFIG_SYSTEM_REVOCATION_KEYS="debian/canonical-certs.pem" -> CONFIG_SYSTEM_REVOCATION_KEYS=""

sudo make menuconfig
#load → ok → exit → yes

grep -c processor /proc/cpuinfo 
sudo make -j(cpuinfo)
sudo make modules_install
sudo make install

#set boot kernel
sudo vi /boot/grub/grub.cfg
sudo vi /etc/default/grub


```

3. module install

```bash
#makefile
make 
insmod mybrd.ko
```



4. Test (example)

```bash
fio --filename=/dev/sqbrd --direct=0 --ioengine=libaio --numjobs=512 --iodepth=32 --rw=randread --bs=4k --size=16G --runtime=60 --time_based --group_reporting --name=brdtest

fio --filename=/dev/mqbrd --direct=0 --ioengine=libaio --numjobs=512 --iodepth=32 --rw=randread --bs=4k --size=16G --runtime=60 --time_based --group_reporting --name=brdtest
```







#### Ref 

https://kernel.bz/boardPost/118679/16

https://xoit.tistory.com/

https://github.com/torvalds/linux/tree/v4.4-rc1

