### - Private Cloud 구축을 위한 Openstack
----------------------------------------------------------------------
주제2 - packstack을 이용하여 two-nodes openstack 을 구축합니다.
Controller-10.0.0.200 (6G,2cpus)
Compute1-10.0.0.201 (2G,2cpus)
구축 후 glance의 backend stores를 swift와 연결합니다.(교재를 참조)
Dashboard에 연결하여 cirros 이미지를 등록하고
swift 사용자로 로그인하여 저장된 container를 확인합니다.

다음은 packstack answerfile의 요구사항입니다.

```
CONFIG_DEFAULT_PASSWORD=abc123
CONFIG_CEILOMETER_INSTALL=n
CONFIG_AODH_INSTALL=n
CONFIG_KEYSTONE_ADMIN_PW=abc123
CONFIG_HEAT_INSTALL=y
CONFIG_MAGNUM_INSTALL=y
CONFIG_TROVE_INSTALL=y
CONFIG_NEUTRON_L2_AGENT=openvswitch
CONFIG_NEUTRON_OVS_BRIDGE_IFACES=br-ex:ens33
COMPUTE_NODES=10.0.0.200,10.0.0.201
```



### controller 설치 

- VMware workstation에서 `contoller-base-10.0.0.100.vmdk`와 `CentOS-7-x86_64-DVD-1611.iso`를 사용하여 설치

- ```shell
  $vi /etc/sysconfig/network-scripts/ifcfg-ens33// ipaddr을 10.0.0.200으로 변경
  ```

- xshell에서 10.0.0.200으로 접속

- timestamp 형식으로 관리 -시간이 안맞으면 fail

```
#timedatectl 또는 #date  ntpdate / rdate 타임 동기화
 
# yum install -y  chrony -y
# yum install -y ntpdate
#date
#ntpdate 2.kr.pool.ntp.org
# date
# systemctl start chronyd 
# systemctl enable chronyd 
# chronyc sources
#systemctl restart chronyd  //127.127.1.0
```

- openstack 설치

```
#packstack --gen-answer-file=/root/packstack.txt
#vi /root/packstack.txt 
CONFIG_DEFAULT_PASSWORD=abc123
CONFIG_CEILOMETER_INSTALL=n
CONFIG_AODH_INSTALL=n
CONFIG_KEYSTONE_ADMIN_PW=abc123
CONFIG_HEAT_INSTALL=y
CONFIG_MAGNUM_INSTALL=y
CONFIG_TROVE_INSTALL=y
CONFIG_NEUTRON_L2_AGENT=openvswitch
CONFIG_NEUTRON_OVS_BRIDGE_IFACES=br-ex:ens33
COMPUTE_NODES=10.0.0.200,10.0.0.201

```

- controller 준비작업

```
-controller 준비작업
#systemctl stop firewalld
#systemctl disable firewalld
#systemctl disable 	NetwrokManager
#systemctl stop NetworkManager
-openstack repo 등록
#yum install -y centos-release-openstack-rocky
#yum repolist
-packstck 설치
#yum install -y openstack-packstack
#time packstack --answer-file=/root/openstack.txt
#cd /etc/sysconfig/network-scripts/
#vi ifcfg-br-ex //DNS 내용 추가
```



