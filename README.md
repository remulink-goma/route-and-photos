# ルートと写真（Route and Photos）

地図データ形式を「絵のフォーマット」として使い、その上にルート（線）を走らせ、
写真を順番に表示する、小さなウェブプレイヤーです。

- 散歩のルートと写真を表示するのが出発点です（`map/`）。
- 舞台は地図でなくても構いません。順序（座標）のある面なら何でもよく、
  `timeline/` は年表の上にアルバムジャケットを並べた例です。
- データの用意（ルートと座標の合わせ込み、写真の配置）は、
  受け取った側の AI が担う前提の設計です。
- 並ぶのは写真だけとは限りません。`album/` は1枚のアルバムの曲順に
  動画のサムネイルを並べた例で、タッチするとその曲の動画が再生されます。
  移動に合わせて、曲をつないだ音を流すこともできます。

操作はこれだけです。

- 再生ボタンを押すと、散歩版はルートが、年表版はポインタが進みます。
- 写真は再生中に順番にポップアップします。
- 写真をタッチすると、全画面で大きく表示されます（もう一度タッチで閉じます）。
- 年表版とアルバム版は、線の上をドラッグしても動かせます。
  どの位置でも、どれか1つのカードが常に出ています。

## 実例（実際に動いているもの）

- 散歩版: https://remulink-goma.github.io/route-and-photos/map/
- 年表版: https://remulink-goma.github.io/route-and-photos/timeline/
- アルバム版: https://remulink-goma.github.io/route-and-photos/album/
- 全アルバム紹介: https://remulink-goma.github.io/route-and-photos/albums-intro/
- 設置見本（ダミーのトップページ）: https://remulink-goma.github.io/route-and-photos/top-sample.html

実際に運用している場所: [https://remu.jp/](https://remu.jp/)（トップページに窓を置き、各ページで使っています）

## 使い方

このフォルダごと、自分の AI（Muse など、コードを扱えるエージェント）に渡して、
自分のデータで作るよう指示してください。データの形式は実例の中のデータが
そのまま見本になっています。

- `map/`： 地図版。ルートデータと写真が HTML の中にデータとして入っています。
  - ルートは「累積距離つきの座標列」です。TCX に地点ごとの距離がなければ、
    座標から計算して作ってください。
  - 写真は `frac`（ルート全体に対する割合 0〜1）と、その位置のルート上の点の
    緯度経度 `pos` を持ちます。表示は `pos` の最寄りのルート点で起きます。
  - `frac` の付け方は、写真の順番に均等割りが標準です（2枚なら 1/3・2/3 付近）。
    正確さより雰囲気で置いてください。
- `timeline/`： 年表版。データは `albums.json`（年・タイトル・画像ファイル名の列）。
  画像は `jackets/` フォルダにファイル名順で置きます。
- `album/`： アルバム版。データは HTML の中の曲リスト（曲名・YouTube の動画ID・開始位置の秒）です。
  サムネイルをタッチすると、その曲の動画が開始位置から15秒だけ再生され、終わると自動で元の画面に戻ります（動画側に操作ボタンはありません）。
  移動中の音は、曲順に短く切ってつないだ1本の音声ファイル（例では `bg4.mp3`）を別に用意して、再生位置に合わせて流しています。
  背景にはアルバムのジャケット画像（`jacket.jpg`）を敷いています。
- `albums-intro/`： 全アルバム紹介版。年表版の並びに、アルバム版の「移動に合わせた音」を足した例です。
  データは HTML の中のアルバム列（年・タイトル・1曲目・ジャケット画像・YouTubeプレイリストID）です。
  移動中の音は、各アルバムの1曲目を4秒ずつつないだ1本の音声ファイル（`bg.mp3`）です。
  ジャケットをタッチすると、そのアルバムの1曲目が15秒流れます（音声ファイルは `clips/`）。
  背景には、今表示しているアルバムのジャケットを大きくぼかして敷いています。
- 写真の並びはファイル名順／リスト順がそのまま表示順になります。
- 各例には `index.html`（ページ全体版）と `embed.html`（他のページに「窓」として埋め込む版）があります。
- `top-sample.html` は窓の設置見本（ダミーのトップページ）です。実際のトップページにどう置くかの参考にしてください。remu.jp のトップページの窓は `embed.html` を iframe で埋め込んだものです。

```html
<iframe src="embed.html" style="width:100%;max-width:640px;height:520px;border:1px solid #cccccc" title="ルートと写真"></iframe>
```

## 名義とライセンス

作： レムリンク−ゴマ（Remu Link–Goma）
設計と判断はレムリンク、実装は AI のゴマ（Muse）が担いました。

ライセンスは MIT です（`LICENSE` を参照）。自由に使って、直して、配ってください。
地図版で使っている Leaflet は BSD 2-Clause ライセンスで、その表示は残してください。

---

## English

**Route and Photos** is a tiny web player that treats map data as a picture format: a route (a line) travels across it, and photos appear in order along the way.

- It started as a way to show walks — a route on a map, with the photos taken on the way (`map/`).
- The stage does not have to be a map. Any surface with order (coordinates) works: `timeline/` lays album covers along a year axis instead.
- Preparing the data — converting a picture into map format, aligning the route and the coordinates, placing the photos — is meant to be done by the receiving side's AI.
- What travels the route does not have to be photos. `album/` lines up video thumbnails along one album's track order; tap one and that track's video plays. A strand of sound, joined from the tracks in order, can also play along with the travel.

That is all the operation there is:

- Press play and the route (or the timeline pointer) runs.
- Photos pop up in order during playback.
- Tap a photo to view it fullscreen; tap again to close.
- The timeline and album versions can also be dragged along the line; exactly one card is shown at every position.

### Live examples

- Walks: https://remulink-goma.github.io/route-and-photos/map/
- Timeline: https://remulink-goma.github.io/route-and-photos/timeline/
- Album: https://remulink-goma.github.io/route-and-photos/album/
- All-albums intro: https://remulink-goma.github.io/route-and-photos/albums-intro/
- Sample placement (dummy top page): https://remulink-goma.github.io/route-and-photos/top-sample.html

In production at: [https://remu.jp/](https://remu.jp/) (windows on the top page, and each page in use)

### How to use

Hand this whole folder to your own AI (an agent that can work with code and files, such as Muse) together with your data, and ask it to build yours. The data inside the examples is the specification — there is no separate manual.

- `map/`: the map version. The route and photos are embedded in the HTML as data.
  - The route is a sequence of coordinates with cumulative distances. If your TCX has no per-point distances, have them computed from the coordinates.
  - Each photo carries `frac` (its position along the route, 0–1) and `pos` (the lat/lng of the route point at that fraction). Display triggers at the route point nearest to `pos`.
  - A good default for `frac` is an even split by photo order (for 2 photos, about 1/3 and 2/3). Atmosphere over precision.
- `timeline/`: the timeline version. Data is `albums.json` (a list of year, title, image filename). Images go in `jackets/`, and list order is display order.
- `album/`: the album version. Data is a track list inside the HTML (title, YouTube video ID, start second per track). Tapping a thumbnail plays that track's video for 15 seconds from its start point, then returns to the route screen on its own (the video itself has no controls). The sound during travel is one pre-joined audio file (`bg4.mp3` in the example), played in step with the position. The album's cover image (`jacket.jpg`) lies under the stage.
- `albums-intro/`: the all-albums intro version — the timeline's line-up plus the album version's "sound that travels". Data is an album list inside the HTML (year, title, first track, jacket image, YouTube playlist ID). The sound during travel is one pre-joined file (`bg.mp3`) of 4 seconds from each album's first track. Tapping a jacket plays 15 seconds of that album's first track (audio files in `clips/`). The current album's jacket lies large and blurred under the stage as the background.
- Photo order is file-name order / list order, and that is the display order.
- Each example comes in two forms: `index.html` (a full page) and `embed.html` (a compact version for embedding as a "window" in another page).
- `top-sample.html` is a dummy top page showing how the windows are placed on a real page. The window on the top page of remu.jp is `embed.html` embedded with an iframe.

```html
<iframe src="embed.html" style="width:100%;max-width:640px;height:520px;border:1px solid #cccccc" title="Route and Photos"></iframe>
```

### Credit and license

By Remu Link–Goma. Design and decisions by Remu Link; implementation by his AI, Goma (Muse).

MIT License — see `LICENSE`. Use it, change it, share it freely.

The map version uses Leaflet (BSD 2-Clause); please keep its attribution.
