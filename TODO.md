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

## アプリ（My ÉBIR）

- [x] **タップリスト + 飲んだ記録アプリ化**（1ページ / コンプ率 / 記録シート / バッジ / QR掲示ページ）
- [ ] **ビールデータの拡充**（`app/index.html` の `BEERS`）
  - `desc`（各銘柄の説明文）を実際の内容で埋める
  - `FORTY OF HOPE` / `IT BRUT` の `abv`・`ibu`・`style` を確定値に
  - `new:true` は現状 `Peel My Lemon` を仮置き。最新銘柄に合わせて見直す
  - 終売した銘柄を `active:false` にすると一覧から外れ、隠しバッジ「伝説の一杯」の対象として記録には残る
- [ ] **Notion API からビールの説明を取得してビルドする仕組み**（コンテンツ運用の省力化）
  - Notion DB（銘柄ごとに `name`／`style`／`abv`／`ibu`／`desc`／`active`／`new` 等を管理）を単一の情報源にし、ビルド時に取得して `BEERS` 相当のデータを生成する
  - 現状は「ビルドステップなし・素の HTML/JS」が前提（[AGENTS.md](./AGENTS.md)）。導入するとビルド工程が増えるので、方針（生成した JSON をコミットする静的生成 / GitHub Actions で定期ビルド など）を決める。Notion トークンはクライアントに出さず、ビルド時のみ使う
  - 取得 → `app/index.html` の `BEERS` を生成 or 外部 JSON 化して読み込み。トップ（`index.html`）のビール紹介と情報源を揃えることも検討
- [ ] **記録の Export / Import 機能**（端末間のバックアップ・引っ越し）
  - localStorage の `ebir.records`（＋スキーマ版数）を JSON で書き出し／取り込みできるようにする
  - 端末故障・機種変更でデータが消える問題への対策。サーバーレス前提なので手動エクスポートで担保する
  - Import 時はスキーマ版を見て `runMigrations()` を通す。重複取り込み（同一 `id`）の扱いも決める
- [ ] **称号名のネーミング案を検討**（「漢字2文字＋カタカナ」のような型）
  - 現状: 新麦/麦汁/発酵/熟成/麦酒（読みは ひらがな）。`麦酒=ビール` のように、漢字2文字のあとにカタカナの読み／異名を添える型で統一できないか案を出す
  - 醸造の段階（新麦→麦汁→発酵→熟成→麦酒）の世界観を保ちつつ、語感の良いカタカナを当てる
- [ ] **隠しバッジ「アーメットの騎士」の条件**（現状: コンプ＋25杯）を店オリジナルのネタに調整
- [ ] QR掲示ページを実際に A4 印刷して余白・QRサイズを確認
