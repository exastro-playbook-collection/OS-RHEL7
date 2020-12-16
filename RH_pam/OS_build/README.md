Ansible Role: OS-RHEL7/RH_pam/OS_build
=======================================================
# Description
本ロールは、RHEL7に関するパスワード設定についての情報の設定を行います。

# Supports
- 管理マシン(Ansibleサーバ)
  * Linux系OS（RHEL7）
  * Ansible バージョン 2.7 以上 (動作確認バージョン 2.7, 2.9)
  * Python バージョン 3.x  (動作確認バージョン 3.6)
- 管理対象マシン
  * RHEL7

# Requirements
- 管理マシン(Ansibleサーバ)
  * Ansibleサーバは管理対象マシンへssh接続できる必要があります。
- 管理対象マシン
  * RHEL7

# Dependencies

本ロールでは、他のロールは必要ありません。
ただし、本READMEに書かれている「エビデンスを取得する場合」の手順を行う場合は、
OS-RHEL7/RH_pam/OS_gatheringロールを利用します。

# Role Variables

本ロールで指定できる変数値について説明します。

## Mandatory Variables

ロール利用時に以下の変数値を指定する必要があります。

| Name | Description | 
| ---- | ----------- | 
| `VAR_RH_pam` | | 
| `- path` | ファイルパス /etc/pam.d/system-auth-ac | 
| &nbsp;&nbsp;&nbsp;&nbsp;`text` | system-auth-acファイルの内容 | 
| `- path` | ファイルパス /etc/pam.d/password-auth-ac | 
| &nbsp;&nbsp;&nbsp;&nbsp;`text` | password-auth-acファイルの内容 | 
| `- path` | ファイルパス /etc/login.defs | 
| &nbsp;&nbsp;&nbsp;&nbsp;`text` | login.defsファイルの内容 | 

### Example
~~~
VAR_RH_pam:
- path: /etc/pam.d/system-auth-ac
  text:
  - '#%PAM-1.0'
  - '# This file is auto-generated.'
  - '# User changes will be destroyed the next time authconfig is run.'
  - auth        required      pam_env.so
  - auth        required      pam_faildelay.so delay=2000000
  ・・・
- path: /etc/pam.d/password-auth-ac
  text:
  - '#%PAM-1.0'
  - '# This file is auto-generated.'
  - '# User changes will be destroyed the next time authconfig is run.'
  - auth        required      pam_env.so
  - auth        required      pam_faildelay.so delay=2000000
  ・・・
- path: /etc/login.defs
  text:
  - '#'
  - '# Please note that the parameters in this configuration file control the'
  - '# behavior of the tools from the shadow-utils component. None of these'
  - '# tools uses the PAM mechanism, and the utilities that use PAM (such as the'
  - '# passwd command) should therefore be configured elsewhere. Refer to'
  ・・・
~~~


## Optional Variables

特にありません。

# Usage

1. 本ロールを用いたPlaybookを作成します。
2. 変数を必要に応じて設定します。
3. Playbookを実行します。

# Example Playbook

## ■エビデンスを取得しない場合の呼び出す方法

本ロールを"roles"ディレクトリに配置して、以下のようなPlaybookを作成してください。

- フォルダ構成

~~~
 - playbook/
    │── roles/
    │    └── OS-RHEL7
    │         └── RH_pam/
    │              └── OS_build/
    │                   │── meta/
    │                   │      main.yml
    │                   │── tasks/
    │                   │      build_flat.yml
    │                   │      main.yml
    │                   │── templates/
    │                   │      flat_template
    │                   └─ README.md
    └─ master_playbook.yml
~~~

- マスターPlaybook サンプル[master_playbook.yml]

~~~
#master_playbook.yml
---
- hosts: all
  gather_facts: true
  roles:
    - role: OS-RHEL7/RH_pam/OS_build
      VAR_RH_pam:
      - path: /etc/pam.d/system-auth-ac
        text:
        - '#%PAM-1.0'
        - '# This file is auto-generated.'
        - '# User changes will be destroyed the next time authconfig is run.'
        - auth        required      pam_env.so
        - auth        required      pam_faildelay.so delay=2000000
        ・・・
      - path: /etc/pam.d/password-auth-ac
        text:
        - '#%PAM-1.0'
        - '# This file is auto-generated.'
        - '# User changes will be destroyed the next time authconfig is run.'
        - auth        required      pam_env.so
        - auth        required      pam_faildelay.so delay=2000000
        ・・・
      - path: /etc/login.defs
        text:
        - '#'
        - '# Please note that the parameters in this configuration file control the'
        - '# behavior of the tools from the shadow-utils component. None of these'
        - '# tools uses the PAM mechanism, and the utilities that use PAM (such as the'
        - '# passwd command) should therefore be configured elsewhere. Refer to'
        ・・・
  strategy: free
~~~

- Running Playbook

~~~
> ansible-playbook master_playbook.yml
~~~

## ■エビデンスを取得する場合の呼び出す方法

エビデンスを収集する場合、以下のようなエビデンス収集用のPlaybookを作成してください。  

- マスターPlaybook サンプル[master_playbook.yml]

~~~
#master_playbook.yml
---
- hosts: all
  gather_facts: true
  roles:
    - role: OS-RHEL7/RH_pam/OS_build
      VAR_RH_pam:
      - path: /etc/pam.d/system-auth-ac
        text:
        - '#%PAM-1.0'
        - '# This file is auto-generated.'
        - '# User changes will be destroyed the next time authconfig is run.'
        - auth        required      pam_env.so
        - auth        required      pam_faildelay.so delay=2000000
        ・・・
      - path: /etc/pam.d/password-auth-ac
        text:
        - '#%PAM-1.0'
        - '# This file is auto-generated.'
        - '# User changes will be destroyed the next time authconfig is run.'
        - auth        required      pam_env.so
        - auth        required      pam_faildelay.so delay=2000000
        ・・・
      - path: /etc/login.defs
        text:
        - '#'
        - '# Please note that the parameters in this configuration file control the'
        - '# behavior of the tools from the shadow-utils component. None of these'
        - '# tools uses the PAM mechanism, and the utilities that use PAM (such as the'
        - '# passwd command) should therefore be configured elsewhere. Refer to'
        ・・・
  strategy: free

- hosts: all
  gather_facts: true
  roles:
    - role: OS-RHEL7/RH_pam/OS_gathering
  strategy: free
~~~

- エビデンス収集結果一覧

エビデンス収集結果は、以下のように格納されます。
エビデンス収集結果の詳細は、OS_gatheringロールを確認してください。

~~~
#エビデンス構成
 - playbook/
    │── _gathered_data/
    │    └── 管理対象マシンホスト名 or IPアドレス/
    │         └── OS/
    │              └── RH_pam/
    │                   │── command/
    │                   │      ・・・
    │                   └── file/
    │                          ・・・
    └── _parameters/
            └── 管理対象マシンホスト名 or IPアドレス/
                 └── OS/
                        RH_pam.yml
~~~

# Remarks
-------

# License
-------

# Copyright
---------
Copyright (c) 2020 NEC Corporation

# Author Information
------------------
NEC Corporation
