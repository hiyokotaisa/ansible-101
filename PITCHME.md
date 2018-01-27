# 今日からはじめるAnsible
@hiyoko_taisa
---
## おしながき
- Infrastructure as Code?
- 自動構成ツール?
- Ansible?
- ちょっとしたデモ
---
## お前は誰だ
ひよこ大佐(@hiyoko_taisa)  
<img src="https://pbs.twimg.com/profile_images/935672125830021120/a_ISqdW9_400x400.jpg" width="200px">  
- SIerで設計・構築を5年ほど
- 「転職の人」
---
# Infrastructure as Code?
---
# いままでのインフラ
---
## 手作業で構築・運用
- エンジニアが手作業で構築
- 場合によってシェルスクリプトなどで自動化
- 目視によるテスト
- 長いリードタイム
---
## 手作業が引き起こす問題
- ヒューマンエラー
- 統一されていない手順
- 管理されていない変更
人間って信用できないよね
---
# Infastructure as Code
---
## Infrastructure as Codeとは
- インフラをコードで定義
- Devで培った手法をインフラにも適用できる
- バージョン管理ツール(VCS)を使用した変更管理
---
# Ansible
<img src="https://www.ansible.com/hubfs/2016_Images/Assets/Ansible-Mark-Large-RGB-Mango.png?t=1516991903140" width="200px">  
---
## Ansibleとは？
- Red Hat社が開発する構成管理ツール
- 2018年1月時点の安定版は2.4
- 
---
## Gather factsの情報を見てみよう
- setupモジュールを使用することで確認できる
```
$ ansible all -m setup
```
