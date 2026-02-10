+++
forward = true
title = '명령어를 이용해 간단한 컨테이너를 만들어보자'
date = 2023-10-06T20:27:34+09:00
draft = false
+++


일반적으로 우리는 컨테이너를 이용하기 위하여 Docker와 같은 컨테이너 프로그램을 통해 컨테이너를 사용합니다. 하지만 기본적인 컨테이너의 경우 리눅스 명령어의 조합만으로도 구현이 가능합니다. 이 글에서는 리눅스 명령을 통해 컨테이너가 어떻게 격리되고 작동되는지 알아보겠습니다.

## chroot를 통한 루트 디렉토리 변경

컨테이너를 공부하다 보면 무조건 들어보는 명령이 하나 있습니다 바로
   `chroot`
   입니다. 
   `chroot`
   가 어떤 명령인지 매뉴얼을 통해 확인하면 다음과 같이 기술되어 있습니다.

**NAME**

chroot - run command or interactive shell with special root directory

**SYNOPSIS**

chroot - [OPTION] NEWROOT [COMMAND [ARG]...]

chroot - OPTION

**DESCRIPTION**

Run COMMAND with root directory set to NEWROOT.

즉 특수한 루트 디렉토리에서 커맨드를 실행하거나 대화형 쉘을 실행시킬 수 있는 명령이며 chroot &lt;디렉토리&gt; &lt;명령어&gt; 구조로 새로운 루트 디렉토리에서 명령어를 실행하는 커맨드입니다. 
   `chroot`
   가 어떤 명령인지 알았으니 실제 어떻게 작동하는지 새로운 폴더를 만들고 
   `chroot`
    명령을 실행해 볼 시간입니다.

```bash
test@jungnas:~/container$ mkdir new_roottest@jungnas:~/container$ sudo chroot new_root lschroot: failed to run command ‘ls’: No such file or directory
```

ls 커맨드가 없다고 하면서 생각했던 대로 동작하지 않습니다. 원인은 해당 폴더가 빈 폴더이기 때문에 새로 만든 루트에서 명령을 실행할 실행파일 또한 없어 발생하는 일입니다. 이를 위해 가장 가볍고 컨테이너 환경에서 많이 쓰이는 alpine 리눅스를 사용해 보겠습니다. alpine 리눅스의 경우 tar.gz 파일로도 제공되어 이번 예제로 사용하기 적합한 배포판입니다.

```bash
test@jungnas:~/container$mkdir alpinelinuxtest@jungnas:~/container$cd alpinelinuxtest@jungnas:~/container/alpinelinux$lstest@jungnas:~/container/alpinelinux$wget https://dl-cdn.alpinelinux.org/alpine/latest-stable/releases/x86_64/alpine-minirootfs-3.17.3-x86_64.tar.gz
```

이제 압축을 해제하였으니 alpinelinux 디렉토리로 chroot 명령을 실행해 보겠습니다.

```bash
test@jungnas:~/container/alpinelinux$ sudo chroot alpinelinux ls /bindevetchomelibmediamntoptprocrootrunsbinsrvsystmpusrvar
```

ls / 명령을 실행하였더니 압축 해제된 폴더의 루트 디렉토리 모습을 확인할 수 있습니다. 그렇다면 실제로 shell 을 실행해 보면 어떨까요? alpine 리눅스의 경우 bash 쉘이 없기 때문에 sh로 실행합니다.

```bash
test@jungnas:~/container/alpinelinux$ sudo chroot alpinelinux sh/ # lsbindevetchomelibmediamntoptprocrootrunsbinsrvsystmpusrvar/ # cat /etc/hostnamelocalhost/ # cat /etc/passwdroot:x:0:0:root:/root:/bin/ashbin:x:1:1:bin:/bin:/sbin/nologindaemon:x:2:2:daemon:/sbin:/sbin/nologinadm:x:3:4:adm:/var/adm:/sbin/nologinlp:x:4:7:lp:/var/spool/lpd:/sbin/nologinsync:x:5:0:sync:/sbin:/bin/syncshutdown:x:6:0:shutdown:/sbin:/sbin/shutdownhalt:x:7:0:halt:/sbin:/sbin/haltmail:x:8:12:mail:/var/mail:/sbin/nologinnews:x:9:13:news:/usr/lib/news:/sbin/nologinuucp:x:10:14:uucp:/var/spool/uucppublic:/sbin/nologinoperator:x:11:0:operator:/root:/sbin/nologinman:x:13:15:man:/usr/man:/sbin/nologinpostmaster:x:14:12:postmaster:/var/mail:/sbin/nologincron:x:16:16:cron:/var/spool/cron:/sbin/nologinftp:x:21:21::/var/lib/ftp:/sbin/nologinsshd:x:22:22:sshd:/dev/null:/sbin/nologinat:x:25:25:at:/var/spool/cron/atjobs:/sbin/nologinsquid:x:31:31:Squid:/var/cache/squid:/sbin/nologinxfs:x:33:33:X Font Server:/etc/X11/fs:/sbin/nologingames:x:35:35:games:/usr/games:/sbin/nologincyrus:x:85:12::/usr/cyrus:/sbin/nologinvpopmail:x:89:89::/var/vpopmail:/sbin/nologinntp:x:123:123:NTP:/var/empty:/sbin/nologinsmmsp:x:209:209:smmsp:/var/spool/mqueue:/sbin/nologinguest:x:405:100:guest:/dev/null:/sbin/nologinnobody:x:65534:65534:nobody:/:/sbin/nologin/ # whoamiroot/ # exittest@jungnas:~/container/alpinelinux$
```

보는 내용과 같이 실제 호스트 운영체제와는 별도의 배포판처럼 동작하는 것을 확인할 수 있습니다. 다만 chroot 는 프로세스가 상위 루트를 볼수있는 현상이 있어 실제 컨테이너 구현에서는 chroot 보다는 pivot_root 를 더 선호합니다.

## 리눅스 Namespace

리눅스에서는 특정 프로세스를 네임스페이스에 넣게되면 그 네임스페이스에서 허용하는 것만 볼 수 있는 기능인 namespace 기능을 지원합니다. 리눅스에서 현재 동작중인 네임스페이스를 보고싶으면 다음 명령어를 통해 확인 가능합니다.

```bash
test@jungnas:~$ lsnsNS TYPENPROCSPID USER COMMAND4026531834 time2 2949039 test bash4026531835 cgroup2 2949039 test bash4026531836 pid2 2949039 test bash4026531837 user2 2949039 test bash4026531838 uts2 2949039 test bash4026531839 ipc2 2949039 test bash4026531840 net2 2949039 test bash4026531841 mnt2 2949039 test bash
```

이 기능을 통해 리눅스의 여러 기능을 격리시켜 이용할 수 있습니다. 이때 네임스페이스를 변경하기 위해서는 `unshare` 라는 명령어를 이용합니다. `man unshare` 명령을 통해 어떻게 생긴 명령어인지 확인해봅시다

**NAME**

unshare - run program in new namespaces

**SYNOPSIS**

unshare [options] [program [arguments]]

**DESCRIPTION**

The unshare command creates new namespaces (as specified by the command-line options described below) and then executes the specified program. If program is not given, then "${SHELL}" is run (default: /bin/sh).

설명을 보게 되면 unshare 명령은 아래에 설명된 명령줄 옵션에 지정된 대로 새 네임스페이스를 만듭니다. 새 네임스페이스를 만든 다음 지정된 프로그램을 실행합니다. 라는 설명을 확인할 수 있습니다. 격리 대상은 pid 부터 네트워크 까지 거의 모든 대상이지만 이번 예시에서는 pid, cgroup 를 격리시켜 컨테이너로 만들어보겠습니다.

## PID 네임스페이스

컨테이너 내부에서 ps 명령을 실행해보신 경우 다음과 같이 pid 가 항상 1 인것을 확인 할 수 있습니다.

```bash
/ # ps -efPIDUSERTIMECOMMAND1 root0:00 nginx: master process nginx -g daemon off;30 nginx0:08 nginx: worker process31 nginx0:08 nginx: worker process32 root0:00 sh38 root0:00 ps -ef
```

이는 pid 를 호스트와 격리시켜 개별적인 네임스페이스를 적용했기 때문입니다. unshare 명령을 이용해 pid 에 새로운 네임스페이스를 만들어 실행시키는 명령어는 다음과 같습니다.

```bash
sudo unshare --pid 〈command〉
```

여태가지 설명에 따르면 위 명령을 이용해 sh 커맨드를 새로운 네임스페이스에서 실행시키면 바로 pid 1 로 sh 명령이 실행될것으로 보입니다. 실제로도 그런지 한번 테스트 해봅시다

```bash
ubuntu@ip-10-1-1-227:~/container$ sudo unshare --pid sh# lsalpinelinux# lssh: 2: Cannot fork# lssh: 3: Cannot fork
```

뭔가 이상합니다 pid 검사를 하지도 않았는데 이상하게 에러가 발생합니다. 해당 내용의 원인은 sh 프로세스에 있습니다. `sudo unshare --pid sh` 명령에서 sh 프로세스의 부모 프로세스는 unshare 가 되어야하지만 sudo 가 부모로 된것이 원인입니다. 이를 해결하기 위해서는 —fork 옵션을 추가해주면 됩니다.

```bash
ubuntu@ip-10-1-1-227:~/container$ sudo unshare --pid --fork sh# psPID TTYTIME CMD1339 pts/100:00:00 sudo1340 pts/100:00:00 unshare1341 pts/100:00:00 sh1342 pts/100:00:00 ps# psPID TTYTIME CMD1339 pts/100:00:00 sudo1340 pts/100:00:00 unshare1341 pts/100:00:00 sh1343 pts/100:00:00 ps
```

fork 문제는 해결되었지만 아직 이상한점이 남아있습니다. 우리는 컨테이너와 같이 pid 가 1로 표현되는 부분을 보고싶으나 1339 와 같이 이상하게 보이고 있습니다. 이 현상의 원인은 ps 명령어를 살펴보면 알수잇습니다.

This ps works by reading the virtual files in /proc. This ps does not need to be setuid kmem or have any privileges to run. Do not give this ps any special permissions.

ps 명령어는 /proc 디렉토리를 읽어 표현해주는것이 원인이였습니다. 우리는 chroot 명령을 통해 루트 디렉토리를 변경하는 방법을 알고있습니다. chroot 를 통해 루트를 변경하고 /proc 디렉토리를 추가 마운트 하여 ps 명령을 쳐보도록 하겠습니다.

```bash
ubuntu@ip-10-1-1-227:~/container$ sudo unshare --pid --fork chroot alpinelinux sh/ # mount -t proc proc proc/ # psPIDUSERTIMECOMMAND1 root0:00 sh3 root0:00 ps
```

성공적으로 pid 가 격리된것을 보실 수 있습니다.

## Cgroup

cgroup이란 *Control groups* 를 줄인 말로 주어진 그룹에 속한 프로세스들이 사용할수있는 자원을 제한하는 기능입니다. 해당 기능을 통하여 위에서 만든 컨테이너가 ubuntu 22.04 에서는 cgroup v2 가 사용됩니다. 따라서 다음 명령어를 따라 가면 cpu 사용량을 제한할 수 있습니다

1. 먼저 cgroup 을 사용하기 위해서는 다음 패키지 설치가 필요합니다.

```bash
sudo apt-get install cgroup-tools
```

2. 먼저 cpu 및 cpu set 컨트롤러를 /sys/fs/cgroup/cgroup.controllers 파일에서 사용할 수 있는지 확인합니다.

```bash
cat /sys/fs/cgroup/cgroup.controllers
```

해당 명령어 실행시 다음과 같은 내용 출력시 정상적으로 사용 가능한 상황입니다.

```bash
cpuset cpu io memory hugetlb pids rdma
```

3. CPU 관련 컨트롤러를 활성화합니다

```bash
echo "+cpu" >> /sys/fs/cgroup/cgroup.subtree_controlecho "+cpuset" >> /sys/fs/cgroup/cgroup.subtree_control
```

해당 명령어는 /sys/fs/cgroup 의 하위 그룹에 대해 cpu, cpuset 컨트롤러를 사용할수 있습니다.

4. /sys/fs/cgroup 하위에 Example 이라는 하위 그룹을 생성합니다.

```bash
mkdir /sys/fs/cgroup/Example/
```

해당 폴더를 만들면 하위에 자동적으로 많은 파일들이 생성된것을 확인하실 수 있습니다.

```bash
ubuntu@ip-10-1-1-227:~/container$ ls /sys/fs/cgroup/Example/cgroup.controllerscpu.max.burstio.prio.classmemory.reclaimcgroup.eventscpu.pressureio.statmemory.statcgroup.freezecpu.statio.weightmemory.swap.currentcgroup.killcpu.uclamp.maxmemory.currentmemory.swap.eventscgroup.max.depthcpu.uclamp.minmemory.eventsmemory.swap.highcgroup.max.descendantscpu.weightmemory.events.localmemory.swap.maxcgroup.pressurecpu.weight.nicememory.highmemory.zswap.currentcgroup.procscpuset.cpusmemory.lowmemory.zswap.maxcgroup.statcpuset.cpus.effectivememory.maxpids.currentcgroup.subtree_controlcpuset.cpus.partitionmemory.minpids.eventscgroup.threadscpuset.memsmemory.numa_statpids.maxcgroup.typecpuset.mems.effectivememory.oom.grouppids.peakcpu.idleio.maxmemory.peakcpu.maxio.pressurememory.pressure
```

이 파일들은 활성화된 컨트롤러로 기본적으로 새로 생성된 하위 그룹은 제한 없이 모든 시스템의 CPU 및 메모리 리소스에 대한 액세스를 상속합니다.

5. CPU 관련 컨트롤러를 활성화하여 CPU와만 관련된 컨트롤러를 가져옵니다.

```bash
echo "+cpu" >> /sys/fs/cgroup/Example/cgroup.subtree_controlecho "+cpuset" >> /sys/fs/cgroup/Example/cgroup.subtree_control
```

해당 커맨드를 통해 cpu 타임을 컨트롤 하는 컨트롤러만 사용할수있습니다.

6. /sys/fs/cgroup/Example/tasks/ 디렉토리를 생성후 cpu 컨트롤러를 활성화 합니다.

```bash
mkdir /sys/fs/cgroup/Example/tasks/echo "1" > /sys/fs/cgroup/Example/tasks/cpuset.cpus
```

해당 디렉토리는 밑에 실제 cpu를 제한할 작업들을 넣는데 사용하게 됩니다.

7. CPU 시간 분배 제어를 설정하여 /sys/fs/cgroup/Example/tasks 하위 그룹의 모든 프로세스가 1초마다 0.2초 동안만 CPU에서 실행될 수 있습니다. 즉, 1초의 5분의 1이 됩니다

```bash
echo "200000 1000000" > /sys/fs/cgroup/Example/tasks/cpu.max
```

8. 이후 chroot 를 pid 분리하여 cpu 에 부하를 줄수있는 명령어를 실행합니다.

```bash
sudo unshare --pid --fork chroot alpinelinux `for i in 1; do while : ; do : ; done & done`
```

위 명령어는 chroot 로 루트 격리, unshare —pid 로 pid 까지 격리시킨 후 `for i in 1; do while : ; do : ; done &amp; done` 명령을 실행하는 명령어입니다. 위 명령어는 cpu 1코어를 사용하게 됩니다.9. 명령어를 실행 후 unshare 명령의 pid 를 찾아낸후 다음 명령을 입력합니다.

```bash
echo> /sys/fs/cgroup/Example/tasks/cgroup.procs
```

10. 이후 top 명령을 실행하게 되면 cpu 가 제한된 부분을 확인하실 수 있습니다.

## 참고자료

   - [24장. cgroups-v2를 사용하여 애플리케이션의 CPU 시간 분배 제어](https://access.redhat.com/documentation/ko-kr/red_hat_enterprise_linux/8/html/managing_monitoring_and_updating_the_kernel/using-cgroups-v2-to-control-distribution-of-cpu-time-for-applications_managing-monitoring-and-updating-the-kernel#doc-wrapper)
   - 컨테이너 보안 - 리즈 라이스 지음

