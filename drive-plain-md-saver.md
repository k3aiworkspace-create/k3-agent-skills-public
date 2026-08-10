---
name: drive-plain-md-saver
description: Save or upload plain text Markdown (.md) files to Google Drive without automatic conversion to Google Docs (.gdoc) format. Use when creating, saving, or updating .md files in Google Drive for tools like Obsidian.
allowed-tools: drive
---

# Drive Plain MD Saver

Google Drive APIでテキストデータからファイルを生成すると、通常はGoogle ドキュメント形式（.gdoc）に自動変換されます。このスキルは、自動変換を回避して純粋なプレーンテキストの `.md` ファイル（`mimeType: text/markdown`）として直接Google Drive上に保存する手順を提供します。

## When to Use

- Google Drive上に純粋な `.md` （プレーンテキスト）ファイルを作成・保存したいとき
- Obsidianなどの外部エディタで直接読み込めるMarkdownファイルをGoogle Drive内に置きたいとき
- スケジュール実行や自動化スクリプトでGoogle DriveへMarkdownファイルを出力するとき

## Steps

1. **ローカル環境で `.md` ファイルを作成**

   - UTF-8エンコーディングでMarkdownコンテンツをファイルに出力します。

2. **バイナリ形式でGoogle Driveへアップロード**

   - `drive:create_file` を以下のパラメータで呼び出します：
     - `title`: `"filename.md"`
     - `mime_type`: `"application/octet-stream"` （汎用バイナリデータとして渡すことで自動変換を回避）
     - `text_content`: `""` （空文字）
     - `base64_content`: `"file:///path/to/filename.md"`

3. **目的のフォルダへ移動（必要に応じて）**

   - `drive:update_file` を使用し、`fileId` と配置先の `parentId`（フォルダID）を指定して移動します。

## Gotchas

- `drive:create_file` で `text_content` を使用したり、`mime_type` に `text/plain` や `text/markdown` を直接設定すると、Google Driveの自動変換機能によりGoogle ドキュメント形式（`application/vnd.google-apps.document`）に変換されます。必ず `mime_type: "application/octet-stream"` と `base64_content: "file://..."` の組み合わせを使用してください。