# 長谷川育美非公式サイト(検)（Hasegawa_Ikumi_Radio）

『長谷川育美公式ラジオ(決)』の非公式検索サイト。見た目と仕組みは、さーち・ざ・らじお！（~/Downloads/Search_The_Radio）に合わせてある。
Claude Pro プランの利用制限に配慮して、読む量を最小にすること。

## 大きいファイルは丸ごと読まない
`style.css`・`main.js`・`admin/admin.js`（約190KB）・`admin/serve.py`（約80KB）は、
まず `grep -n` で場所を探し、必要な行の範囲だけ読む（Read の offset/limit や `sed -n 'A,Bp'`）。

## 読まなくてよいもの
- `admin/backup/`（編集ツールが作る過去の版）・`thumbnails/`（画像）
- `admin/serve.log` は `tail` で最後の数十行だけ
- `episodes.json` などのデータは、必要な回だけ `grep` で探す

## YouTube ではなく AuDee
- 番組は AuDee（audee-membership.jp/hasegawa-ikumi/audio/… ・動画は …/video/…）の配信。会員のログインが要るので、回の情報やサムネを自動では取れない
- AuDee は時刻を指定して再生できない。時刻は表記だけにして、URLに `?t=` や `?time=` を付けない（2026-10-28 ぱせの指示）
- さーち・ざ・らじお！からコードを移すときは、YouTube の部分（動画ID・`withTimeParam`・i.ytimg.com のサムネ・動画プレーヤー）を持ち込まない

## データ
| ファイル | 中身 |
| --- | --- |
| `episodes.json` | 回の一覧（新しい順・1回1行）。episode・title・works（出演作品「作品名@時刻」）・category（ラジオ／動画／ゲスト／地上波。古い回は文字のままのものがある）・date・link・keywords・duration |
| `photomemory.json` | コーナー「フォトメモリー」の話題（title）と写真のポスト（links: label・url） |
| `dokokara.json` | コーナー「あなたの長谷川育美はどこから？」の作品（title） |
| `onshot.json` | スタジオオンショット（date・label・url） |
| `history.json` | ラジオの歴史 |
| `keywords.json`・`readings.json`・`related.json` | 検索用の読み（キーワード／人名・愛称）と関連ワード |
| `thumbnails/<回>.jpg` | サムネ（564×317） |

## 編集ツール
- `admin/start.command` で起動（http://localhost:8800/admin/ ）。止めるときは `admin/stop.command`。`serve.py` は書き換えると自動で入れ替わるので、再起動は不要
- サムネは、AuDee のページの画像を保存してフォームにドロップ・貼り付けすると、564×317 の JPEG にして保存する
- 外から見るプレビュー（Cloudflare のトンネル）とその仕組みは `admin/serve.py` 冒頭の説明にまとまっている

## 編集ツールを直すときは4つのサイトとも直す
編集ツール（admin/）は、さーち・ざ・らじお！（~/Downloads/Search_The_Radio）の編集ツールをもとにしている。
ぼっち・ざ・ひすとりー！（~/Downloads/Bocchi_The_History）・ypik（~/Downloads/ypik）も含めた4つで共通の部分は、4つとも同じように直す。
- `admin/sites.js`・`admin/live.js`・`admin/undo.js`・`admin/savepreview.js` は4つとも同じ中身
- `admin/serve.py` の「編集ツールのサイト切り替え（4サイト共通）」の `SITES` は4つとも同じ
- `admin/admin.js`・`admin/index.html`・`admin/admin.css` はこのサイト用に作り替えてある（AuDee・カテゴリ・出演作品・フォトメモリー・どこから？・オンショット）。さーち・ざ・らじお！の改良を移すときは、同じ意図で移す
