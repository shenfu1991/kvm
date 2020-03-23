# kvm
kvm虚拟化技术

检查硬件是否支持,返回数量大于0就是支持
<pre>
egrep -c '(vmx|svm)' /proc/cpuinfo
</pre>

安装依赖
<pre>
sudo apt-get install qemu-kvm libvirt-bin virtinst bridge-utils cpu-checker
</pre>

是否可以安装
<pre>
kvm-ok
</pre>

返回
<pre>
INFO: /dev/kvm exists
KVM acceleration can be used
</pre>

启用
<pre>
sudo systemctl enable libvirtd
sudo systemctl start libvirtd
</pre>

创建镜像
<pre>
qemu-img create -f qcow2 /home/shenfu/img/ubuntu.qcow2 20G
</pre>

创建虚拟机
<pre>
sudo virt-install  --name shenfu  --ram=1024 --vcpu 2  --disk path=/home/shenfu/img/ubuntu.qcow2,bus=virtio,size=20  --cdrom /home/shenfu/ubuntu-18.04.4-live-server-amd64.iso --graphics vnc --network bridge:br0
</pre>

vnc映射,192.168.123.170是母鸡ip
<pre>
ssh shenfu@192.168.123.170 -L 5901:127.0.0.1:5901
</pre>

使用127.0.0.1:5901 登录虚拟机，引导安装完成  


母鸡桥接设置

<pre>
network:
    ethernets:
        enp1s0:
            dhcp4: false
        enp2s0:
            dhcp4: true
        enp3s0:
            dhcp4: true
        enp4s0:
            dhcp4: true
        enp5s0:
            dhcp4: true
        enp6s0:
            dhcp4: true
    bridges:
        br0:
            interfaces: [enp1s0]
            addresses: [192.168.123.239/24]
            gateway4: 192.168.123.1
            mtu: 1500
            nameservers:
                addresses: [202.103.224.68,8.8.8.8]
            parameters:
                stp: true
                forward-delay: 4
            dhcp4: false

    version: 2

</pre>
