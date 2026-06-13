# TODO

このサイトの今後の作業メモ。

## SEO・公開まわり

- [ ] **独自ドメインの設定**（GitHub Pages のカスタムドメイン）
  - リポジトリ直下に `CNAME` ファイルを置き、DNS（A／CNAME レコード）を設定する
  - 設定後は SEO 用の URL を一斉に差し替える（`index.html` ／ `app/index.html` の `canonical`・OGP、`app/qr.html` の `canonical` と QR の `APP_URL`、`robots.txt`、`sitemap.xml`）。現状はすべて `https://optimisuke.github.io/ebir-brew-studio/` を基準にしている
- [ ] 公開後、[Google Search Console](https://search.google.com/search-console) にサイトを登録し、`sitemap.xml` を送信する（検索結果への反映を早めるため）
  - **Google Search Console** = Google が提供する無料ツール。自分のサイトが検索でどう扱われているか（流入ワード・エラー・インデックス状況）を確認・管理できる。`sitemap.xml` を送信すると Google にページ一覧を直接伝えられ、検索結果への掲載が早く・確実になる
  - 手順: サイト公開 → Search Console でサイト所有権を確認（指定 HTML ファイル設置 or DNS 認証）→「サイトマップ」メニューで `sitemap.xml` の URL を送信

## アプリ（My ÉBIR）

- [x] **タップリスト + 飲んだ記録アプリ化**（1ページ / コンプ率 / 記録シート / バッジ / QR掲示ページ）
- [ ] **ビールデータの拡充**（`app/index.html` の `BEERS`）
  - `desc`（各銘柄の説明文）を実際の内容で埋める
  - `FORTY OF HOPE` / `IT BRUT` の `abv`・`ibu`・`style` を確定値に
  - `new:true` は現状 `Peel My Lemon` を仮置き。最新銘柄に合わせて見直す
  - 終売した銘柄を `active:false` にすると一覧から外れ、隠しバッジ「伝説の一杯」の対象として記録には残る
- [ ] **隠しバッジ「アーメットの騎士」の条件**（現状: コンプ＋25杯）を店オリジナルのネタに調整
- [ ] QR掲示ページを実際に A4 印刷して余白・QRサイズを確認
