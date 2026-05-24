## 概要
プロジェクトのダッシュボード的なhtmlファイルを、GitHub Pagesを使わずに、GitHub Actions（YML）で自動的にBluehostにアップしたいという話。

リポジトリ直下のhtmlファイルを、以下に自動アップロードするためには、

https://jugani-japan.com/tgg/github/プロジェクト名

** リポジトリごと **に設定が必要です。

## 閲覧方法
https://jugani-japan.com/tgg/github/には、BluehostのcPanelのFileManagerでパスワードロックが掛かってます。
ユーザ名：tgg
パスワード：Tgglobal2026

GitHub Pages（月額2000円）だとそんな設定要らないんだけどね。

## リポジトリに設定する場所
リポジトリのSettingーSecrets and Variables-Actions-Repository secrets[New repository secrets]

- Secret名:値（コピペ用）
FTP_SERVER:ftp.jugani-japan.com
FTP_USERNAME:github-deploy@tg-global.asia
FTP_PASSWORD:Tgglobal2015（だと思う）

## このsecretsはどこからきてるのか？
これは、BluehostのcPanelの「FTP Accounts」で追加されたユーザーです。

## YMLファイルはtgglobal-standardテンプレートから引き継がれる
\.github\workflows\deploy.yml
に置かれてるファイルは、テンプレートから引き継がれるそうです。

※最下部に書いてます

## robots.txtの設置
同様に、直下にファイルを置かないとYMLファイルでエラーなるよ。

中身これだけ？謎。

User-agent: *
Disallow: /

### 参考資料
name: 🚀 Deploy to Bluehost

on:
  push:
    branches: [ main ]   # master の場合は [ master ] に変更

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - name: 🚚 最新コードを取得
        uses: actions/checkout@v4

      - name: 📂 アップロード先ディレクトリを自動設定
        run: echo "DEPLOY_DIR=${GITHUB_REPOSITORY#*/}" >> $GITHUB_ENV

      - name: 📦 公開ファイルをステージング（*.html と robots.txt）
        run: |
          mkdir -p _deploy
          cp *.html _deploy/
          cp robots.txt _deploy/

      - name: 📤 FTPでアップロード（_deploy/ の中身のみ）
        uses: SamKirkland/FTP-Deploy-Action@v4.3.6
        with:
          server: ${{ secrets.FTP_SERVER }}
          username: ${{ secrets.FTP_USERNAME }}
          password: ${{ secrets.FTP_PASSWORD }}
          server-dir: ${{ env.DEPLOY_DIR }}/
          local-dir: ./_deploy/
          protocol: ftps
          dangerous-clean-slate: false
