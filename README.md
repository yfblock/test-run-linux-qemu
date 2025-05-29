# 在 qemu 中运行 Linux

> 测试了一些方法，发现最方便的是下面提供的方案

## 下载镜像

```shell
wget https://cdimage.debian.org/cdimage/cloud/bullseye/20250528-2126/debian-11-nocloud-amd64-20250528-2126.qcow2
```

## 运行 qemu 

```shell
 qemu-system-x86_64 \
  -m 2048 \
  -smp 2 \
  -drive file=debian-11-nocloud-amd64-20250528-2126.qcow2,format=qcow2 \
  -net nic -net user,hostfwd=tcp::2222-:22 \
  -nographic
```

> 可以根据需要对参数进行调整，进入系统后**用户名为 root，密码为空，直接回车可以进入系统**，如果需要传输程序，可以有多种方法，

- 再 guest 中安装 ssh，然后利用 scp 和 ssh 传输和运行，注意这里将 guest 的 22 端口映射到宿主机的 2222
- 创建一个 img 文件，然后塞入需要使用的程序，利用 `-drive file=xxx.img` 指定，在 qemu 启动后的 guest 里挂载
- 利用 9pfs 再 host 和 guest 之间共享

## 参考资料

- https://cdimage.debian.org/cdimage/cloud/
- https://wiki.debian.org/QEMU
- https://cloudinit.readthedocs.io/en/latest/reference/modules.html#mod-cc-set-passwords
- https://documentation.ubuntu.com/server/how-to/virtualisation/qemu/
