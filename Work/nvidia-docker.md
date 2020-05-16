# 安装 docker 和 NVIDIA-docker

2020年04月20日18:52:15

##  dpkg-deb 错误

https://blog.csdn.net/l2181265/article/details/90116818

udo apt-get -f install nvidia-cuda-dev

**出错：**dpkg-deb：错误：子进程 粘贴 被信号(断开的管道) 终止了
在处理时有错误发生：
/var/cache/apt/archives/nvidia-cuda-dev_7.5.18-0ubuntu1_amd64.deb
E: Sub-process /usr/bin/dpkg returned an error code (1)

**解决：**sudo dpkg -i --force-overwrite /var/cache/apt/archives/*.deb
再重新输入sudo apt-get -f install nvidia-cuda-dev

##　docker: Error response from daemon: Unknown runtime specified nvidia.

https://blog.csdn.net/weixin_32820767/article/details/80538510

nvidia-docker 没有注册

注册方式

```bash
# Systemd drop-in file
sudo mkdir -p /etc/systemd/system/docker.service.d
sudo tee /etc/systemd/system/docker.service.d/override.conf <<EOF
[Service]
ExecStart=
ExecStart=/usr/bin/dockerd --host=fd:// --add-runtime=nvidia=/usr/bin/nvidia-container-runtime
EOF
sudo systemctl daemon-reload
sudo systemctl restart docker

# Daemon configuration file
sudo tee /etc/docker/daemon.json <<EOF
{
    "runtimes": {
        "nvidia": {
            "path": "/usr/bin/nvidia-container-runtime",
            "runtimeArgs": []
        }
    }
}
EOF
sudo pkill -SIGHUP dockerd
```



## docker run error

```bash
docker: Error response from daemon: OCI runtime create failed: unable to retrieve OCI runtime error (open /run/containerd/io.containerd.runtime.v1.linux/moby/4165cd770f6e40cbca377fd5799e7a533c67b45f5eb997a1971ea15966483ca0/log.json: no such file or directory): fork/exec /usr/bin/nvidia-container-runtime: no such file or directory: unknown.
```

https://github.com/NVIDIA/nvidia-docker/issues/686

> sudo apt-get install nvidia-container-runtime



## run.sh error

```bash
docker: Error response from daemon: create nvidia_driver_418.39: VolumeDriver.Create: internal error, check logs for details.
```

https://www.jianshu.com/p/d24b41acd001

```bash
systemctl status nvidia-docker
```

log is 

```bash
● nvidia-docker.service - NVIDIA Docker plugin
   Loaded: loaded (/lib/systemd/system/nvidia-docker.service; enabled; vendor preset: enabled)
   Active: active (running) since 一 2020-04-20 18:35:19 CST; 1h 46min ago
     Docs: https://github.com/NVIDIA/nvidia-docker/wiki
 Main PID: 10932 (nvidia-docker-p)
    Tasks: 9
   Memory: 36.8M
      CPU: 2.555s
   CGroup: /system.slice/nvidia-docker.service
           └─10932 /usr/bin/nvidia-docker-plugin -s /var/lib/nvidia-docker

4月 20 18:35:22 amax nvidia-docker-plugin[10932]: /usr/bin/nvidia-docker-plugin | 2020/04/20 18:35:22 Serving plugin API at /var/lib/nvidia-docker
4月 20 18:35:22 amax nvidia-docker-plugin[10932]: /usr/bin/nvidia-docker-plugin | 2020/04/20 18:35:22 Serving remote API at localhost:3476
4月 20 19:25:15 amax nvidia-docker-plugin[10932]: /usr/bin/nvidia-docker-plugin | 2020/04/20 19:25:15 Received activate request
4月 20 19:25:15 amax nvidia-docker-plugin[10932]: /usr/bin/nvidia-docker-plugin | 2020/04/20 19:25:15 Plugins activated [VolumeDriver]
4月 20 19:25:15 amax nvidia-docker-plugin[10932]: /usr/bin/nvidia-docker-plugin | 2020/04/20 19:25:15 Received create request for volume 'nvidia_driver_418.39'
4月 20 19:25:15 amax nvidia-docker-plugin[10932]: /usr/bin/nvidia-docker-plugin | 2020/04/20 19:25:15 Error: link /usr/lib/nvidia-418/vdpau/libvdpau_nvidia.so.418.39 /var/lib/nvidia-docker/volumes/nvidia_
4月 20 19:35:59 amax nvidia-docker-plugin[10932]: /usr/bin/nvidia-docker-plugin | 2020/04/20 19:35:59 Received create request for volume 'nvidia_driver_418.39'
4月 20 19:35:59 amax nvidia-docker-plugin[10932]: /usr/bin/nvidia-docker-plugin | 2020/04/20 19:35:59 Error: link /usr/lib/nvidia-418/vdpau/libvdpau_nvidia.so.418.39 /var/lib/nvidia-docker/volumes/nvidia_
4月 20 20:21:25 amax nvidia-docker-plugin[10932]: /usr/bin/nvidia-docker-plugin | 2020/04/20 20:21:25 Received create request for volume 'nvidia_driver_418.39'
4月 20 20:21:25 amax nvidia-docker-plugin[10932]: /usr/bin/nvidia-docker-plugin | 2020/04/20 20:21:25 Error: link /usr/lib/nvidia-418/vdpau/libvdpau_nvidia.so.418.39 /var/lib/nvidia-docker/volumes/nvidia_
```

就算使用 nvidia-docker volume create ... 只是能保证进入镜像, 但用到gpu还是会崩溃

https://www.cnblogs.com/hozhangel/p/11900224.html 正如这篇博客的报错

稍有参考意义的博客

https://blog.csdn.net/qq_38079008/article/details/83620573

https://zhuanlan.zhihu.com/p/37519492

 ` dpkg -l | grep -i nvidia` 查看和 nvidia 有关的库

 `systemctl status nvidia-docker` 查看 nvidia 日志

https://github.com/NVIDIA/nvidia-docker/issues/1109 docker版本过低

https://github.com/NVIDIA/nvidia-docker/issues/142 nvidia-docker 多版本冲突

https://github.com/NVIDIA/nvidia-docker/issues/229 非常相似的问题, link file exists

https://github.com/NVIDIA/nvidia-docker/issues/359 非常相似的问题, link file exists

https://github.com/NVIDIA/nvidia-docker/issues/735 非常相似的问题, link file exists, 没有回复

https://github.com/NVIDIA/nvidia-docker/issues/300 库的问题, 重装系统解决

https://github.com/NVIDIA/nvidia-docker/issues/358 无关



以下以 \#133 为模板, invalid cross-device link 错误, /var 和 /usr 不在同一磁盘分区

https://github.com/NVIDIA/nvidia-docker/issues/133

https://github.com/NVIDIA/nvidia-docker/issues/188 

https://github.com/NVIDIA/nvidia-docker/issues/188#issuecomment-244764993 具体操作

https://github.com/NVIDIA/nvidia-docker/issues/290 \#290 的操作和 \#188 的操作相同

https://github.com/NVIDIA/nvidia-docker/issues/184

https://github.com/NVIDIA/nvidia-docker/issues/334 kill nvidia-docker-plugin 的讨论