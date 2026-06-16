# TODO

このサイトの今後の作業メモ。

## SEO・公開まわり

- [ ] **独自ドメインの設定**（GitHub Pages のカスタムドメイン）
  - リポジトリ直下に `CNAME` ファイルを置き、DNS（A／CNAME レコード）を設定する
  - 設定後は SEO 用の URL を一斉に差し替える（`index.html` ／ `app/index.html` の `canonical`・OGP、`app/qr.html` の `canonical` と QR の `APP_URL`、`robots.txt`、`sitemap.xml`）。現状はすべて `https://optimisuke.github.io/ebir-brew-studio/` を基準にしている
- [ ] 公開後、[Google Search Console](https://search.google.com/search-console) にサイトを登録し、`sitemap.xml` を送信する（検索結果への反映を早めるため）
  - **Google Search Console** = Google が提供する無料ツール。自分のサイトが検索でどう扱われているか（流入ワード・エラー・インデックス状況）を確認・管理できる。`sitemap.xml` を送信すると Google にページ一覧を直接伝えられ、検索結果への掲載が早く・確実になる
  - 手順: サイト公開 → Search Console でサイト所有権を確認（指定 HTML ファイル設置 or DNS 認証）→「サイトマップ」メニューで `sitemap.xml` の URL を送信
- [ ] **Google Analytics（GA4）の導入**（アクセス解析）
  - GA4 のプロパティを作成し、測定 ID（`G-XXXXXXXXXX`）を取得 → 各ページの `<head>` に gtag スニペットを設置（`index.html` / `app/index.html` / `app/qr.html`）。共通 `<head>` の仕組みがないので各ファイルに追記する
  - localStorage 以外の永続化・外部送信を避ける方針（[AGENTS.md](./AGENTS.md) の「約束ごと」）と矛盾しないか確認する。導入する場合は Cookie 同意やプライバシー表記の要否も検討
  - `app/qr.html` は `noindex`。計測に含めるかどうかは方針次第（店舗掲示用なので除外も可）

## サイト本文・コンテンツ

- [ ] **お店・オーナーの写真をサイトに追加**（ブランド/アクセスまわりの訴求強化）
  - 店内・外観・ビールなどの店舗写真と、海老原夫妻（オーナー）の写真を `index.html` に掲載する
  - 写真は `assets/` に置き、スマホファースト＆白基調のデザイン（[DESIGN.md](./DESIGN.md) / [STYLEGUIDE.md](./STYLEGUIDE.md)）に合わせる。OGP 画像の差し替えも検討
  - 掲載可否（人物写真の公開許可）を確認してから載せる

## アプリ（My ÉBIR）

- [x] **ラインナップ + 飲んだ記録アプリ化**（1ページ / コンプ率 / 記録シート / バッジ / QR掲示ページ）
- [x] **イベント参加機能**（店内イベントQR `#event` から参加を記録 / 種類別コレクションバッジ＋全種類制覇＋参加回数 / `app/event-types.js` で種類を一元管理 / `app/event.html` でポスター風QR発行）
- [x] **バッジ拡充**（5種類達成🎨・10種類達成🌈を追加 / 獲得表示を✓マーク化＋条件を常時表示 / 「アーメットの騎士」→「甲殻類の王🦐」）
- [x] **ビールデータの拡充**（`app/index.html` の `BEERS`）
  - 新10銘柄を写真・`style`・`abv`・`ibu`・`taste`・`pairing`・`desc` 付きで登録済み（トップ `index.html` のビール紹介も同データに更新）
  - 終売した銘柄を `active:false` にすると一覧から外れ、記録には残る（隠しバッジ「伝説の一杯」は廃止済み）
- [ ] **Notion API からビールの説明を取得してビルドする仕組み**（コンテンツ運用の省力化）
  - Notion DB（銘柄ごとに `name`／`style`／`abv`／`ibu`／`desc`／`active`／`new` 等を管理）を単一の情報源にし、ビルド時に取得して `BEERS` 相当のデータを生成する
  - 現状は「ビルドステップなし・素の HTML/JS」が前提（[AGENTS.md](./AGENTS.md)）。導入するとビルド工程が増えるので、方針（生成した JSON をコミットする静的生成 / GitHub Actions で定期ビルド など）を決める。Notion トークンはクライアントに出さず、ビルド時のみ使う
  - 取得 → `app/index.html` の `BEERS` を生成 or 外部 JSON 化して読み込み。トップ（`index.html`）のビール紹介と情報源を揃えることも検討
- [x] **記録の Export / Import 機能**（端末間のバックアップ・引っ越し）
  - 記録を base64 にして URL ハッシュ（`#import=...`）に埋めた引き継ぎリンクを発行（ヘッダーの「機種変更」導線）。別端末で開くと確認のうえ上書き。`hashchange` 対応・長リンク警告つき
- [ ] **称号名のネーミング案を検討**（「漢字2文字＋カタカナ」のような型）
  - 現状: 新麦/麦汁/発酵/熟成/麦酒（読みは ひらがな）。`麦酒=ビール` のように、漢字2文字のあとにカタカナの読み／異名を添える型で統一できないか案を出す
  - 醸造の段階（新麦→麦汁→発酵→熟成→麦酒）の世界観を保ちつつ、語感の良いカタカナを当てる
- [ ] **隠しバッジ「甲殻類の王🦐」の条件**（現状: コンプ＋通算25杯）を店オリジナルのネタに調整（旧名「アーメットの騎士」から改名済み）
- [ ] QR掲示ページを実際に A4 印刷して余白・QRサイズを確認
