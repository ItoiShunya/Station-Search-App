# 沿線施設サーチャー (Gemini搭載)

出発駅・到着駅（乗り換え駅も指定可）を入力すると、Gemini APIが経路上の駅を自動で洗い出し、各駅の周辺にある施設（本屋・カフェなど）をGoogle Maps APIで一括検索して地図上に表示するWebアプリです。

## 主な機能

- 鉄道会社（JR / 名鉄 / 地下鉄）と駅名を指定して経路を入力
- 乗り換え駅の追加にも対応
- Gemini APIが経路上の全駅を推定
- 各駅を中心に指定した範囲(m)内で、キーワードに一致する施設をGoogle Maps Places APIで検索
- 検索結果を地図上のピンとリストの両方で表示（クリックで地図が該当地点にパン）

## 技術構成

- HTML / Tailwind CSS / JavaScript（フレームワーク不使用、単一の`index.html`）
- [Google Maps JavaScript API](https://developers.google.com/maps/documentation/javascript)（地図表示・Places検索）
- [Gemini API](https://ai.google.dev/)（経路上の駅リスト推定）
- デプロイ: GitHub Pages + GitHub Actions

## ローカルでの動作確認

APIキーは`config.js`に分離しており、このファイルは`.gitignore`でリポジトリから除外しています。ローカルで動かす場合は、リポジトリ直下に自分で`config.js`を作成してください。

```js
// config.js（このファイルはコミットしないこと）
const CONFIG = {
    GEMINI_API_KEY: "自分のGemini APIキー",
    GOOGLE_MAPS_API_KEY: "自分のGoogle Maps APIキー",
};
```

作成後、`index.html`をブラウザで開くか、簡易サーバーを立てて確認してください。

## デプロイ（GitHub Pages）

APIキーをリポジトリに含めないよう、GitHub Actionsでデプロイ時に`config.js`を自動生成する構成になっています。

1. リポジトリの `Settings → Secrets and variables → Actions` に以下を登録
   - `GEMINI_API_KEY`
   - `GOOGLE_MAPS_API_KEY`
2. `Settings → Pages` の「Source」を **GitHub Actions** に設定
3. `main`ブランチにpushすると、`.github/workflows/deploy.yml`が自動実行され、Secretsから`config.js`を生成した上でGitHub Pagesに公開されます

## APIキーの制限について

両APIキーとも、Google Cloud Console側で以下のように使用範囲を制限しています。

- **Google Maps APIキー**: HTTPリファラー制限として、GitHub PagesのURL（`https://<ユーザー名>.github.io/*`）を許可リストに登録
- 用途外での不正利用を防ぐため、必要な範囲以外へのアクセスは許可しない設定を推奨

## 既知の課題

- Gemini APIによる経路（駅リスト）推定の精度に改善余地あり
