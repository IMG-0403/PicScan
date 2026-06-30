# PicScan

型番を入力すると、同名の画像ファイルを表示するWebアプリです。

## GitHub Pagesで使う場合

GitHub Pagesでは `index.html` をそのまま公開できます。

1. `index.html` をリポジトリ直下に置く
2. 管理者画面を使う場合は `admin.html` もリポジトリ直下に置く
3. 画像ファイルを `images` フォルダに置く
4. GitHub Pagesを有効化する

たとえば `images/ABC-123.png` を置いた場合、Webアプリの型番フィールドには `ABC-123` または `ABC-123.png` と入力します。

## 管理者画面

`admin.html` は画像登録用の管理者画面です。

管理者ログインはアカウント `img`、パスワード `5381` です。

GitHub Personal Access Tokenには、対象リポジトリの `Contents: Read and write` 権限を付けてください。

## ローカルDB版を使う場合

```powershell
python app.py
```

ブラウザで `http://127.0.0.1:8000` を開きます。

## 画像の取り込み

画像ファイルを任意のフォルダに置き、次のコマンドで SQLite に取り込みます。

```powershell
python app.py import .\images
```

ファイル名がそのまま型番になります。たとえば `ABC-123.png` を取り込んだ場合、Webアプリの型番フィールドには `ABC-123.png` と入力します。

## データベース

初回起動時に `picscan.db` が作成されます。画像テーブルは以下です。

```sql
CREATE TABLE images (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    model_no TEXT NOT NULL UNIQUE,
    filename TEXT NOT NULL,
    mime_type TEXT NOT NULL,
    data BLOB NOT NULL,
    created_at TEXT NOT NULL DEFAULT CURRENT_TIMESTAMP
);
```
