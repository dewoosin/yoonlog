# 0. server 계정
- ECSS서버에는 DPMST 계정, Agent에는 dpagent(dpagt) 계정을 생성.
- 각 계정마다 사용하는 Port 등록 필요. Data-AID는 8011 부터 증가하며 사용하지만 고객사 요청에 맞게 변경하여 사용한다.
- Port와 방화벽 등록은 필수이다.
- GUI에서 접속 시 DB PORT 방화벽 , ECSS 서버. 총 2곳이 열려있어야 Data-AID 정상 구동이 가능하다.
- **Data-AID의 비밀번호는 dacpwd로 많이 설정되어있다.**
##    0.1_DPMST 계정
- ECSS 서버에 구동되는 Data-AID의 서버 계정이다.
- ECSS 서버에서는 dpnetc(통신모듈)과 dpplan(스케쥴), dpsvc(배치), dpstat (모듈 체크) 프로그램이 항상 작동되고 있어야 한다. (실행 계정 dpmst)

###    0.1.1 Dpmst 계정 환경 설정
- Bash_Profile 예시이다(본사 버전). 고객사 환경에 맞게 수정 필요
> ==Dpmst의 .bash_profile==
```Linux
# Get the aliases and functions
if [ -f ~/.bashrc ]; then
        . ~/.bashrc
fi

# User specific environment and startup programs

export PATH=.:$HOME/bin:$PATH

export LANG=ko_KR.euckr

ulimit -c unlimited

#*** Oracle env.
#  Ora12
#export ORACLE_HOME=/oracle/product/ora12/instantclient_12_2
#export NLS_LANG=AMERICAN_AMERICA.KO16MSWIN949
#export LD_LIBRARY_PATH=.:$ORACLE_HOME
#export PATH=$PATH:$ORACLE_HOME
# Ora19
#export ORACLE_HOME=/oracle/product/ora19/client_19_3
export ORACLE_HOME=/opt/oracle/product/19c/dbhome_1
export NLS_LANG=AMERICAN_AMERICA.KO16MSWIN949
export ORACLE_HOSTNAME=oracle
export PRACLE_UNQNAME=oracle19
export LD_LIBRARY_PATH=.:$ORACLE_HOME/lib
export PATH=$PATH:$ORACLE_HOME/bin
export ORACLE_SID=ORCLCDB

#*** Data-AID env.
export DPMSROOT=~/DAC
export DPMSROOTSRC=~/trunk
export LD_LIBRARY_PATH=.:$DPMSROOT/lib:$LD_LIBRARY_PATH

```

### 0.1.2 Dpmst config 파일
- DPMST 계정의 Config파일
**DAC/config/dacenv.xml**
```xml
<?xml version="1.0" encoding="EUC-KR"?>
<dacenv>
        <db kind="ORACLE" uid="ZGFj" pwd="ZGFjcHdk">
                <connect>
                        <!--<tns name="orcl"/-->
                                <direct hostname="192.168.219.126" port="1521" service="ORCLCDB" svntype="SID"/>
                </connect>
        </db>
        <debug module="dpnetc"   level="DETAIL" debug="Y" />
        <debug module="dpmakjob" level="DETAIL" debug="Y" />
        <debug module="dppaln"   level="DETAIL" debug="Y" />
        <debug module="dpstat"   level="DETAIL" debug="Y" />
        <separation data_col="&#x7E;&#x09;" data_row="&#x7E;&#x0A;" />
        <network master_ip="192.168.219.126" master_port="8001" agent_ip="" agent_port="" />
</dacenv>
~

```
- port를 8001로 설정되어있다. dpnetc 모듈이 8001을 사용한다.

## 0.2_dpagent
- source, target 서버에 사용되는 계정이다.
### 0.2.1 dpagent 계정 환경 설정

**dpagent 계정의 .bash_profile** 
```linux
# Get the aliases and functions
if [ -f ~/.bashrc ]; then
        . ~/.bashrc
fi

# User specific environment and startup programs

export PATH=.:$HOME/bin:$PATH

export LANG=ko_KR.euckr

ulimit -c unlimited


#*** Oracle env.
#  Ora12
#export ORACLE_HOME=/oracle/product/ora12/instantclient_12_2
#export NLS_LANG=AMERICAN_AMERICA.KO16MSWIN949
#export LD_LIBRARY_PATH=.:$ORACLE_HOME
#export PATH=$PATH:$ORACLE_HOME
# Ora19
export ORACLE_HOME=/oracle/product/ora19/client_19_3
export NLS_LANG=AMERICAN_AMERICA.KO16MSWIN949
export LD_LIBRARY_PATH=.:$ORACLE_HOME/lib
export PATH=$PATH:$ORACLE_HOME/bin

#*** Data-AID env.
export DPMSROOT=~/DAC
export DPMSROOTSRC=~/trunk
export LD_LIBRARY_PATH=.:$DPMSROOT/lib:$LD_LIBRARY_PATH

```

### 0.2.2 Dpmst config 파일
- dpagent 계정의 Config파일
```xml
<?xml version="1.0" encoding="EUC-KR"?>
<dacenv>
        <debug module="dpnets"   level="DETAIL" debug="Y" />
        <debug module="dpmakqry" level="DETAIL" debug="Y" />
        <debug module="dpora"   level="DETAIL" debug="Y" />
        <debug module="dporal"   level="DETAIL" debug="Y" />
        <debug module="dporau"   level="DETAIL" debug="Y" />
        <debug module="dporaupd"   level="DETAIL" debug="Y" />
        <separation data_col="&#x7E;&#x09;" data_row="&#x7E;&#x0A;" />
        <network master_ip="" master_port="" agent_ip="192.168.219.126" agent_port="8011" />
</dacenv>
~

```

# 1. 서버 directory 구조
- Data-AID의 모든 계정은 아래와 같은 구조로 되어있다.

**계정의 directory 구조**
	DAC
		bin (실행 모듈)
		config (config 파일 )
		data (변환 시 data 파일 위치)
		lib (구동에 필요한 lib)
		log (구동시 log 적재)
		temp (Data-AID 구동 시 생성되는 임시파일)



# 3. 설치

## 3.1 ECSS 서버 설치 (DPMST 계정 )



- trunk 폴더 (source) 로 진입하여 각 모듈 빌드 실행
- 빌드 방법
	- make파일 실행 (ex make_dpmakjob.sh)


---

dpmst 계정에 필요한 모듈
- dpcvt
- dpmakjob
- dpnetc
- dpplan
- dpstat
- dpsvc
dpagent 계정에 필요한 모듈
- dpcvt
- dpmakqry
- dpnets
- dpora
- dporal
- dporau
- dporaupd
	