---

# JSON Formatter & Viewer ✨

貼り付けた JSON を**一瞬で整形**し、**色付け（シンタックスハイライト）**して表示。
**エラー位置（行・列）もピンポイント表示**してくれる、**単一HTMLファイル**のシングルページアプリです。

> 中2でもいける：左に貼って、ボタンぽち。赤い波線のとこ直せばOK！

---

## 🔎 これでできること

* ✅ **整形（pretty print）**：読みやすい標準JSONに整える
* ✅ **ミニファイ**：1行に圧縮（通信や保存に便利）
* ✅ **JSON5入力に対応**：コメント、シングルクォート、末尾カンマなど「ちょい壊れ」もOK
* ✅ **エラー表示**：赤い波線＋下部リスト。クリックで該当行へジャンプ
* ✅ **コピー／ダウンロード**：入出力ともワンクリック
* ✅ **ドラッグ＆ドロップ**や**ファイル読み込み**に対応
* ✅ **ダーク／ライト**テーマ切替（自動保存）
* ✅ **自動保存**：入力内容はリロードしても残ります

---

## ⚡ すぐ使う（3通り）

> プロジェクトは「単一HTML」だけ。`index.html` を用意すればOKです。

### A. VS Code で右クリックして開く（おすすめ）

1. VS Code のエクスプローラーで `index.html` を右クリック
2. **Open with Live Server** を選ぶ
3. ブラウザが開いたら、左にJSONを貼り付け！

### B. ターミナルで超簡単サーバー

```bash
# フォルダを開いた状態で
# Windows
py -m http.server
# macOS / Linux
python3 -m http.server
```

表示された `http://localhost:8000/` を開き、`index.html` をクリック。

### C. ダブルクリック（file:// 直開き）

`index.html` をダブルクリックでも表示可能。
※最初に必要なライブラリをCDNから取るため、**ネット接続は必要**です。

---

## 🧭 基本のながれ

1. 左のエディターに JSON を**貼り付ける**（コメントや末尾カンマがあってもOK）
2. 自動で解析 → 右側に**色付きで整形表示**
3. エラーがあれば**赤い波線**＋下部に**エラー一覧**（クリックで移動）
4. 上部ボタンで

   * **整形**（読みやすく）
   * **ミニファイ**（1行に）
   * **コピー**・**ダウンロード**・**テーマ切替**・**入力クリア**

---

## ⌨️ キーボードショートカット

| 操作            | ショートカット                  |
| ------------- | ------------------------ |
| 整形（Pretty）    | `Ctrl/⌘ + Enter`         |
| ミニファイ（Minify） | `Shift + Ctrl/⌘ + Enter` |

---

## 🧪 サンプルデータ（コピペで試せる）

### 1) 正しいJSON（整形の見本）

```json
{
  "app": "JSON Formatter & Viewer",
  "version": "1.0.0",
  "user": { "id": 12345, "name": "しまリン", "active": true },
  "items": [
    { "id": 1, "title": "ネコ", "price": 1200.5, "inStock": true },
    { "id": 2, "title": "イヌ", "price": 980, "inStock": false }
  ],
  "notes": "絵文字OK 🦊✨"
}
```

### 2) JSON5 としては OK（入力OK→出力は標準JSON）

```js
// コメントOK（JSON5）。出力は標準JSONに整形されます
{
  unquotedKey: 'single quotes OK',
  trailingCommaAllowed: [1, 2, 3,],
  hexNumber: 0xff,
  message: 'JSON5としては有効',
}
```

### 3) あえて壊したJSON（エラー検出の見本）

```js
{
  // 閉じない文字列（エラー行/列が出ます）
  "message": "ここで終わってない,
  "value": 42
}
```

> 追加のサンプルが欲しければ、もっと用意するよ！

---

## 🧩 技術メモ（やさしめ）

* **Monaco Editor**：VS Code の中身。入力・出力の2画面で使っています
* **JSON5**：コメントや末尾カンマなど “ゆるいJSON” を読み込めるライブラリ
* 入力は **JSON5としてパース** → 出力は **標準JSON** に整形（`JSON.stringify`）
* エラーは **Monaco のマーカー**（赤波線）＋下部一覧で表示
* **テーマ**は Tailwind のダーク/ライトと **Monaco の `vs-dark/vs`** を同期
* **ローカル直開き対策**：Web Worker を **data URL** で差し替え済み（`file://`でもOK）
* 初回表示時に **CDN へアクセス**（Tailwind/Monaco/JSON5）

---

## 🧰 トラブルシューティング

* **「Cannot GET /…」が出る**
  → サーバーの**ルートとURLのパスがズレ**ています。
  対策：`index.html` を**右クリック → Open with Live Server**。
  フォルダ名は **空白・`&` を使わない**（例：`json-formatter-viewer`）。

* **`Unknown at rule @apply` が出る**
  → Tailwind の `@apply` は**ビルド時**向け。CDN運用（単一HTML）では使えません。
  本プロジェクトでは **純CSSで置き換え済み** です。

* **真っ白／何も表示されない**
  → ネット接続（CDN）が必要。ブラウザのデベロッパーツールのエラーも確認してね。

* **文字化けっぽい**
  → ファイルを **UTF-8** で保存。

---

## ❓ よくある質問

* **オフラインで使える？**
  初回にCDNからライブラリを読むので**最初はオンライン必須**。その後のキャッシュがあっても、更新時は再取得が発生します。

* **データはどこかに送られる？**
  いいえ、**ブラウザ内のみ**で処理します（ローカルで完結）。

* **サイズの上限は？**
  数MB程度なら快適。極端に大きいJSONはブラウザのメモリに依存します。

* **対応ブラウザ**
  最新の Chrome / Edge / Firefox / Safari を想定。

---

## 🔧 開発者向け（拡張したい人へ）

* **Tailwind を本格導入して `@apply` を使う場合**

  1. `npm i -D tailwindcss postcss autoprefixer`
  2. `npx tailwindcss init -p`
  3. `input.css` に

     ```css
     @tailwind base;
     @tailwind components;
     @tailwind utilities;
     ```
  4. `npx tailwindcss -i ./input.css -o ./output.css --watch`
  5. HTML から `output.css` を読み込む
     → ただしこのプロジェクトは **単一HTML** を主眼にしているため、CDN＋純CSS版が簡単＆おすすめ。

---

## 🙌 ありがとう

* Editor: [Monaco Editor（VS Codeエンジン）]
* Parser: JSON5
* Style: Tailwind CSS（CDN）

---

> 迷ったら「左にペースト → 整形 → 赤波線の場所を直す → もう一回整形」でOK。
> 完璧主義はスリープにして、**まず一歩を気軽に**出そうね😌✨
