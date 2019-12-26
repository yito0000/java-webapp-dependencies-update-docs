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
バージョン変えるのに何調べたかとか、どんなバグが発生したかとか話します

---
## バージョンアップの理由
- Java 6 → 8
- 実行環境 Weblogic → Tomcat
- Redisサーバの追加(セッション維持のため)
- Oracle DB 11g → 12c R2

## バージョンアップ前
- Spring Framework
- Hibernate
- Servlet

---
## バージョン選定の流れ
1. springの各バージョンの変更点を確認
1. 現在使用しているライブラリを一覧化
1. 各ライブラリの最新バージョンの確認
1. ライブラリにセキュリティ面で問題はないかの確認

---
## springの各バージョンの変更点を確認
- フレームワークはアプリに大きく影響するので優先して確認
- バージョンごとの変更点はgithubのwiki/Release-Notes
  - spring boot使うか検討していたのでそちらも確認
- 2.x系にバージョンアップしたくても他ライブラリ等に影響があるので注意
  - velocity template使っているのに連携クラスがなくなるとか・・
- マイグレーションガイド
- spring session
  - redis使うので対応するバージョン等を確認

---
## 現在使用しているライブラリを一覧化
- 各ライブラリがどのような機能で使っているのか確認
- プロジェクトレポートプラグイン
  - Maven/Gradleで使える
  - プロジェクトの依存関係をレポート出力
  - [出力結果](./dependencies/index.html)
- そもそも使っていないのがある？
  - 不要なのは削除
  - ソースコード上は使っていないがライブラリで必要な可能性もある

---
## 各ライブラリの最新バージョンの確認
-  リポジトリの取得元がmaven repositoryであればライブラリのHPリンク記載あり
  - 情報がなければgithubかググって探す・・(だいたいgithubにあった)
- 対応するJavaバージョン確認
- Release Noteから変更点を確認
  - アプリへどのような影響があるか
- jdbcドライバのバージョン確認
  - 影響するのはJavaとDBのバージョン

---
## ライブラリの脆弱性確認
- OWASP dependency check
  - OWASP
  - 脆弱性の診断をしてくれるプラグイン
    - NVDなどに報告があるかチェック
      - National Vulnerability Database
      - 脆弱性情報データベース
  - [出力結果](./dependency-check-report.html)
    - 解決済みのも出力される
- 明らかな脆弱性があるライブラリは使いたくない

---
## バージョン変更後
### ビルド・デプロイ → ログイン画面を開く → ログインする

# いろんなエラーが起きた✌️😇

---
## バージョンアップによって生まれたバグ
- 別モジュールの画面へ遷移できない
- ログインできない
- oauth2を使った機能が動かない
- native queryでエラー
- 画面が文字化けする
- など・・

## 主な原因
- springの各バージョンアップによりデフォルトの挙動が変わった
- セッション維持の方法を変えたため

---
## まとめ
- ライブラリ/フレームワークのバージョンを全て変えるのは辛い
- フレームワークのバージョンを変えるのが一番影響が大きい
- 依存関係を全て確認したい場合はプロジェクトレポートを使うと良い
- 現在使っているライブラリに脆弱性があるかどうかはOWASPのプラグインを使うと良い


---
## 参考にしたもの
- spring boot マイグレーションガイド
  - https://github.com/spring-projects/spring-boot/wiki/Spring-Boot-2.0-Migration-Guide
- spring framwework githubのwiki
  - https://github.com/spring-projects/spring-framework/wiki/Upgrading-to-Spring-Framework-5.x
- プロジェクトレポートプラグイン
  - http://gradle.monochromeroad.com/docs/userguide/project_reports_plugin.html
- OWASP
  - https://www.owasp.org/index.php/Japan
  - Gradleプラグイン https://plugins.gradle.org/plugin/org.owasp.dependencycheck

---
## 参考にしたもの
- maven repository
  - https://mvnrepository.com
- oracle jdbcドライバ 
  - https://www.oracle.com/technetwork/jp/database/application-development/jdbc/overview/default-090281-ja.html#01_03_1 
