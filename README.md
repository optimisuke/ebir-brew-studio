# ÉBIR BREW STUDIO

神戸・住吉のクラフトブリュワリー **ÉBIR BREW STUDIO（エビールブリュースタジオ）** の
サイトと、来店者向けのちょっとした Web アプリです。

> エビールは、神戸・住吉のまちに根をおろした、小さなブリュワリーです。
> スタイルや流行に縛られすぎず、土地の空気・人の手・偶然の発酵が生むニュアンスを受け入れる。
> ビールを“つくる”というより、“育てる”感覚で仕込んでいます。

## 特徴

- **シンプルな静的サイト** — HTML + 素の JavaScript + Tailwind CSS のみ。ビルド不要
- **サーバーレス** — GitHub Pages などの静的ホスティングで配信
- **データは端末内に** — アプリのデータは localStorage に保存。アカウント登録なし
- **スマホファースト** — PC でも見やすいレスポンシブ

## 構成

```
index.html     トップページ（ブランド紹介 / ビール / アクセス / Instagram）
app/           来店者向けの localStorage アプリ
assets/        ロゴ・画像
```

## ローカルで確認する

ビルドは不要です。任意の静的サーバーで開いてください。

```sh
python3 -m http.server 8000
# ブラウザで http://localhost:8000 を開く
```

## リンク

- Instagram: <https://www.instagram.com/ebirbrewstudio/>
- オンラインショップ: <https://ebirbrewstudio.square.site/>

## デプロイ

静的ファイルのみなので、GitHub Pages の対象ブランチ／ディレクトリを指定するだけで公開できます。

---

開発者・AI エージェント向けの詳細は [AGENTS.md](./AGENTS.md) を参照してください。

*20歳未満の方の飲酒は法律で禁止されています。*
