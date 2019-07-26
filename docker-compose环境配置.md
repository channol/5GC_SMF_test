# docker-compose脚本环境
使用docker-compose脚本启动docker containers，管理配置及命令触发
# 配置复制
- Copy ../sm/tools directory to your test machine i.e
```
cp ../sm/tools/* ~/test/.
```
- you can copy dcomp to /usr/sbin/dcomp, so don’t need specify path for using dcomp

- Create config directory under ~/test/
```
copy ../sm/cfg/config.json to ~/test/config/smf_cfg.json
copy ../pfcp/config/config.json to ~/test/config/pcfp_cfg.json
copy  uemgr/n2mgr/sctpmgr/nrf config to containers
```
- 各个模块配置经常变化，需要和image更新同步
```
{
 "svc_fqdn": "smfsm",
  "svc_port": 80,
  "smf_id": "smfsm",
  "db_addr": "redis",
  "db_port": 6379,
  "etcd_addr": "etcd",
  "etcd_port": 2379,
  "n4_port": 8805,
  "pfcp_svc_fqdn": "pfcp",
  "pfcp_svc_port": 50051,
  "pfcp_port": 50051,
  "udm_svc_fqdn": "udm",
  "udm_svc_port": 80,
  "log_level": "debug",
  "procedure_timer":1000,
  "slice_infos": [{
    "sNssai": {
      "sst": 1,
      "sd": "internet"
    },
    "dnns": [{
      "dnn": "inet1",
      "upf_infos": [{
        "upf_instance_id": "static-upf1",
        "upf_addr": "172.18.0.5",
        "ip_pool": [{
          "name": "v4_pool",
          "cidr": "11.11.0.1/16"
        }]
      }],
      "pcf_svc_fqdn": "pcf1",
      "pcf_port": 8080,
      "chf_svc_fqdn": "chf1",
      "chf_port": 5848
    }]
  },{
    "sNssai": {
      "sst": 1,
      "sd": "123ABC"
    },
    "dnns": [{
      "dnn": "inet1",
      "upf_infos": [{
        "upf_instance_id": "static-upf1",
        "upf_addr": "172.18.0.5",
        "ip_pool": [{
          "name": "v4_pool",
          "cidr": "11.11.0.1/16"
        }]
      }],
      "pcf_svc_fqdn": "pcf1",
      "pcf_port": 8080,
      "chf_svc_fqdn": "chf1",
      "chf_port": 5848
    }]
  }, {
    "sNssai": {
      "sst": 11,
      "sd": "sd"
    },
    "dnns": [{
      "dnn": "internet",
      "upf_infos": [{
        "upf_instance_id": "static-upf2",
        "upf_addr": "172.18.0.5",
        "ip_pool": [{
          "name": "v4_internet",
          "cidr": "10.10.0.1/24"
        }]
      }],
      "pcf_svc_fqdn": "pcf1",
      "pcf_port": 8080,
      "chf_svc_fqdn": "chf1",
      "chf_port": 5848
    }]
  }, {
    "sNssai": {
      "sst": 12,
      "sd": "sd"
    },
    "dnns": [{
      "dnn": "URLLC",
      "upf_infos": [{
        "upf_instance_id": "static-upf3",
        "upf_addr": "172.18.0.5",
        "ip_pool": [{
          "name": "v4_URLLC",
          "cidr": "10.20.0.1/24"
        }]
      }],
      "pcf_svc_fqdn": "pcf1",
      "pcf_port": 8080,
      "chf_svc_fqdn": "chf1",
      "chf_port": 5848
    }, {
      "dnn": "eMBB",
      "upf_infos": [{
        "upf_instance_id": "static-upf4",
        "upf_addr": "172.18.0.5",
        "ip_pool": [{
          "name": "v4_eMBB",
          "cidr": "10.30.0.1/24"
        }]
      }],
      "pcf_svc_fqdn": "pcf1",
      "pcf_port": 8080,
      "chf_svc_fqdn": "chf1",
      "chf_port": 5848
    }]
  }],
  "ltmap": [{
      "locality": "DC-1",
      "taiList": [{
        "plmnId": {
          "mcc": "460",
          "mnc": "01"
        },
        "tac": "ABCD11"
      }, {
        "plmnId": {
          "mcc": "460",
          "mnc": "01"
        },
        "tac": "ABCD22"
      }]
    },
    {
      "locality": "DC-2",
      "taiList": [{
        "plmnId": {
          "mcc": "460",
          "mnc": "01"
        },
        "tac": "ABCD33"
      }, {
        "plmnId": {
          "mcc": "460",
          "mnc": "01"
        },
        "tac": "ABCD44"
      }]
    },
    {
      "locality": "DC-3",
      "taiList": [{
        "plmnId": {
          "mcc": "222",
          "mnc": "200"
        },
        "tac": "24B"
      }, {
        "plmnId": {
          "mcc": "222",
          "mnc": "200"
        },
        "tac": "25"
      }]
    }
  ]
}
``` 
- pfcp_cfg.json
``` 
{
    "smf_svc_fqdn": "smfsm",
    "smf_svc_port": 50051,
    "etcd_addr": "etcd",
    "etcd_port": 2379,
    "db_addr": "redis",
    "db_port": 6379,
    "metric_path": "/metrics",
    "metric_port": 80,
    "log_level": 2,
    "pfcp": {
        "ip": "172.17.0.2",
        "publicip": "12.12.12.10",
        "port": 8805,
        "tmr":{
            "t1": 1000,
            "n1": 3,
            "t2": 4000,
            "gd": 4000,
            "hb": 5000
        }
    }
}
```

- Create script directory under ~/test/
- Copy ../sm/cfg/*.lua to ~/test/script/.
- Copy ../pfcp/config/*.lua to ~/test/script/.

#  修改docker-compose.yml配置
## 配置文件根据需求做了新的改动
- 集成全部产品类images
- nrf使用配套的redis-cluster，集群db模式
- ausf使用sim-ausf名字，因为代码有hardcode
- amf多个模块也需要不同配置
- sim-gnb建立2个，一个跑gnb，另一个跑cpe
- container模块启动增加一定时序，ip分配及流程更合理
- modify ~/test/docker-compose.yml as the following:
``` 
# Container definitions used for SMF integration testing
# Copyright (C) 2019 Casa Systems, Inc.  All Rights Reserved.
# Use this file in conjunction with the "dcomp" shell script.

version: '3'

# Containers all use a network defined by the environment variable ${RNDNET}
# The value of ${RNDNET} must be unique on the docker host and must not
# conflict with any other network that must be reachable from the host.
networks:
  default:
    ipam:
      driver: default
      config:
        - subnet: ${RNDNET}

# Each docker container is defined as a named service.
# The service identifies the image to be run, the command (if any) to be executed,
# and other parameters necessary to bring up the container.
#
# Note that each container defines a security context. The security context defined
# here disables apparmor protections so that the root user can terminate container
# processes that are malfunctioning. With the default context, even the root user
# cannot stop bad containers without restarting the docker service.
services:

  redis:
    image: ${REDIS_IMG:-redis}
    command: redis-server
    security_opt:
      - apparmor:unconfined

  rediscluster:
    #image: "grokzen/redis-cluster"
    image: ${REDISCLUSTER_IMG:-redis-cluster}
    #container_name: nrf-redis-cluster
    ports:
      - 7000:7000
    depends_on:
      - redis

  etcd:
    image: ${ETCD_IMG:-quay.io/coreos/etcd}
    command: etcd -listen-client-urls http://0.0.0.0:2379 -advertise-client-urls http://etcd-srv:2379
    security_opt:
      - apparmor:unconfined
    depends_on:
      - rediscluster

  pfcp:
    image: ${PFCP_IMG:-registry.gitlab.casa-systems.com/mobility/smf/pfcp}
    # Privileged containers have access to certain host resources.
    privileged: true
    # Services that require interactive shells must include the following two lines.
    stdin_open: true
    tty: true
    volumes:
      # Mount a local file to the container. The local file path is based on
      # the current working directory.
      - "./config/pfcp_cfg.json:/etc/config/config.json"
      - "./script:/cfg"
    command: /opt/casa/pfcp/pfcp
    security_opt:
      - apparmor:unconfined

        #  upf:
        #    image: ${UPF_IMG:-upf}
        #    command: sim_upf pfcp 8805
        #    security_opt:
        #      - apparmor:unconfined
    depends_on:
      - sctpmgr

  udm:
    image: ${UDM_IMG:-udm}
    security_opt:
      - apparmor:unconfined
    depends_on:
      - nrf

  smfsm:
    image: ${SMFSM_IMG:-sm}
    volumes:
      # Mount a local file to the container.
      - "./config/smf_cfg.json:/etc/config/config.json"
      #- "./config/450udm:/opt/casa/smf/sm/450udm"
      - "./script:/cfg"

    command: ./smfsm
    security_opt:
      - apparmor:unconfined
    depends_on:
      - sctpmgr
      - udm
      - nrf

#  amf:
#    image: ${AMF_IMG:-amf}
#    volumes:
#      # Mount a local file to the container.
#      - "./config/sim_amf_cfg.json:/etc/config/sim_amf_cfg.json"
#    privileged: true
#    stdin_open: true
#    tty: true
#    command: bash
#    security_opt:
#      - apparmor:unconfined

  nrf:
    image: ${NRF_IMG:-sim-nrf}
    #image: registry.gitlab.casa-systems.com/mobility/sim-nrf:test-123
    #container_name: root-test_nrf_1
    command: /usr/local/bin/sim-nrf -c /sim-nrf.yaml
    #command: /usr/local/bin/sim-nrf -c conf.ini
    volumes:
       - ./config/sim-nrf.yaml:/sim-nrf.yaml
      #- ./config/conf.ini:/conf.ini
    ports:
      - 80:80
    depends_on:
      #- redis-cluster
      - etcd

  sctpmgr:
    image: ${SCTPMGR_IMG:-sctpmgr}
    volumes:
      # Mount a local file to the container.
      - "./config/sctpmgr.json:/etc/config/config.json"
    security_opt:
      - apparmor:unconfined
    #ports:
    #  - 9093:9093
      #- "38412:38412"
    depends_on:
      - sim-ausf

  n2mgr:
    image: ${N2MGR_IMG:-n2mgr}
    volumes:
      # Mount a local file to the container.
      - "./config/n2mgr.json:/etc/config/config.json"
    security_opt:
      - apparmor:unconfined
    #ports:
      #- 9097:9097
    depends_on:
      - sctpmgr

  uemgr:
    image: ${UEMGR_IMG:-uemgr}
    volumes:
      # Mount a local file to the container.
      - "./config/uemgr.json:/etc/config/config.json"
    security_opt:
      - apparmor:unconfined
    #ports:
    #  - 9095:9095
    depends_on:
      - sctpmgr

  sim-ausf:
    image: ${AUSF_IMG:-sim-ausf}
    container_name: root-test_ausf_1
    #volumes:
      # Mount a local file to the container.
      #- "./config/sim-aush.json:/etc/config/config.json"
    security_opt:
      - apparmor:unconfined
    #ports:
      #- 9096
    depends_on:
      - nssf

  nssf:
    image: ${NSSF_IMG:-sim-nssf}
    #volumes:
      # Mount a local file to the container.
      #- "./config/sim-aush.json:/etc/config/config.json"
    command: sim-nssf -r redis
    depends_on:
      - redis
    security_opt:
      - apparmor:unconfined
    depends_on:
      - udm


  gnb:
    image: ${GNB_IMG:-sim-gnb}
    #environment:
      #N2IP: n2mgr
    command: sh
    #volumes:
      # Mount a local file to the container.
      #- "./config/sim-aush.json:/etc/config/config.json"
    security_opt:
      - apparmor:unconfined
    ports:
      - 65501:65501
    privileged: true
    stdin_open: true
    depends_on:
      - sctpmgr

  cpe:
    image: ${CPE_IMG:-sim-gnb}
    container_name: root-test_cpe_1
    command: sh
    security_opt:
      - apparmor:unconfined
    privileged: true
    stdin_open: true
    depends_on:
      - gnb

```

#  配置images文件
adjust images names in ~/test/imagenames   i.e. replace any container images on local machine instead of default pulling from gitlab.
``` 
export PFCP_IMG=registry.gitlab.casa-systems.com/mobility/smf/pfcp
export UDM_IMG=registry.gitlab.casa-systems.com/mobility/sim-udm
export SMFSM_IMG=registry.gitlab.casa-systems.com/mobility/smf/sm
export AMF_IMG=registry.gitlab.casa-systems.com/mobility/smf/sm/amf-sim
export UEMGR_IMG=registry.gitlab.casa-systems.com/mobility/amf/uemgr
export N2MGR_IMG=registry.gitlab.casa-systems.com/mobility/amf/n2mgr/n2mgr-bmo
export SCTPMGR_IMG=registry.gitlab.casa-systems.com/mobility/amf/sctpmgr
export GNB_IMG=registry.gitlab.casa-systems.com/mobility/sim-gnb:latest
export NSSF_IMG=registry.gitlab.casa-systems.com/mobility/sim-nssf
export NRF_IMG=registry.gitlab.casa-systems.com/mobility/sim-nrf
export AUSF_IMG=registry.gitlab.casa-systems.com/mobility/ausf/sim-ausf
export REDISCLUSTER_IMG=grokzen/redis-cluster
export CPE_IMG=registry.gitlab.casa-systems.com/mobility/sim-cpe
```
# 运行命令
start setup: run under ~/test directory:   (dcomp will use curl to configure udm automatically)

`dcomp up –d`
- 例子
```
yzhang@mec2-90:~/test$ dcomp up -d
using network 172.24.57.0/24
Creating network "yzhang-test_default" with the default driver
Creating yzhang-test_udm_1   ... done
Creating yzhang-test_pfcp_1  ... done
Creating yzhang-test_redis_1 ... done
Creating yzhang-test_upf_1   ... done
Creating yzhang-test_amf_1   ... done
Creating yzhang-test_etcd_1  ... done
Creating yzhang-test_smfsm_1 ... done
using network 172.24.57.0/24
*   Trying 172.24.57.2...
* TCP_NODELAY set
* Connected to udm (172.24.57.2) port 80 (#0)
> PUT /udm-configure/sdm/v1/12345678901234 HTTP/1.1
> Host: udm
> User-Agent: curl/7.52.1
> Accept: */*
> Content-Type: application/json
> Content-Length: 3364
> Expect: 100-continue
>
* Done waiting for 100-continue
* We are completely uploaded and fine
< HTTP/1.1 204 No Content
< content-length: 0
< date: Tue, 16 Jul 2019 20:49:33 GMT
<
* Curl_http_done: called premature == 0
* Connection #0 to host udm left intact
```
# udm数据注册
- 有时候脚本不发注册消息至udm，需要手动注册
- udm注册数据，注意imsi-45005xxxxxx格式，目前没有确定
- sst，dnn需要对比一直
命令：
```
curl --verbose -X PUT -H "Content-Type: application/json"  http://172.24.14.6:80/udm-configure/sdm/v1/imsi-450051234000000 -d '{"gpsi": "msisdn-54321","supi": "imsi-450051234000000", "subscriptionDataSets": {"amData": {"supportedFeatures":"support","gpsis":["gpsi1234","gpsi5678"],"internalGroupIds":["internalGroupId0","internalGroupId1"],"subscribedUeAmbr":{"uplink":"120 Mbps","downlink":"110 Mbps"},"nssai":{"supportedFeatures":"support1","defaultSingleNssais":[{"sst":1,"sd":"abd001"}],"singleNssais":[{"sst":2,"sd":"sd2345"}]},"ratRestrictions":["WLAN","VIRTUAL","NR"],"forbiddenAreas":[{"tacs":["tac1"],"areaCodes":["areaCodes1"]},{"tacs":["tac3","tac4"],"areaCodes":["areaCodes3","areaCodes4"]}],"serviceAreaRestriction":{"restrictionType":"ALLOWED_AREAS","areas":[{"tacs":["svcAreaTac1","svcAreaTac2"],"areaCodes":["svcAreaCodes1","svcAreaCodes2"]},{"tacs":["svcAreaTac3","svcAreaTac4"],"areaCodes":["svcAreaCodes3","svcAreaCodes4"]}],"maxNumOfTAs":123},"coreNetworkTypeRestrictions":["5GC", "EPC"],"rfspIndex":12,"subsRegTimer":13,"ueUsageType":14,"mpsPriority":true,"activeTime":15,"dlPacketCount":16,"sorInfo":{"steeringInfoList":[{"plmnId":{"mcc":"901","mnc":"31"},"accessTechList":["NR"]},{"plmnId":{"mcc":"902","mnc":"32"},"accessTechList":["NR","UTRAN"]}],"ackInd":true,"sorMacIausf":"sorMacIausf.val","countersor":"countersor.val"},"micoAllowed":true,"sharedDataIds":["sharedDataId1","sharedDataId2"]},"smfSelData":{"supportedFeatures":"SUPPORT","subscribedSnssaiInfos":{"1":{"dnnInfos":[{"dnn":"DNN","defaultDnnIndicator":true,"lboRoamingAllowed":true,"iwkEpsInd":true,"ladnIndicator":true}],"singleNssai":{"sst":1,"sd":"abd001"}}}},"uecSmfData":{"pduSessions":{"1":{"pduSessionId":1,"dnn":"dnn1","smfInstanceId":"smfId1","plmnId":{"mcc":"48","mnc":"902"}},"2":{"pduSessionId":2,"dnn":"dnn2","smfInstanceId":"smfId2","plmnId":{"mcc":"49","mnc":"902"}}},"pgwInfo":[{"dnn":"dnn1","pgwFqdn":"pgw1","plmnId":{"mcc":"50","mnc":"903"}},{"dnn":"dnn2","pgwFqdn":"pgw2","plmnId":{"mcc":"51","mnc":"903"}}]},"uecSmsfData":{"smsfInfo3GppAccess":{"smsfInstanceId":"smsfInstanceId.val","plmnId":{"mcc":"51","mnc":"901"}},"smsfInfoNon3GppAccess":{"smsfInstanceId":"smsfInstanceId.val","plmnId":{"mcc":"52","mnc":"902"}}},"smsSubsData":{"smsSubscribed":true,"sharedDataIds":["sharedDataId1","sharedDataId2"]},"smData":[{"singleNssai":{"sst":1,"sd":"sd2123"},"dnnConfigurations":{"inet1":{"dnn":"DNN1","pduSessionTypes":{"defaultSessionType":"IPV4","allowedSessionTypes":["IPV6","IPV4V6"]},"sscModes":{"defaultSscMode":"SSC_MODE_1","allowedSscModes":["SSC_MODE_2","SSC_MODE_3"]},"iwkEpsInd":true,"ladnIndicator":true,"_5gQosProfile":{"_5qi":171,"priorityLevel":2,"arp":{"priorityLevel":1,"preemptCap":"MAY_PREEMPT","preemptVuln":"NOT_PREEMPTABLE"}},"sessionAmbr":{"uplink":"101 Mbps","downlink":"102 Mbps"},"_3gppChargingCharacteristics":"TERISTICS","staticIpAddress":[{"ipv4Addr":"1.2.3.4","ipv6Addr":"1.2.3.4::","ipv6Prefix":"5.6.7.8::"}],"upSecurity":{"upIntegr":"REQUIRED","upConfid":"NOT_NEEDED"}}}, "internalGroupIds":["internalGroupId-1"]}],"traceData":{"traceRef":"traceRefVal","traceDepth":"MEDIUM","neTypeList":"EEAA","eventList":"22","collectionEntityIpv4Addr":"1.2.3.4","collectionEntityIpv6Addr":"1.2.3.4::","interfaceList":"ABCD"},"smsMngData":{"supportedFeatures":"supported","mtSmsSubscribed":true,"mtSmsBarringAll":true,"mtSmsBarringRoaming":true,"moSmsSubscribed":true,"moSmsBarringAll":true,"moSmsBarringRoaming":true,"sharedSmsMngDataIds":["sharedDataId1","sharedDataId2"]}}}'

```

 
# 检查状态
check status:

```
yzhang@mec2-90:~/test$ dcomp ps
using network 172.24.57.0/24
       Name                      Command               State         Ports      
---------------------------------------------------------------------------------
yzhang-test_amf_1     bash                             Up                       
yzhang-test_etcd_1    etcd -listen-client-urls h ...   Up      2379/tcp, 2380/tcp
yzhang-test_pfcp_1    ./pfcp                           Up                       
yzhang-test_redis_1   docker-entrypoint.sh redis ...   Up      6379/tcp         
yzhang-test_smfsm_1   /opt/casa/smfsm                  Up                       
yzhang-test_udm_1     /bin/sh -c ./start.sh            Up                       
yzhang-test_upf_1     sim_upf pfcp 8805                Up                
```

# 检查log
get logs i.e. for smfsm

``` 
yzhang@mec2-90:~/test$ dcomp logs -f smfsm
using network 172.24.57.0/24
Attaching to yzhang-test_smfsm_1
smfsm_1  | init nas parser........
smfsm_1  | NGAP Lib init (OSS context unused): 0x1fbf840
smfsm_1  | cfg_file:  config
smfsm_1  | stubUdm:  false
smfsm_1  | realPcf:  false
smfsm_1  | stubChf:  true
smfsm_1  | stubPfcp:  false
smfsm_1  | stubEtcd:  false
smfsm_1  | ngapStackInit()=0x1fc2bb0
smfsm_1  | NGAP Parser New: obj = 0xc0001d4880, handle = 0x1fc2bb0
smfsm_1  | Successful connection to redis
smfsm_1  | Successful connection to redis
smfsm_1  | {"level":"debug","casa":{"src":{"file":"upfmgr.go","function":"smfsm/pkg/upfmgr.NewUpfMgr","line":86}},"time":"2019-07-16T20:49:29Z","message":"NewUpfMgr: 2866834488"}
smfsm_1  | {"level":"debug","casa":{"src":{"file":"ipam.go","function":"smfsm/pkg/ipam.(*IpamRealEp).AddPoolCidr","line":123}},"time":"2019-07-16T20:49:29Z","message":"|||||||||||||||||||||||||duration|||||    : 0"}
smfsm_1  | {"level":"debug","casa":{"src":{"file":"ipam.go","function":"smfsm/pkg/ipam.(*IpamRealEp).AddPoolCidr","line":123}},"time":"2019-07-16T20:49:29Z","message":"|||||||||||||||||||||||||duration|||||    : 0"}
smfsm_1  | {"level":"debug","casa":{"src":{"file":"ipam.go","function":"smfsm/pkg/ipam.(*IpamRealEp).AddPoolCidr","line":123}},"time":"2019-07-16T20:49:29Z","message":"|||||||||||||||||||||||||duration|||||    : 0"}
smfsm_1  | {"level":"debug","casa":{"src":{"file":"ipam.go","function":"smfsm/pkg/ipam.(*IpamRealEp).AddPoolCidr","line":123}},"time":"2019-07-16T20:49:29Z","message":"|||||||||||||||||||||||||duration|||||    : 0"}
smfsm_1  | {"level":"debug","casa":{"src":{"file":"ipam.go","function":"smfsm/pkg/ipam.(*IpamRealEp).AddPoolCidr","line":123}},"time":"2019-07-16T20:49:29Z","message":"|||||||||||||||||||||||||duration|||||    : 0"}
smfsm_1  | pfcp server on [:50051]
```
# 启动sim-amf
- login to container i.e. to amf-sim container and run smfsim command:  (can use fqdn instead of IP address)
 
`dcomp exec amf bash`

- 例子
```
yzhang@mec2-90:~/test$ dcomp exec amf bash
using network 172.24.57.0/24
root@cb80abbfb3a0:/opt/casa# ./smfsim
init nas parser........
NGAP Lib init (OSS context unused): 0x1106840
input -h to see the usage
sim_amf>--proc Create --dnn inet1 --supi 12345678901234 --pdu 7 --host smfsm --amf amf
sim_amf>
procs [Create]
duration [0]
in func main: dnn: inet1 supi: 12345678901234 pdu: 7 host: smfsm amf: amf rate: 1 count: 1
==========time.Duration(delay) 0s
 
==========CreateSession.Supi 12345678901234
==========CreateSession count: 1
==========CreateSession httpRsp.Status 200 OK
 

==
--proc Create --dnn inet1 --supi 12345678901234 --pdu 7 --host smfsm --amf amf
```
# 启动sim-gnb
- sim-gnb可以设置sctp和udp不同tunnel ip
- -d 参数为udp ip
- sctp及udp ip可以相同，但此时端口不能相同
## set IMSI PLMN IP
```
root@ubuntu18:~/test# dcomp exec gnb sh
using network 172.24.14.0/24
/ # sim-gnb -h 38412 -p 38412 -d  10.27.0.1 -n 1 -u 1 -B 65501 -a 172.24.14.9
NGAP Server address 172.24.14.9 port 38412
 GNB number is  1 and per ue number is 1 ...
plmnID 54 f0 50
Using eth0 IPv4 address 172.24.14.13
Configure tools communicator listen socket on port 65501 and start listen thread
ue_per_gnb ==1
Server Running on sctp sfd 10
Server Running on udp sfd 11
default retransmit time is 0
gnbmgr_init succeeded
 diag> create cpe raw socket success on interface:eth0
set plmn 45005
snew plmnID 54 f0 50
 diag> et imsi mcc 450 mnc 05 msin 1234000000
set IMSI(mcc+mnc+msin):450051234000000
 diag> ule 0 0 7 1 inet1
 diag> r a
 diag> .ule 0 0 7 1 inet1
```
## set cpe
```
 diag> set cpe  172.24.14.15 0 0 7
gnb[0]ue[0] session[7] set cpe success
 diag>
 diag>
 diag>
 diag> show stat

gnb id 0 [SCTP connected][NGAP registered]  total UE = 1
  local sctp 172.24.14.13:38412. local udp 10.27.0.1:2152
 gnb-450-05-1-22-0000
    UE[0][state RM_REGISTERED CM_CONNECTED]
            ran_ue_ngap_id=0,    amf_ue_ngap_id=1
            imsi=450051234000000
              rab[7] - PDU address type 1, address 11.11.92.32, sst 1, dnn inet1
                local teid=00000007,   te ip=10.27.0.1
                remote teid=ff003cf0,   te ip=172.0.5.38
                QFI = 0, selected PDU session type 1, AMBR DL 102 kbps, UL 101 kbps
                QoS rules:
                  QRI: 1, QFI 0, qos_rule_pres 0, dqr 1, rule_oper_code 1 [create rule], n_pkt_filters 1:
                    filter id 1, dir 3 [bi], value


 >>>>> total connected gnb num = 1, ng setup num = 1, ue num = 1, registered ue = 1, connected ue 1, rab num = 1
 diag>
```

# 停止脚本
- stop setup:
```
yzhang@mec2-90:~/test$ dcomp down
using network 172.24.57.0/24
Stopping yzhang-test_etcd_1  ... done
Stopping yzhang-test_redis_1 ... done
Stopping yzhang-test_upf_1   ... done
Stopping yzhang-test_pfcp_1  ... done
Stopping yzhang-test_amf_1   ... done
Stopping yzhang-test_udm_1   ... done
Removing yzhang-test_etcd_1  ... done
Removing yzhang-test_redis_1 ... done
Removing yzhang-test_smfsm_1 ... done
Removing yzhang-test_upf_1   ... done
Removing yzhang-test_pfcp_1  ... done
Removing yzhang-test_amf_1   ... done
Removing yzhang-test_udm_1   ... done
Removing network yzhang-test_default
yzhang@mec2-90:~/test$
```

# NRF对接upf
## NRF注册curl
```
curl --verbos -d '{"nfInstanceID":"smf1", "nfType":"SMF", "nfStatus":"REGISTERED", "heartBeatTimer": 30, "sNssais":[{"sst":1,"sd":"abc"},{"sst":2},{"sst":3},{"sst":4}], "plmnList":[{"mcc":"460","mnc":"01"},{"mcc":"512","mnc":"018"}],"priority":1, "capacity":10, "load":10, "locality":"gz", "nsiList":["abc","bcd","efg"], "ipv4Addresses":["172.17.0.1"], "ipv6Addresses":["172:17:0::12/64"],"recoveryTime":"2019/7/17", "NFServices":[{"serviceInstanceID":"smf1"},{"serviceName":"smf1"},{"scheme":"http"},{"nfServiceStatus":"REGISTERED"},{"allowedNfTypes":["AMF","UPF","SMF"]}], "SmfInfo":[{"sNssaiSmfInfoList":[{"sst":1},{"sst":2,"sd":"abcd"}]},{"taiList":["1","2"]},{"pgwFqdn":"casa.com"},{"accessType":"wifi"}]}' -H "Content-Type:application/json" -X PUT http://172.0.5.27:8008/nnrf-nfm/v1/nf-instances/smf1
```

## 检查upf是否注册成功

```
curl --verbos -H "Content-Type:application/json" -X GET http://172.0.5.27:8008/nnrf-nfm/v1/nf-instances/container-name

```
## 返回结果
```
root@CASA-MOBILE:~# curl --verbos -H "Content-Type:application/json" -X GET http://172.0.5.27:80/nnrf-nfm/v1/nf-instances
* Hostname was NOT found in DNS cache
*   Trying 172.0.5.27...
* Connected to 172.0.5.27 (172.0.5.27) port 80 (#0)
> GET /nnrf-nfm/v1/nf-instances HTTP/1.1
> User-Agent: curl/7.35.0
> Host: 172.0.5.27
> Accept: */*
> Content-Type:application/json
>
< HTTP/1.1 200 OK
< content-length: 52
< content-type: application/json
< date: Fri, 26 Jul 2019 08:14:56 GMT
<
* Connection #0 to host 172.0.5.27 left intact
["http://172.0.5.27/nnrf-nfm/v1/nf-instances/upf01"]root@CASA-MOBILE:~#
root@CASA-MOBILE:~#
root@CASA-MOBILE:~#
```
# UPF配置及路由
## upf配置

```
user-plane-service upf
 access
   interface name gige 0/0
   exit
 pfcp
   interface name loopback 5
   peer pgw-c
     ip-addr 5.1.1.37
     initiate-association
     ip pool ue-pool-outside type ipv4  172.31.1.0/24 public index 1
     ip pool ue-pool-v6 type ipv6  2001:2001::/52 public index 2
     exit
   exit
 nrf-agent
   nrf-addr ip 172.0.5.27
   nrf-port 80
   exit
 identity
   plmn mcc 450 mnc 05
   instance-id upf01
   locality local
   smf-service-areas smfSerArea0
   fqdn root-upf01-agent.root
   exit
 network-slice sst 1 sd 1
   dnn inet1
     ip pool nrf-pool type ipv4 3.3.0.0/24
     exit
   exit
 exit


```
## upf添加u面路由到gnb
```
route host 10.27.0.1 gw 172.0.5.27
10.27.0.1/32 [1/0] via 172.0.5.27, gige0/0
```
## upf添加c面路由到gnb
```
route net 172.24.14.0/24 gw 172.0.5.27
172.24.14.0/24 [1/0] via 172.0.5.27, gige0/0
```


# docker主机添加iptable路由
## c面路由
```
iptables -I DOCKER-USER --dst 172.24.14.0/24 -j ACCEPT
```
## u面路由
```
iptables -I DOCKER-USER --dst 10.27.0.0/24 -j ACCEPT
```

# 添加CPE路由
把pdn下一跳设为sim-gnb
```
route add -host 172.0.5.25 gw 172.24.14.13
```
# 添加pdn路由
把ue下一跳设为upf
```
route add -net 11.11.0.0/16 gw 172.0.5.38
```
