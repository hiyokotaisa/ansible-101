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
## 物理サーバ
- 長いリードタイム  
  
→サーバーのセットアップは大掛かりな作業  
   
**スタティックなインフラ**
---
## サーバー仮想化の台頭
- デプロイにかかる時間がさらに短く
- 大量のサーバーが生み出されるようになった
---
## クラウドの登場
- 大量のインスタンスを数分でデプロイ可能に
- サーバの増減も柔軟になった  
   
**ダイナミックなインフラに**
---
## 今までの手法
- エンジニアが手作業で構築
- 場合によってシェルスクリプトなどで自動化
- 手動でテスト  
   
→長いリードタイムが許される時代のやり方
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
   
→人間を介在させず、もっと効率的に品質も柔軟性も高めたい
---
## でも……
- 環境がごちゃごちゃで自動化するのが怖い
- だから自動化ツールの外で変更しよう
- さらにサーバに統一性がなくなる  
   
→どんどん自動化が怖くなる悪循環
---
# Infastructure as Code
---
## Infrastructure as Codeとは
- インフラをコードで定義
- Devで培った手法をインフラにも適用できる
- バージョン管理ツール(VCS)を使用した変更管理
---
# Ansible
---
## Ansibleとは？
- Red Hat社が開発する構成管理ツール
- 2018年1月時点の安定版は2.4
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
- 詳細はここから
[Module Index — Ansible Documentation] (http://docs.ansible.com/ansible/latest/modules_by_category.html)
---
## Gather factsの情報を見てみよう
- setupモジュールを使用することで確認できる
```
$ ansible all -m setup
```
---
