# いまからはじめるAnsible
@hiyoko_taisa
---
## おしながき
- いままでのインフラ
- Infrastructure as Codeとは?
- Ansibleとは？
- ちょっとしたデモ
---
## お前は誰だ
ひよこ大佐(@hiyoko_taisa)  
<img src="https://pbs.twimg.com/profile_images/935672125830021120/a_ISqdW9_400x400.jpg" width="200px">  
- SIerで設計・構築を5年ほど
- 「転職の人」
---
# いままでのインフラ
---
## 物理サーバ中心の時代
- 長いリードタイム
- 長期利用を見越した設計
- 大掛かりなセットアップ作業
   
**スタティックなインフラ**
---
## 今までの手法
- エンジニアが手作業で構築
- 場合によってシェルスクリプトなどで自動化
- 手動でテスト  
**スタティックなインフラのやり方**
---
## サーバー仮想化の台頭
- ESXi/Hyper-V/Xen/KVM
- デプロイにかかる時間がさらに短く
- 大量のサーバーが生み出されるようになった
---
## クラウドの登場
- 大量のインスタンスを数秒〜数分でデプロイ可能に
- サーバの増減も柔軟になった  
- スケールアップよりもスケールアウトを重視  
**ダイナミックなインフラに**
---
## その結果
- 構築・運用対象が爆発的に増え、環境も多様化
- スノーフレークサーバの発生
- 従来のやり方では立ち行かなくなってきた
---
## 手作業が引き起こす問題
- ヒューマンエラー
- 時間がかかる
- 統一されていない手順
- 管理されていない変更  
   
### 人間を介在させず、もっと効率的に品質も柔軟性も高めたい
---
## でも……
- 環境がごちゃごちゃで自動化するのが怖い
- だから自動化ツールの外で変更しよう
- さらにサーバに統一性がなくなる  
   
### どんどん自動化が怖くなる悪循環
---
# Infrastructure as Code
---
## Infrastructure as Codeとは
- インフラをコードで定義
- Devで培った手法をインフラにも適用できる
- バージョン管理ツール(VCS)を使用した変更管理
- 冪等性を保った変更
---
## 冪(べき)等性とは
- **何度実行しても常に同じ結果になる**
- 同じ環境を同じコードで何度でも構築可能になる
---
# Ansible
---
## Ansibleとは？
- Red Hat社が開発する構成管理ツール
- 2018年1月時点の安定版は2.4
- 多種多様な環境のセットアップが可能
- エージェントレス
- YAML形式のPlaybookで構成を定義
---
## Ansibleの構成要素
- Inventory
- Module
- Playbook
- Role
---
## Inventory
- Ansibleが管理する環境の接続情報
- 複数の環境をグループ分けして定義することが可能
- IaaSからAPI経由で動的にInventoryを生成する機能もある(Dynamic Inventory)
---
## Inventory サンプル
```
[test_servers]
test01
test02
[web_servers]
web[01:10]
```
---
## Module
- Ansibleから実行するコマンド郡
- OS操作やファイル操作等多種多様なModule
- 詳細は下記コマンドもしくは公式Web参照
```
$ ansible-doc -l
```
---
## Moduleの特徴
- 実行前に現在の状態を調査する
- 変更が必要な場合にのみ変更処理を実行
- command/shell モジュールなどは冪等性が担保されない
---
## Playbook
- Ansibleにおける「手順書」
- YAML形式で記述
- 細かなRole単位に細分化することが多い
---
## Playbook サンプル
[ansible-example/lamp_simple_rhel7/site.yml](https://github.com/ansible/ansible-examples/blob/master/lamp_simple_rhel7/site.yml)
```
# This playbook deploys the whole application stack in this site.

- name: apply common configuration to all nodes
  hosts: all
  remote_user: root

  roles:
    - common

- name: configure and deploy the webservers and application code
  hosts: webservers
  remote_user: root

  roles:
    - web

- name: deploy MySQL and configure the databases
  hosts: dbservers
  remote_user: root

  roles:
    - db
```
---
## Role
- Playbookを切り出したもの
- 各セットアップ単位(e.g. nginxのセットアップ)で切り出ることが多い
- Roleに切り出すことで、別の環境でも再利用しやすくなる
---
## Roleサンプル(common)
[ansible-examples/lamp_simple_rhel7/roles/common/tasks/main.yml](https://github.com/ansible/ansible-examples/blob/master/lamp_simple_rhel7/roles/common/tasks/main.yml)
```
# This playbook contains common plays that will be run on all nodes.

- name: Install ntp
  yum: name=ntp state=present
  tags: ntp

- name: Install common dependencies
  yum: name={{ item }} state=installed
  with_items:
   - libselinux-python
   - libsemanage-python
   - firewalld

- name: Configure ntp file
  template: src=ntp.conf.j2 dest=/etc/ntp.conf
  tags: ntp
  notify: restart ntp

- name: Start the ntp service
  service: name=ntpd state=started enabled=yes
  tags: ntp
```
---
## template(ntp.conf.j2)
```
driftfile /var/lib/ntp/drift

restrict 127.0.0.1 
restrict -6 ::1

server {{ ntpserver }}

includefile /etc/ntp/crypto/pw

keys /etc/ntp/keys
```
---
# 実際に触ってみよう
---
## Ansibleのインストール
今回はyumでinstallする
- yum install ansible
- pip2 install ansible
- Sourceから
---
## Ansibleの動作確認
```
$ ansible localhost -m ping
```
Warningが出ても想定通りの動作なので問題なし
---
## Dockerのインストール
```
# yum install docker
```
---
## docker環境のセットアップ
```
# sudo systemctl start docker
# docker pull centos:latest

# docker run --privileged  --name test01 -i -t -d centos /sbin/init
# docker run --privileged  --name test02 -i -t -d centos /sbin/init
# doker ps
```
---
## Inventoryを書いてみよう
./hosts
```
[testservers]
test01
test02
```
---
## Playbookを書いてみよう
./site.yml
```
---
---
- name: install httpd
  hosts: testservers
  connection: docker
  tasks:
    - name: Install httpd
      yum:
        name: httpd
        state: present

    - name: Start httpd
      systemd:
        name: httpd
        state: started
        enabled: yes
```
---
## 実行してみよう
```
# ansible-playbook -i hosts site.yml --check

# ansible-playbook -i hosts site.yml
```
---
## 確認してみよう
```
# docker exec -it test01 /bin/bash
# systemctl status httpd
```
---
# Ansibleの旅はこれからだ！
---
# END
