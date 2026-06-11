# TODO

このサイトの今後の作業メモ。

## SEO・公開まわり

- [ ] **独自ドメインの設定**（GitHub Pages のカスタムドメイン）
  - リポジトリ直下に `CNAME` ファイルを置き、DNS（A／CNAME レコード）を設定する
  - 設定後は SEO 用の URL を一斉に差し替える（`index.html` ／ `app/index.html` の `canonical`・OGP、`robots.txt`、`sitemap.xml`）。現状はすべて `https://optimisuke.github.io/ebir-brew-studio/` を基準にしている
- [ ] 公開後、[Google Search Console](https://search.google.com/search-console) にサイトを登録し、`sitemap.xml` を送信する（検索結果への反映を早めるため）
  - **Google Search Console** = Google が提供する無料ツール。自分のサイトが検索でどう扱われているか（流入ワード・エラー・インデックス状況）を確認・管理できる。`sitemap.xml` を送信すると Google にページ一覧を直接伝えられ、検索結果への掲載が早く・確実になる
  - 手順: サイト公開 → Search Console でサイト所有権を確認（指定 HTML ファイル設置 or DNS 認証）→「サイトマップ」メニューで `sitemap.xml` の URL を送信

## アプリ（My ÉBIR）

- [ ] **My ÉBIR app の改善**（来店者向けの localStorage アプリ。内容を詰めていく）
