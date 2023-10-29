# Linux 기초
1. AWS Free-tier 계정으로 EC2 Amazon Linux2 인스턴스(t2a.micro)를 생성하고 ssh로 접속해보세요.
   ```bash
   The authenticity of host 'ec2-3-37-88-120.ap-northeast-2.compute.amazonaws.com (3.37.88.120)' can't be established.
   ED25519 key fingerprint is SHA256:2ocO3eflRnP1uZELq3qaPvib+rzvf100R/JvpnjNEbQ.
   This key is not known by any other names
   Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
   Warning: Permanently added 'ec2-3-37-88-120.ap-northeast-2.compute.amazonaws.com' (ED25519) to the list of known hosts.
   ,     #_
   ~\_  ####_        Amazon Linux 2023
   ~~  \_#####\
   ~~     \###|
   ~~       \#/ ___   https://aws.amazon.com/linux/amazon-linux-2023
   ~~       V~' '->
    ~~~         /
      ~~._.   _/
         _/ _/
       _/m/'
    ```
    - 위 sshd 접속 과정을 설명해주세요.
    - 퍼블릭 키는 어디에 있나요? : 클라이언트가 소유
    - 리눅스 내부에서 접속 포트 번호를 22에서 2022로 변경하려면 어떻게 할까요?
    sudo vim /etc/ssh/sshd_config
# If you want to change the port on a SELinux system, you have to tell
# SELinux about this change.
# semanage port -a -t ssh_port_t -p tcp #PORTNUMBER
#
# Port 22 
-> 21번째줄 Port 22의 주석을 제거 후 2022로 변경한다.
-> sudo systemctl restart sshd 사용하여 sshd 재시작
korwoo@DESKTOP-VB8PEDI:~$ ssh -i "ssibal.pem" ec2-user@ec2-3-37-88-120.ap-northeast-2.compute.amazonaws.com -p 2022
,     #_
~\_  ####_        Amazon Linux 2023
~~  \_#####\
~~     \###|
~~       \#/ ___   https://aws.amazon.com/linux/amazon-linux-2023
2. 현재 사용 중인 리눅스의 파일 시스템을 조회하는 명령어를 입력하고 결과를 작성해주세요.
    ```
   [ec2-user@ip-172-31-32-62 ~]$ df -T
   Filesystem     Type     1K-blocks    Used Available Use% Mounted on
   devtmpfs       devtmpfs      4096       0      4096   0% /dev
   tmpfs          tmpfs       486100       0    486100   0% /dev/shm
   tmpfs          tmpfs       194440    2896    191544   2% /run
   /dev/xvda1     xfs        8310764 1550768   6759996  19% /
   tmpfs          tmpfs       486100       0    486100   0% /tmp
   /dev/xvda128   vfat         10202    1306      8896  13% /boot/efi
   tmpfs          tmpfs        97220       0     97220   0% /run/user/1000
   ```
3. 최상위 루트 디렉토리('/')의 하위 디렉토리를 간략하게 설명해주세요.
    - /bin : ls, cp, mv 와 같은 기본 명령어들의 바이너리 파일이 위치
    - /dev : 물리적 하드웨어를 다루기 위한 파일
    - /etc : 시스템 전반의 설정 파일 저장 (Ex 네트워크 설정 daemon 설정 등)
    - /lib : 시스템 라이브러리(실행파일을 동작시키기 위해 필요한 코드 모음) 저장
    - /mnt : 외부 장치를 마운트 할떄 사용되는 디렉터리
    - /proc : 동작중인 프로세스 정보와 커널 정보가 업데이트되며 보관되는 가상 파일시스템
    - /usr : 사용자 관련 프로그램과 데이터 보관
    - /var : 변경 가능한 데이터를 저장하는곳(로그파일, 메일 큐 등)
4. 현재 사용 중인 쉘과 사용 가능한 쉘의 목록을 확인하는 명령어를 각각 입력해주세요.
    ```
    [ec2-user@ip-172-31-32-62 ~]$ echo $SHELL
    /bin/bash
    [ec2-user@ip-172-31-32-62 ~]$ cat /etc/shells
    /bin/sh
    /bin/bash
    /usr/bin/sh
    /usr/bin/bash
    /bin/csh
    /bin/tcsh
    /usr/bin/csh
    /usr/bin/tcsh
    ```
    - 쉘이란 무엇일까요? : 사용자와 운영 체제 사이의 인터페이스 역할을 담당하는 소프트웨어
5. ls 명령어를 입력하면 현재 디렉토리 내 파일과 디렉토리의 목록을 반환합니다. 해당 명령어의 입력부터 출력까지의 과정을 설명해주세요.
    > 작성 키워드 : **fork & exec(system call)**
    - system call이란 무엇일까요?
6. ps -ef | grep "/bin/sh" 명령어를 입력해보시고, 파이프라인('**|**') 문자가 어떤 기능인지 설명해주세요.
    - 마찬가지로 ls | sort | cd /home/ec2-user 명령어를 입력하고, 어떤 내용이 출력되는지 작성해주세요.
        - 만약 출력되지 않는다면 그 이유를 설명해주세요.
7. pstree 명령어를 입력해주세요.
    - 최상단 프로세스(init 또는 systemd)의 역할은 무엇인지 설명해주세요.
    - init과 systemd의 차이점을 설명해주세요.
8. 네이버(www.naver.com)의 IP 주소를 확인하는 명령어는 무엇인가요?
    - 실행 결과(IP 주소)를 받는 과정을 설명해주세요(DNS 질의 과정)
    - /etc/hosts와 /etc/resolv.conf의 차이점을 설명해주세요.
9. curl, telnet, ping의 차이점을 설명해주세요.
10. yum 패키지를 통해 httpd를 설치하고, 80 포트를 개방하여 서비스를 실행해주세요.
    - yum과 apt의 차이점은 무엇인가요?
    - httpd 포트 상태(LISTEN…)를 확인하는 명령어를 입력하고 결과를 작성해주세요.
11. 다음은 리소스 모니터링을 수행하는 명령어 중 top 명령어를 수행한 결과입니다. 각각의 값이 무엇을 의미하는지 설명해주세요.
    ```bash
    $top
    top - 22:39:40 up 0 min,  0 users,  load average: 0.00, 0.00, 0.00
    Tasks:   5 total,   1 running,   4 sleeping,   0 stopped,   0 zombie
    %Cpu(s):  0.0 us,  0.0 sy,  0.0 ni,100.0 id,  0.0 wa,  0.0 hi,  0.0 si,  0.0 st
    MiB Mem :   6948.5 total,   6844.0 free,     75.0 used,     29.5 buff/cache
    MiB Swap:      0.0 total,      0.0 free,      0.0 used.   6733.4 avail Mem

    PID USER     PR  NI    VIRT    RES    SHR  S  %CPU  %MEM     TIME+  COMMAND
    1   root     20   0    1276    788    472  S   0.0   0.0   0:00.03  init
    14  root     20   0    1284    388     20  S   0.0   0.0   0:00.00  init
    15  root     20   0    1284    396     20  S   0.0   0.0   0:00.00  init
    16  eljoelee 20   0    6200   5028   3316  S   0.0   0.1   0:00.02  bash
    27  eljoelee 20   0    7788   3268   2904  R   0.0   0.0   0:00.00  top
    ```
    - load average
    - tasks
        - sleeping
        - zombie
    - Cpu(s)
        - hi
        - si
    - MiB
        - buff/cache
    - PR
    - VIRT
    - RES
    - SHR
    - MEM
12. 메모리 사용량을 확인하는 명령어를 입력하고 결과를 작성해주세요.
    - swap이란 항목은 무엇을 의미하는 걸까요?
        - 2GB 가량의 swap 메모리를 설정하고 메모리 사용량을 확인하는 명령어를 통해 결과를 작성해주세요.
