---
marp: true
---
<style>
section {
    justify-content: start;
    padding: 50px
}

p {
  margin: 10px;
}

h1 {
  position: absolute;
  top: 50%;
}

img[alt="認証クラス"] {
  position: absolute;
  height: 70%;
}

img[alt="処理フロー"] {
  position: absolute;
  height: 75%;
}

p, ul, li {
  font-size: 28px;
}

</style>

# Javaアプリのライブラリバージョンアップ/依存関係解決

---
## はじめに
JavaのWebアプリのライブラリ/フレームワークをバージョンアップしました
依存関係の解決方法等の共有になります

---
## バージョンアップの理由

- 不要になった機能の削除に伴い使われなくなる
- Javaのバージョンアップ
  - 6→8
- 構成の変更
  - 実行環境 weblogic→tomcat
  - redisサーバの追加(セッション維持のため)
  - Oracle DB 11g→12cr2


---
## 大まかな流れ
1. 現在使用しているライブラリを一覧化
1. spring frameworkの次バージョンの変更点を確認
1. 各ライブラリの最新バージョンが対応するJavaバージョンの確認
1. ライブラリ/フレームワークの依存関係の確認
1. ライブラリにセキュリティ面で問題はないかの確認

---
## 現在使用しているライブラリを一覧化
- 各ライブラリがシステム上どのような機能で必要なのか確認
- プロジェクトレポートプラグイン
  - http://gradle.monochromeroad.com/docs/userguide/project_reports_plugin.html
  - プロジェクトの依存関係をレポート出力
  - フレームワークに依存するライブラリも確認可能
- そもそも使っているのかgrepして確認
  - ソースコード上は使っていないがライブラリで必要な可能性もある

---
## spring frameworkの各バージョンの変更点を確認
- githubのwiki/Release-Notesに載ってる
  - https://github.com/spring-projects/spring-framework/wiki/Upgrading-to-Spring-Framework-5.x
  - spring boot使うか検討していたのでそちらも確認
- 2.x系にバージョンアップしたくても他ライブラリ等に影響があるので注意
  - velocity template使っているのに連携クラスがなくなるとか・・
- マイグレーションガイド
  - https://github.com/spring-projects/spring-boot/wiki/Spring-Boot-2.0-Migration-Guide
- spring session
  - redis使うので対応するバージョン等を確認
- （見積もり前に調べていたりする・・）

---
## 各ライブラリの最新バージョンが対応するJavaバージョンの確認
-  リポジトリの取得元がmaven repositoryであれば、そこから確認
  - https://mvnrepository.com
- 情報がなければgithubのリポジトリを探す
  - たぶん、だいたい載ってる
- jdbcドライバのバージョン確認
  - 影響するのはJavaとDBのバージョン
  - https://www.oracle.com/technetwork/jp/database/application-development/jdbc/overview/default-090281-ja.html#01_03_1 

---
## ライブラリ/フレームワークの依存関係の確認
- (再)プロジェクトレポートプラグインで確認
- spring bootを使う場合は不要になるライブラリが多い
  - 使用したいライブラリを依存関係にもつ
