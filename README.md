# PicScan

登録名または4桁の登録番号を入力すると、登録済み画像・PDFとコメントを表示するWebアプリです。

## GitHub Pagesで使う場合

GitHub Pagesでは `index.html` をそのまま公開できます。

1. `index.html` をリポジトリ直下に置く
2. 管理者画面を使う場合は `admin.html` もリポジトリ直下に置く
3. 画像・PDFファイルを `images` フォルダに置く
4. GitHub Pagesを有効化する

管理者画面から登録すると、リポジトリ直下の `catalog.json` に登録名、4桁登録番号、画像・PDF、コメントの紐づけが保存されます。
1つの登録名に画像・PDFを合わせて最大6件登録でき、1つの登録名に複数の4桁登録番号を紐づけできます。

`catalog.json` がない場合は、従来通り `images/ABC-123.png` のような画像ファイル名でも検索できます。

## 管理者画面

`admin.html` は画像・PDF登録用の管理者画面です。

管理者ログインはアカウント `img`、パスワード `5381` です。

GitHub Personal Access Tokenには、対象リポジトリの `Contents: Read and write` 権限を付けてください。

管理者画面では登録名、複数の4桁登録番号、画像・PDFを合わせて最大6件、コメントを登録できます。
新しい紐づけは `catalog.json` に保存されます。互換用として画像コメントは従来の `comments.json` にも保存されます。

`catalog.json` の例:

```json
{
  "records": [
    {
      "name": "商品A",
      "numbers": ["1234", "5678"],
      "images": [
        {
          "file": "商品A_1.jpg",
          "comment": "正面"
        }
      ]
    }
  ]
}
```

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
