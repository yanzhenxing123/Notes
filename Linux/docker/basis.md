# docker工作原理及基础命令



## 工作原理

> 镜像就相当于模板，容器相当于实例

![image-20200825192708839](https://cdn.jsdelivr.net/gh/yanzhenxing123/blogImg@master/typora202008/25/192709-940993.png)

## 为什么比VM快

![image-20200825192858880](https://cdn.jsdelivr.net/gh/yanzhenxing123/blogImg@master/typora202008/25/192859-91311.png)

1.Docker有着比虚拟机更少的抽象层，由于Docker不需要Hypervisor实现硬件资源虚拟化，运行在Docker容器上的程序直接使用的都是实际物理机的硬件资源，因此在Cpu、内存利用率上Docker将会在效率上有明显优势。

2.Docker利用的是宿主机的内核，而不需要**Guest OS**，因此，当新建一个容器时，Docker不需要和虚拟机一样重新加载一个操作系统，避免了引导、加载操作系统内核这个比较费时费资源的过程，当新建一个虚拟机时，虚拟机软件需要加载Guest OS，这个新建过程是分钟级别的，而Docker由于直接利用宿主机的操作系统则省略了这个过程，因此新建一个Docker容器只需要几秒钟。

|            | Docker容器              | 虚拟机（VM）                |
| ---------- | ----------------------- | --------------------------- |
| 操作系统   | 与宿主机共享OS          | 宿主机OS上运行宿主机OS      |
| 存储大小   | 镜像小，便于存储与传输  | 镜像庞大（vmdk等）          |
| 运行性能   | 几乎无额外性能损失      | 操作系统额外的cpu、内存消耗 |
| 移植性     | 轻便、灵活、适用于Linux | 笨重、与虚拟化技术耦合度高  |
| 硬件亲和性 | 面向软件开发者          | 面向硬件运维者              |



## 常用命令

### 帮助命令

![image-20200825193240894](https://cdn.jsdelivr.net/gh/yanzhenxing123/blogImg@master/typora202008/25/193241-859182.png)

### 镜像命令

`docker images`

![image-20200825193421130](https://cdn.jsdelivr.net/gh/yanzhenxing123/blogImg@master/typora202008/25/193424-475702.png)

![image-20200825193339340](https://cdn.jsdelivr.net/gh/yanzhenxing123/blogImg@master/typora202008/25/193343-343595.png)

`docker search`

![image-20200825193730352](https://cdn.jsdelivr.net/gh/yanzhenxing123/blogImg@master/typora202008/25/193731-256297.png)`docker pull`: 下载镜像

![image-20200825194037116](https://cdn.jsdelivr.net/gh/yanzhenxing123/blogImg@master/typora202008/25/194044-619639.png)

![image-20200825194107034](https://cdn.jsdelivr.net/gh/yanzhenxing123/blogImg@master/typora202008/25/194107-114641.png)

 `docker rmi -f` ： 删除镜像 

**remove image**

![image-20200825194425032](https://cdn.jsdelivr.net/gh/yanzhenxing123/blogImg@master/typora202008/25/194425-681941.png)

## 容器命令

### 启动

![image-20200825194948627](https://cdn.jsdelivr.net/gh/yanzhenxing123/blogImg@master/typora202008/25/194949-818747.png)

### docker运行

![image-20200825195045179](https://cdn.jsdelivr.net/gh/yanzhenxing123/blogImg@master/typora202008/25/195057-53338.png)

![image-20200825195210898](https://cdn.jsdelivr.net/gh/yanzhenxing123/blogImg@master/typora202008/25/203415-329958.png)



### 删除容器

![image-20200825195402996](https://cdn.jsdelivr.net/gh/yanzhenxing123/blogImg@master/typora202008/25/195403-811580.png)

### 启动和停止容器的操作

![image-20200825195449084](https://cdn.jsdelivr.net/gh/yanzhenxing123/blogImg@master/typora202008/25/195449-850220.png)

## 常用的其他命令

### 后台启动容器dock

![image-20200825195941978](https://cdn.jsdelivr.net/gh/yanzhenxing123/blogImg@master/typora202008/25/203618-540686.png)

### 查看日志

![image-20200825200447007](https://cdn.jsdelivr.net/gh/yanzhenxing123/blogImg@master/typora202008/25/200447-247641.png)

### 查看容器内部的进程信息

![image-20200825200524576](https://cdn.jsdelivr.net/gh/yanzhenxing123/blogImg@master/typora202008/25/200524-98638.png)

### 查看镜像的元数据

`docker inspect 容器id`

```shell
docker inspect dbdbad0077ad
[
    {
        "Id": "dbdbad0077ad52f2cbc03ece0f307ca04f9fa252bc5e37f147ebb0168c7157c5",
        "Created": "2020-08-25T11:48:21.146791524Z",
        "Path": "/bin/bash",
        "Args": [],
        "State": {
            "Status": "exited",
            "Running": false,
            "Paused": false,
            "Restarting": false,
            "OOMKilled": false,
            "Dead": false,
            "Pid": 0,
            "ExitCode": 0,
            "Error": "",
            "StartedAt": "2020-08-25T11:48:21.886501543Z",
            "FinishedAt": "2020-08-25T11:49:19.38230342Z"
        },
        "Image": "sha256:0d120b6ccaa8c5e149176798b3501d4dd1885f961922497cd0abef155c869566",
        "ResolvConfPath": "/var/lib/docker/containers/dbdbad0077ad52f2cbc03ece0f307ca04f9fa252bc5e37f147ebb0168c7157c5/resolv.conf",
        "HostnamePath": "/var/lib/docker/containers/dbdbad0077ad52f2cbc03ece0f307ca04f9fa252bc5e37f147ebb0168c7157c5/hostname",
        "HostsPath": "/var/lib/docker/containers/dbdbad0077ad52f2cbc03ece0f307ca04f9fa252bc5e37f147ebb0168c7157c5/hosts",
        "LogPath": "/var/lib/docker/containers/dbdbad0077ad52f2cbc03ece0f307ca04f9fa252bc5e37f147ebb0168c7157c5/dbdbad0077ad52f2cbc03ece0f307ca04f9fa252bc5e37f147ebb0168c7157c5-json.log",
        "Name": "/reverent_bhabha",
        "RestartCount": 0,
        "Driver": "overlay2",
        "Platform": "linux",
        "MountLabel": "",
        "ProcessLabel": "",
        "AppArmorProfile": "docker-default",
        "ExecIDs": null,
        "HostConfig": {
            "Binds": null,
            "ContainerIDFile": "",
            "LogConfig": {
                "Type": "json-file",
                "Config": {}
            },
            "NetworkMode": "default",
            "PortBindings": {},
            "RestartPolicy": {
                "Name": "no",
                "MaximumRetryCount": 0
            },
            "AutoRemove": false,
            "VolumeDriver": "",
            "VolumesFrom": null,
            "CapAdd": null,
            "CapDrop": null,
            "Capabilities": null,
            "Dns": [],
            "DnsOptions": [],
            "DnsSearch": [],
            "ExtraHosts": null,
            "GroupAdd": null,
            "IpcMode": "private",
            "Cgroup": "",
            "Links": null,
            "OomScoreAdj": 0,
            "PidMode": "",
            "Privileged": false,
            "PublishAllPorts": false,
            "ReadonlyRootfs": false,
            "SecurityOpt": null,
            "UTSMode": "",
            "UsernsMode": "",
            "ShmSize": 67108864,
            "Runtime": "runc",
            "ConsoleSize": [
                0,
                0
            ],
            "Isolation": "",
            "CpuShares": 0,
            "Memory": 0,
            "NanoCpus": 0,
            "CgroupParent": "",
            "BlkioWeight": 0,
            "BlkioWeightDevice": [],
            "BlkioDeviceReadBps": null,
            "BlkioDeviceWriteBps": null,
            "BlkioDeviceReadIOps": null,
            "BlkioDeviceWriteIOps": null,
            "CpuPeriod": 0,
            "CpuQuota": 0,
            "CpuRealtimePeriod": 0,
            "CpuRealtimeRuntime": 0,
            "CpusetCpus": "",
            "CpusetMems": "",
            "Devices": [],
            "DeviceCgroupRules": null,
            "DeviceRequests": null,
            "KernelMemory": 0,
            "KernelMemoryTCP": 0,
            "MemoryReservation": 0,
            "MemorySwap": 0,
            "MemorySwappiness": null,
            "OomKillDisable": false,
            "PidsLimit": null,
            "Ulimits": null,
            "CpuCount": 0,
            "CpuPercent": 0,
            "IOMaximumIOps": 0,
            "IOMaximumBandwidth": 0,
            "MaskedPaths": [
                "/proc/asound",
                "/proc/acpi",
                "/proc/kcore",
                "/proc/keys",
                "/proc/latency_stats",
                "/proc/timer_list",
                "/proc/timer_stats",
                "/proc/sched_debug",
                "/proc/scsi",
                "/sys/firmware"
            ],
            "ReadonlyPaths": [
                "/proc/bus",
                "/proc/fs",
                "/proc/irq",
                "/proc/sys",
                "/proc/sysrq-trigger"
            ]
        },
        "GraphDriver": {
            "Data": {
                "LowerDir": "/var/lib/docker/overlay2/ff073eba073031b10b9962d9b555a36bc5308d6fd76a5e4ddceb2aac8235bcdc-init/diff:/var/lib/docker/overlay2/05ca34ff196d548bb6c0b52d91d66c46a25117940311deccfda59aa4c555008d/diff",
                "MergedDir": "/var/lib/docker/overlay2/ff073eba073031b10b9962d9b555a36bc5308d6fd76a5e4ddceb2aac8235bcdc/merged",
                "UpperDir": "/var/lib/docker/overlay2/ff073eba073031b10b9962d9b555a36bc5308d6fd76a5e4ddceb2aac8235bcdc/diff",
                "WorkDir": "/var/lib/docker/overlay2/ff073eba073031b10b9962d9b555a36bc5308d6fd76a5e4ddceb2aac8235bcdc/work"
            },
            "Name": "overlay2"
        },
        "Mounts": [],
        "Config": {
            "Hostname": "dbdbad0077ad",
            "Domainname": "",
            "User": "",
            "AttachStdin": true,
            "AttachStdout": true,
            "AttachStderr": true,
            "Tty": true,
            "OpenStdin": true,
            "StdinOnce": true,
            "Env": [
                "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"
            ],
            "Cmd": [
                "/bin/bash"
            ],
            "Image": "centos",
            "Volumes": null,
            "WorkingDir": "",
            "Entrypoint": null,
            "OnBuild": null,
            "Labels": {
                "org.label-schema.build-date": "20200809",
                "org.label-schema.license": "GPLv2",
                "org.label-schema.name": "CentOS Base Image",
                "org.label-schema.schema-version": "1.0",
                "org.label-schema.vendor": "CentOS"
            }
        },
        "NetworkSettings": {
            "Bridge": "",
            "SandboxID": "8522f6bd7b98b2f8363d4dc1b44d4c4de1e44af571f44a012745feaa17a268ca",
            "HairpinMode": false,
            "LinkLocalIPv6Address": "",
            "LinkLocalIPv6PrefixLen": 0,
            "Ports": {},
            "SandboxKey": "/var/run/docker/netns/8522f6bd7b98",
            "SecondaryIPAddresses": null,
            "SecondaryIPv6Addresses": null,
            "EndpointID": "",
            "Gateway": "",
            "GlobalIPv6Address": "",
            "GlobalIPv6PrefixLen": 0,
            "IPAddress": "",
            "IPPrefixLen": 0,
            "IPv6Gateway": "",
            "MacAddress": "",
            "Networks": {
                "bridge": {
                    "IPAMConfig": null,
                    "Links": null,
                    "Aliases": null,
                    "NetworkID": "57bf29e296d89ef66f5b298746e81cc8215bcfbf1309693dbd1930578d7e1c2c",
                    "EndpointID": "",
                    "Gateway": "",
                    "IPAddress": "",
                    "IPPrefixLen": 0,
                    "IPv6Gateway": "",
                    "GlobalIPv6Address": "",
                    "GlobalIPv6PrefixLen": 0,
                    "MacAddress": "",
                    "DriverOpts": null
                }
            }
        }
    }
]

```

### 进入当前正在运行的容器

`docker exec -it 容器id /bin/bash`

![image-20200825201034111](https://cdn.jsdelivr.net/gh/yanzhenxing123/blogImg@master/typora202008/25/201034-301434.png)

![image-20200825201151537](https://cdn.jsdelivr.net/gh/yanzhenxing123/blogImg@master/typora202008/25/201151-957455.png)

![image-20200825201213929](https://cdn.jsdelivr.net/gh/yanzhenxing123/blogImg@master/typora202008/25/201214-862532.png)

### 将容器中的文件拷贝到主机

![image-20200825201524736](https://cdn.jsdelivr.net/gh/yanzhenxing123/blogImg@master/typora202008/25/201525-155244.png)



## 小结

![image-20200825202635967](https://cdn.jsdelivr.net/gh/yanzhenxing123/blogImg@master/typora202008/25/202636-104476.png)