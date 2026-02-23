---
description: 結婚式招待状の静的Webサイトを作成する
---

# 結婚式招待状サイト作成ワークフロー

file:// で開ける静的サイト（index.html / styles.css / main.js / assets/）を生成する。

## 1. プロジェクトディレクトリの作成

`wedding-invite/assets/` ディレクトリを作成する。

## 2. プレースホルダー画像の生成（13枚）

`generate_image` ツールで以下を生成し `assets/` に配置する（後から同名ファイルで差し替え可能）：

| ファイル名 | 内容 |
|---|---|
| `hero-placeholder.jpg` | ヒーロー背景（ガーデン・花・ゴールデンアワー） |
| `couple-placeholder.jpg` | カップル写真（ポートレート） |
| `venue-placeholder.jpg` | 会場外観 |
| `schedule-placeholder.jpg` | 挙式セットアップ |
| `gallery-01.jpg` 〜 `gallery-09.jpg` | ギャラリー用（リング、ブーケ、テーブル、ケーキ、アーチ、招待状、シャンパン、会場内装、手繋ぎ） |

## 3. index.html の作成

1ページ完結のHTML。以下のセクションをアンカーリンクで繋ぐ：

- **Header（固定）** — サイトタイトル＋ナビ（About / Details / Schedule / Access / Gallery / RSVP / FAQ / Contact）、ハンバーガーメニュー（モバイル用）
- **Hero** — 背景画像＋オーバーレイ、タイトル（名前）、日付、CTAボタン
- **About** — 新郎新婦メッセージ＋写真
- **Details** — カード4枚（日時・会場・ドレスコード・回答締切）、住所コピーボタン
- **Schedule** — タイムライン表示（受付→挙式→披露宴→お開き）
- **Access** — 会場情報、最寄り駅、Google Mapsリンク、会場写真
- **Gallery** — 3×3グリッド、クリックでモーダル表示
- **RSVP** — 方式A（外部Googleフォームリンク）＋ 方式B（JSONダウンロードフォーム）
- **FAQ** — アコーディオン開閉（服装・子連れ・受付時間・プレゼント・欠席連絡）
- **Contact** — 新郎・新婦の連絡先（メール/LINE）
- **Footer** — © + Made with love

OGP・Twitterカードのmetaタグも `<head>` に含める。

## 4. styles.css の作成

デザイン方針：
- **テイスト** — 上品・シンプル・余白多め
- **カラー** — 白基調＋ゴールド/ベージュアクセント（CSS変数で管理）
- **フォント** — システムフォント優先（Hiragino / Noto Sans JP / Yu Gothic）
- **レスポンシブ** — モバイルファースト（360px〜）、768px以下で1カラム
- **UIパーツ** — 角丸カード（控えめシャドウ）、Primary/Secondaryボタン
- **アニメーション** — フェードイン（IntersectionObserver用）、CTAグロウ

含めるスタイル：
- 固定ヘッダー（スクロール背景変更）
- ギャラリーモーダル
- FAQアコーディオン
- RSVPフォーム
- トースト通知

## 5. main.js の作成

外部依存なし・Vanilla JSで以下を実装：

1. **スムーズスクロール** — `a[href^="#"]` クリック時
2. **ヘッダースクロール検知** — `scrollY > 60` で `.scrolled` クラス付与
3. **ハンバーガーメニュー** — モバイル用開閉
4. **ギャラリーモーダル** — 画像クリックで開く、Esc/背景クリック/矢印キーで操作
5. **住所コピー** — Clipboard API + `execCommand('copy')` フォールバック
6. **FAQアコーディオン** — クリックで開閉（排他）
7. **RSVPフォーム方式B** — 入力 → バリデーション → JSON生成 → Blob → ダウンロード
8. **フェードインアニメーション** — IntersectionObserver（非対応時は全表示）
9. **トースト表示** — 3秒後自動消去

## 6. ブラウザ検証

`browser_subagent` で `index.html` を `file://` プロトコルで開き、以下を確認：

- [ ] 全セクションが崩れず表示される
- [ ] モバイル幅（375px）でレスポンシブ表示される
- [ ] FAQアコーディオンが開閉する
- [ ] ギャラリーモーダルが開閉する（Escキー対応）
- [ ] 住所コピーボタンが動作する（トースト表示）
- [ ] RSVPフォームからJSONダウンロードできる

## カスタマイズガイド（ユーザー向け）

| 変更したい要素 | 変更箇所 |
|---|---|
| 名前・日付 | `index.html` 内のテキストを直接変更 |
| 画像 | `assets/` 内の同名ファイルで上書き |
| カラー | `styles.css` 先頭の CSS変数 `--color-accent` 等 |
| RSVPリンク | `index.html` 内の `https://forms.gle/XXXXXXXXXX` を実際のURLへ |
| 会場住所 | `index.html` 内の住所テキスト＋Google Maps URLを変更 |
