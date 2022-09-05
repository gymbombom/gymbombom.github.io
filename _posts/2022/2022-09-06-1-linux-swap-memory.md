---
layout: post
title: Linux swap 메모리 설정
tags :
    - swap
    - 스왑
    - linux
---

#### swap 메모리란?
* 메모리(RAM)가 가득 차고 더 많은 메모리가 필요할 때 디스크 공간(가상 메모리)으로 대체
* 하드디스크를 이용하기 때문에 속도 면에서는 현저히 떨어짐.

---

#### swap Memory 설정방법
* swapMemory 확인
```shell
# swapMemory 설정 상태를 확인한다.(아래 command 3개중 하나로)
$ swapon -s;
$ free;
$ cat /proc/swaps;
```
---

* swap파일 생성
```shell
# sudo dd if=/dev/zero of=<swapfilePath> bs=1024 count=<fileSize>
# /var/swapfile 경로의 파일로 1G swapfile 을 생성한다. (count 는 KB단위)
$ sudo dd if=/dev/zero of=/var/swapfile bs=1024 count=1000000;
```
---

* swapFile 로 포멧
```shell
# mkswap <swapfilePath>
$ sudo mkswap /var/swapfile;
```
---

* swapFile 권한변경
```shell
$ sudo chmod 0600 /var/swapfile;
```
---

* swapFile 활성화
```shell
# sudo swapon <swapfilePath>
$ sudo swapon /var/swapfile;
```
---

#### 사용예
* swapFile 비활성화
```shell
# swapoff <swapfilePath>
$ swapoff /home/swapfile;
```
---

* 재부팅시에도 swap 파일 활성화
```shell
# echo "swapon <swapfilePath>" >> /etc/rc.d/rc.local;
$ echo "swapon /var/swapfile" >> /etc/rc.d/rc.local;
```
---

#### Experience
* 라즈베리파이3에서 swap memory 설정

```shell
# sudo vi /etc/dphys-swapfile

# swapfile위치설정
CONF_SWAPFILE=/var/swapfile

#swap size설정(MB단위)
CONF_SWAPSIZE=4000

# max swap size 설정(MB단위)
CONF_MAXSWAP=4000
```
---

#### Links
---