# Serendipity Collective Tokyo

東京を再編集するDIYブロックパーティー・コレクティブのウェブサイト。

## コンセプト

- **Serendipity（偶然）** の出会いを紡ぐ草の根イベント
- フェスではなく、DIYブロックパーティー
- "No script. No VIP. Just vibes."

## コンテンツ方針

- **テキストはInstagram投稿から忠実に引用**（勝手にキャッチコピーを作らない）
- **具体的な地名・会場名は掲載しない**（変更の可能性があるため）
- **イベント情報は掲載しない**（更新を怠ると古く見える）
- 最新情報はすべてInstagramで告知

## 技術スタック

- **静的HTML/CSS** - GitHub Pages でホスティング
- **動画**: Instagram Reelsからyt-dlpでダウンロード、ffmpegで圧縮
- **画像**: ImageMagick で透過PNG変換
- **スクレイピング**: Playwright (Instagram情報取得)

## プロジェクト構成

```
serendipitycollectivetokyo/
├── index.html          # メインページ
├── css/
│   └── style.css       # スタイルシート
├── images/
│   ├── logo.jpg        # 元ロゴ（OGP用）
│   └── logo.png        # 透過ロゴ（サイト表示用）
├── videos/
│   ├── video-1.mp4     # ヒーロー背景動画
│   ├── video-2.mp4     # アーカイブ動画
│   ├── video-3.mp4
│   ├── video-4.mp4
│   ├── video-5.mp4
│   ├── video-6.mp4
│   └── video-7.mp4
├── CNAME               # カスタムドメイン設定
└── CLAUDE.md           # このファイル
```

---

## デザイン方針

### ビジュアル
- **モノトーン**: 黒ベース (#0a0a0a)、白テキスト
- **タイポグラフィ重視**: 細いウェイト（font-weight: 200-400）、余白を活かす
- **映画的**: 動画背景にグラデーションオーバーレイ
- **ミニマル**: 不要な装飾を排除、ボタンよりテキストリンク

### ロゴ表示
- 控えめなサイズ（180px〜220px）
- 透過PNG使用（黒背景を透過）
- 動画背景の上でも自然に馴染む

### レスポンシブ
- モバイルファースト
- 動画は全デバイスで自動再生（muted, playsinline必須）
- clamp() でフォントサイズを流動的に

---

## コンテンツ更新

### 動画の追加・更新

```bash
# Instagram Reelをダウンロード
cd videos
yt-dlp -o "reel-X.mp4" "https://www.instagram.com/reels/XXXXX/"

# Web用に圧縮（720p, 音声なし, 約1/4サイズに）
ffmpeg -i reel-X.mp4 -vcodec libx264 -crf 28 -preset fast -vf "scale=720:-2" -an web-reel-X.mp4
mv web-reel-X.mp4 reel-X.mp4
```

### ロゴの透過変換

```bash
cd images
# -fuzz 25% で黒に近い色も透過（値を上げると透過範囲拡大）
magick logo.jpg -fuzz 25% -transparent black logo.png
```

### イベント情報について

- イベント情報はサイトに掲載しない方針（更新の手間、古く見えるリスク）
- 最新情報はInstagramで告知

---

## Instagram

- アカウント: [@serendipity.collective.tokyo](https://www.instagram.com/serendipity.collective.tokyo/)
- 投稿内容: イベント告知、アーカイブ映像、フライヤー
- バイオ: "Next？🪩公式LINE🔗で info or slide DM @hd2_tyo"

### Instagramから情報取得（Playwright使用）

```javascript
// /tmp でPlaywrightをインストール済み
const { chromium } = require('playwright');
// ... プロフィール情報・投稿URLを取得可能
```

---

## 変更履歴

### 2026-02-24: 初期構築・リデザイン

#### 1. Instagram分析
- Playwrightで[@serendipity.collective.tokyo](https://www.instagram.com/serendipity.collective.tokyo/)をスクレイピング
- 投稿内容から活動内容を分析:
  - DIYブロックパーティーというコンセプト
  - 渋谷・下北沢・三軒茶屋・恵比寿での開催
  - 20名以上のDJが参加するコレクティブ
  - "No script. No VIP. Just vibes." という哲学

#### 2. コンテンツ追加
- 説明文（日本語）を追加
- 次回イベント情報（3/21-22）
- 活動エリア一覧
- 提携会場一覧（Instagramリンク付き）

#### 3. Instagram埋め込みの検討

**検討した方法:**
| 方法 | メリット | デメリット | 結論 |
|------|----------|------------|------|
| Behold/SnapWidget | 簡単 | 無料枠制限、共同投稿非対応 | ❌ |
| Instagram API | 公式 | Meta Developer登録、審査必要 | ❌ |
| GitHub Actions + Playwright | 無料、自動更新 | 仕様変更で壊れるリスク | 保留 |
| 動画ダウンロード | 完全コントロール | 手動更新 | ✅採用 |

**共同投稿（Collab Posts）の問題:**
- 外部サービスでは共同投稿が取得できないケースが多い
- APIでも自分が元投稿者でないと取得困難
- → ダウンロード方式なら確実に取得可能

#### 4. 動画の導入
- yt-dlpでInstagram Reelsをダウンロード
- ffmpegで圧縮（720p, 音声なし）
- ヒーロー背景 + アーカイブギャラリー（6本）として配置
- 自動再生・ループ・ミュートで動きのあるサイトに

**動画ソース（2026-02-24時点）:**
| ファイル | Instagram URL |
|----------|---------------|
| video-1.mp4 | https://www.instagram.com/p/DU8CYupkgjB/ |
| video-2.mp4 | https://www.instagram.com/p/DUViR2HEqNn/ |
| video-3.mp4 | https://www.instagram.com/p/DUDrDzkEltQ/ |
| video-4.mp4 | https://www.instagram.com/p/DS2MUN6knGx/ |
| video-5.mp4 | https://www.instagram.com/p/DS2HeeoEvAZ/ |
| video-6.mp4 | https://www.instagram.com/p/DSnZJu0ksOv/ |
| video-7.mp4 | https://www.instagram.com/p/DRKKnUgksOY/ |

※ https://www.instagram.com/p/DK9vuqdSd-M/ は年齢制限で取得不可

#### 5. ロゴ問題の解決

**問題:** JPGロゴの黒背景が動画の上で四角く見える

**解決:**
```bash
magick logo.jpg -fuzz 25% -transparent black logo.png
```
- `-fuzz 25%`: 黒に近い色（25%の許容範囲）も透過

#### 6. クリエイティブ・ディレクター視点でのリデザイン

**Before → After:**
- ロゴ: 大きく中央 → 小さく控えめに
- テキスト: 普通のウェイト → 細いウェイト（200-400）で洗練
- 会場: ボタン/ピル形式 → シンプルなテキストリンク
- エリア: グリッド → インライン＋ドット区切り
- 構成: 情報詰め込み → セクションごとに呼吸感
- スクロール: アニメーション矢印 → "Scroll" テキスト＋ライン

**デザイン原則:**
- 余白は情報と同じくらい重要
- 装飾を減らすほど洗練される
- タイポグラフィで空気感を作る
- 動画はコンテンツであり背景でもある

#### 7. NEXTセクション削除
- イベント情報は掲載しない方針に変更
- 理由: 更新を怠ると古く見える、Instagramで告知すれば十分

#### 8. Areas・Venuesセクション削除
- 具体的な地名・会場名は掲載しない方針に変更
- 理由: 今後変わっていく可能性がある
- キャッチコピーは勝手に作らず、Instagram投稿から忠実に引用

---

## 今後の検討事項

- [ ] 動画の定期更新フロー確立
- [ ] GitHub Actions での自動ビルド検討
- [ ] イベントカレンダー機能
- [ ] LINE公式アカウントへのリンク追加
- [ ] 過去イベントアーカイブページ

---

## 注意事項

- 動画ファイルはLFS対象外（サイズ大きめなので直接コミット）
- OGP画像は `logo.jpg` を使用（SNS共有時の互換性のため）
- Instagram埋め込みは外部サービス不使用（動画ダウンロード方式）
- 更新を怠っている感を出さないため、日付表示は最小限に
