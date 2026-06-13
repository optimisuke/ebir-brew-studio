# STYLEGUIDE.md

実装レベルのスタイル仕様（デザイントークンとコンポーネント）。方針は [DESIGN.md](./DESIGN.md) を参照。
スタイルは Tailwind CSS（Play CDN）で、配色・フォントは各ページ冒頭の `tailwind.config` に定義。

## カラートークン

| 名前 | HEX | 用途 |
|---|---|---|
| `ink` | `#141414` | 本文・見出し・Story/フッターの背景・主要ボーダー |
| `paper` | `#ffffff` | ベース背景・暗色面上のテキスト |
| `cream` | `#FBF7EF` | あたたかい白。Instagram 節の背景、画像の下地 |
| `gold` | `#E2A52A` | 差し色。CTA・見出しラベル・星評価・リンク hover・スペック表記 |
| `goldDark` | `#C0871C` | ゴールドの hover／押下時 |

透明度は Tailwind の `/` 表記で調整（例: `text-ink/70`, `border-ink/10`, `bg-paper/90`）。

### ゴールドの使いどころ
- 主ボタン「ビールを見る」: `bg-gold text-ink`（hover `bg-goldDark text-paper`）
- ヘッダー「Shop」ピル: `bg-gold text-ink`
- セクションラベル（eyebrow）: `text-gold`
- ビールのスタイル表記（例 `SOUR ・ 5.0% ・ IBU 7`）: `text-gold`
- リンク／フッターナビ hover: `hover:text-gold`
- アプリ: 「記録する」`bg-gold`、星評価 `text-gold`

## タイポグラフィ

- フォント: `Inter, system-ui, -apple-system, "Hiragino Kaku Gothic ProN", "Noto Sans JP", sans-serif`
- ウェイト: 400 / 500 / 600 / 700
- h1: `text-3xl sm:text-5xl font-bold tracking-tight`
- h2: `text-2xl sm:text-3xl font-bold`
- eyebrow ラベル: `text-xs font-semibold uppercase` + `letter-spacing: 0.25em`（`.section-label`）+ `text-gold`
- 本文: `text-base sm:text-lg`（Story は `leading-loose`、他は `leading-relaxed`）
- 注記: `text-xs text-ink/50`

## レイアウト

- コンテナ幅: メイン `max-w-5xl`、読み物系 `max-w-3xl` / `max-w-2xl`、左右 `px-5`
- セクション縦余白: `py-20 sm:py-28`
- 角丸: カード `rounded-3xl` / `rounded-2xl`、ボタン `rounded-full`、入力 `rounded-xl`
- 影は控えめ（`shadow-sm` / hover `shadow-md`）。境界は `border-ink/10` の細線が基本

## コンポーネント

### ヘッダー（共通）
- `sticky top-0`、`bg-paper/90 backdrop-blur`、下境界 `border-ink/10`、`h-16`
- 左: ロゴ（`assets/logo.jpeg`, `h-9`）＋ サイト名（＋ `β` タグ）／ 右: ナビ（sm 以上）と「Shop」ピルを 1 グループにまとめて右寄せ
- ナビの「My Beers」と各 APP 導線にはゴールドの `APP` バッジを付与

### ヒーロー（トップ）
- `grid lg:grid-cols-2`。**DOM 順は「コピー → 画像」**
  - スマホ: コピー（ロゴ→`ÉBIR BREW STUDIO`→`エビールブリュースタジオ`→ボタン）が先、下にキービジュアル
  - PC: 左コピー／右キービジュアル
- ロゴ `h-14`、サブタイトル `tracking-widest text-ink/50`
- ボタン: 主＝ゴールド塗り、副＝`border-ink` 枠線
- キービジュアル（`assets/key-visual.jpg`）: `rounded-3xl ring-1 ring-ink/10`。スマホは**最初の画面に収まるよう高さを制限**（`max-h-[42vh] max-w-[220px]`、`w-fit` のラッパで中央寄せ・縦横比維持）、PC（`lg`）は列いっぱい（`max-h-none max-w-none w-full`）。あわせてモバイルはヒーローの上下余白・行間を詰める（`pt-8 pb-10`, ロゴ `h-12`, 見出し `mt-4`, ボタン `mt-7`, グリッド `gap-7`）
- 動き（CSSのみ・ビールらしさ）:
  - **泡**: `.bubbles` レイヤー（`section` を `relative overflow-hidden`、コンテンツは `z-10`）。小円が `@keyframes rise` で立ちのぼる。泡はカード／ボタンと同じ語彙の 2 バリエーションのみ — `b-ink`（ink の細枠×白・主役）/ `b-ring`（gold の細枠×白・差し色）。インライン span（6ストリーム＋大玉）で配置（現在48個）。`prefers-reduced-motion` 時は `.bubbles` を非表示
  - **入場**: `.fade-up`（コピー、キービジュアルは `delay-2` でスタッガー）
  - **ゆらぎ**: キービジュアルに `.floaty`（ゆっくり上下）
  - `prefers-reduced-motion` 時はいずれも無効化
- リロード時は常に最上部から表示（`history.scrollRestoration = 'manual'`）

### Story
- `bg-ink text-paper` の反転セクション。ラベル `text-gold`、本文 `leading-loose text-paper/90`、段落間 `space-y-6`

### Beers
- 導入（ラベル＋見出し＋注記「ラインアップは随時変わります。最新は Instagram で」）
- 注目2種（AURA DE NADA / BRAGGOT WHISPER）: 写真付きカード
  - 画像枠 `aspect-[4/5] bg-cream object-cover`、hover で `scale-[1.03]`
  - 名前 → ゴールドのスタイル表記 → 説明
- ラインアップ: 集合写真（`assets/bottles-lineup.jpg`）＋ その他ビール一覧（確定スペックのみ表記。FORTY OF HOPE / IT BRUT は名前中心）
- 末尾に「オンラインショップで購入」ボタン（枠線）

### Instagram
- `bg-cream` の節。フォロー導線（ink ボタン＋Instagram アイコン、hover `goldDark`）
- ※プロフィールのフィード埋め込みは未実装（公式の簡易手段がないためフォロー導線で代替）

### Access
- 2カラム（情報 dl ＋ Google マップ iframe）
- 営業時間は固定表記＋注記「※お休みや営業時間の変更は Instagram をご確認ください。」
- 電話・メールはリンク（hover `text-gold`）

### フッター
- `bg-ink text-paper/70`。ロゴ＋サイト名、ナビ（hover `text-gold`）
- 「20歳未満の方の飲酒は法律で禁止されています。」の注記を必ず表示

### アプリ（タップリスト + 飲んだ記録 / `app/`）
飲みながら片手で使う前提で、**1ページ・1スクロール**（タブ／画面遷移なし）。
- トップと同じ配色・フォント。コンテナ `max-w-2xl`、`pt-6 pb-20`
- ヘッダー: ロゴ＋「My ÉBIR」＋ゴールドの `APP` バッジ＋`β` タグ／右に「← サイトに戻る」
- **ビールの単一情報源**は JS の `BEERS` 配列（`id`/`name`/`style`/`abv`/`ibu`/`img` or `color`/`desc`/`active`/`new`）。写真があれば写真、なければ `color` のビール色スウォッチ（泡→ビールの縦グラデーション）。スペック表記は `specLine()` が `style ・ abv% ・ IBU` を組み立て `text-gold`
- **ページ構成（上から）**:
  1. **コンプ率バー（スリム1行）** — 左に数値（`text-2xl`、達成時 `text-gold`）、右にプログレスバー（`bg-ink`／達成時 `bg-gold`）と「現行 M/N種・X杯」。タップリストを上部に引き上げるため、あえて小さく1行に圧縮している
  2. **ラインナップ**（見出し表記。内部的にはタップリスト）— 横スクロールのカルーセル（`flex overflow-x-auto snap-x snap-mandatory`, `scrollbar-hide`）。**左端は他コンポーネントと揃える**（`px-5` 内に収め、全幅ブリードはしない）。カード幅 `w-32`、画像枠 `aspect-[3/4] rounded-2xl`、名前は `min-h-[2.5em]` で高さ揃え。飲んだ銘柄は右上にゴールドの丸バッジ（**1杯目は ✓、2杯目以降は杯数の数字**。`min-w-7 h-7` ではみ出さない）。フィルタ（すべて／未飲、選択中は `bg-ink text-paper`、他は `ring-1 ring-ink/15`）。**カードをタップで記録シートを開く**
  3. **きろく** — 記録一覧（新しい順）。**今日の記録はカードで展開表示**（左サムネ＋名前＋「日付 時刻」＋すき度＋メモ）、**昨日より前は日付ごとに折りたたみ**、ヘッダー（日付＋杯数＋▼／▲）をタップで展開（開閉状態は `expandedDays`）。**記録カードをタップで編集シート**を開く。空状態は破線カード
  4. **バッジ** — `grid-cols-3 sm:grid-cols-4`。獲得は `border-gold ring-gold/40 bg-gold/5`、未獲得は `opacity-60`＋アイコン `grayscale`、隠しバッジ未獲得は 🔒＋「???」。末尾に店舗向け QR ページへの控えめなリンク
- **記録シート（`.sheet-overlay` / `.sheet`、`openSheet(beerId, editId)`）**: ビール／記録カードをタップで下からスライドアップ（`rounded-t-3xl`, `max-h-90vh`）。上部にグラバー、ビジュアル＋名前＋スペック、大きめの星評価（ラベルは「お気に入り指数（任意）」、`text-4xl`、タップ選択・再タップ解除, `text-gold`）、メモ、「閉じる」＋保存ボタン。オーバーレイ背景タップで閉じる
  - **新規記録**: 保存ボタンは「✓ 飲んだ！記録する」。下にその銘柄の記録履歴（日付＋時刻）
  - **編集**（記録カードから）: すき度・メモを引き継いで表示、保存ボタンは「保存する」、下部に「この記録を削除」。**日時は元のまま保持**し、すき度／メモのみ更新
- **バッジ獲得トースト（`.toast-wrap` / `.toast`）**: 記録で新しくバッジを獲得すると、画面上部中央に金枠のピル（アイコン＋「バッジ獲得」＋名前）がスライドインし約2.8秒で自動消滅。`complete` はコンプ演出があるためトーストは出さない
- **金色演出**: コンプ率100%到達時に `.celebrate` 全画面オーバーレイ（金のラジアル＋🏆＋火花 `.spark`）。🏆 は回転させず `celebPulse` で拍動のみ。`prefers-reduced-motion` で記録シート／トースト／演出のアニメを無効化
- 入力フォーカス: `focus:border-ink focus:ring-2 focus:ring-ink/10`

### QRページ（店舗向け / `app/qr.html`）
- **スマホ画面に収まる「ただの1ページ」**（印刷ボタン・印刷専用CSSは持たない。スクショして印刷する想定）。`main` は `min-h-[calc(100dvh-4rem-1px)]` ＋ `flex flex-col justify-center` で**ヘッダー下にちょうど収め、スクロールさせない**
- ヘッダーは**アプリと共通**（ロゴ＋`My ÉBIR`＋`APP`＋`β`、右に「← アプリ」）
- **サイトと同じ配色**: 地は `bg-cream`、QR はサイト定番の白タイル（`bg-paper rounded-2xl shadow-sm`）に載せる。ゴールドの eyebrow ＋短い金罫（`h-px w-10 bg-gold/70`）＋見出し「飲んだ一杯を、記録しよう。」＋一言＋QR＋「スマホのカメラで読み取り」＋**URL リンク**（`optimisuke.github.io/ebir-brew-studio/app/`）＋最下部に「20歳未満の飲酒禁止」注記
- QR は `assets/vendor/qrcode.min.js`（qrcodejs）でオフライン生成（190px、`#141414`／`#ffffff`）。`#qr canvas/img` は `max-width:190px; width:100%`。外部送信なし
- フッターに「20歳未満の方の飲酒は法律で禁止されています。」を明記

## 画像（assets/）

| ファイル | 内容 | 使用箇所 |
|---|---|---|
| `logo.jpeg` | ロゴマーク | ヘッダー／ヒーロー／フッター／favicon |
| `key-visual.jpg` | ブランドアート（青い女性） | ヒーロー |
| `aura-de-nada.jpg` | AURA DE NADA ボトル＋グラス | Beers カード |
| `braggot-whisper.jpg` | BRAGGOT WHISPER ボトル＋グラス | Beers カード |
| `bottles-lineup.jpg` | FORTY OF HOPE / IT BRUT のボトル | Beers ラインアップ |
| `aura-de-nada-art.jpg` | AURA DE NADA ラベルアート | 予備（未使用） |
| `braggot-whisper-art.jpg` | BRAGGOT WHISPER ラベルアート | 予備（未使用） |

- Web 用に最適化済み（JPEG・最大辺 ~1100–1200px・各 40–200KB 程度）
- 下地は `bg-cream`、角丸 `object-cover` でトリミング

## アクセシビリティ
- 画像 `alt` は意味に応じて付与（装飾は空 `alt=""`）
- ゴールド塗りボタンは濃い `ink` テキストでコントラスト確保
