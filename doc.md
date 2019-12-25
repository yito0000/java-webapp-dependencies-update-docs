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

<style scoped>
h1 {
  position: absolute;
  top: 50%;
}

</style>

# Javaアプリのライブラリバージョンアップ/依存関係解決

---
## はじめに
JavaのWebアプリのライブラリ/フレームワークを整理/バージョンアップしました


---
## バージョンアップの理由
- Java 6 → 8
- 実行環境 Weblogic → Tomcat
- Redisサーバの追加(セッション維持のため)
- Oracle DB 11g → 12c R2

---
## バージョン選定の流れ
1. spring frameworkの次バージョンの変更点を確認
1. 現在使用しているライブラリを一覧化
1. 各ライブラリの最新バージョンの確認
1. ライブラリにセキュリティ面で問題はないかの確認

---
## spring frameworkの各バージョンの変更点を確認
- フレームワークはアプリに大きく影響するの優先して確認
- githubのwiki/Release-Notesに載ってる
  - https://github.com/spring-projects/spring-framework/wiki/Upgrading-to-Spring-Framework-5.x
  - spring boot使うか検討していたのでそちらも確認
- 2.x系にバージョンアップしたくても他ライブラリ等に影響があるので注意
  - velocity template使っているのに連携クラスがなくなるとか・・
- マイグレーションガイド
  - https://github.com/spring-projects/spring-boot/wiki/Spring-Boot-2.0-Migration-Guide
- spring session
  - redis使うので対応するバージョン等を確認

---
## 現在使用しているライブラリを一覧化
- 各ライブラリがどのような機能で必要なのか確認
- プロジェクトレポートプラグイン
  - http://gradle.monochromeroad.com/docs/userguide/project_reports_plugin.html
  - プロジェクトの依存関係をレポート出力
  - フレームワークに依存するライブラリも確認可能
  - [出力結果](./dependencies/index.html)
- そもそも使っているのかgrepして確認
- ソースコード上は使っていないがライブラリで必要な可能性もある


---
## 各ライブラリの最新バージョンの確認
-  リポジトリの取得元がmaven repositoryであればライブラリのHPリンク記載あり
  - https://mvnrepository.com
  - 情報がなければgithubかググって探す・・(だいたいgithubにあった)
- 対応するJavaバージョン確認
- Release Noteから変更点を確認
  - アプリへどのような影響があるか
- jdbcドライバのバージョン確認
  - 影響するのはJavaとDBのバージョン
  - https://www.oracle.com/technetwork/jp/database/application-development/jdbc/overview/default-090281-ja.html#01_03_1 

---
## ライブラリの脆弱性確認
- owasp dependency check
  - OWASP：https://www.owasp.org/index.php/Japan
  - https://plugins.gradle.org/plugin/org.owasp.dependencycheck
  - 脆弱性の診断をしてくれるプラグイン
    - NVDなどに報告がないかのチェック
      - National Vulnerability Database
      - 脆弱性情報データベース
- 明らかな脆弱性があるライブラリは使いたくない

---
## バージョン変更後
### ビルド・デプロイ → ログイン画面を開く → ログインする

# いろんなエラーが起きた✌️😇

---
## バージョンアップによって生まれたバグ
- 画面が表示できない
- 別モジュールの画面へ遷移できない
- ログインできない
- oauth2を使った機能が動かない
- native queryでエラー
- 画面が文字化けする
- など・・

---
## まとめ
- プロジェクトレポートが便利
- アプリ全体の仕様を再度理解する良い機会になった
  - ライブラリは必ずどこかの機能で必要
- フレームワークのバージョンを変えると影響を把握するのが難しい
- マイグレーションガイドがあればしっかり読んだほうが良い
  - ガイドがあることを知らずに変えてエラーを解決するのにハマったり・・
- 全て変えるのはおすすめできないが、適切なタイミングがワカラナイ

