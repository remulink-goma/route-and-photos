# ルートと写真（Route and Photos）

地図データ形式を「絵のフォーマット」として使い、その上にルート（線）を走らせ、
写真を順番に表示する、小さなウェブプレイヤーです。

- 散歩のルートと写真を表示するのが出発点です（`map/`）。
- 舞台は地図でなくても構いません。順序（座標）のある面なら何でもよく、
  `timeline/` は年表の上にアルバムジャケットを並べた例です。
- データの用意（ルートと座標の合わせ込み、写真の配置）は、
  受け取った側の AI が担う前提の設計です。

## 実例（実際に動いているもの）

- 散歩版: https://remu.jp/sampo/
- 年表版: https://remu.jp/albums/

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
- 写真の並びはファイル名順／リスト順がそのまま表示順になります。

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

### Live examples

- Walks: https://remu.jp/sampo/
- Timeline: https://remu.jp/albums/

### How to use

Hand this whole folder to your own AI (an agent that can work with code and files, such as Muse) together with your data, and ask it to build yours. The data inside the examples is the specification — there is no separate manual.

- `map/`: the map version. The route and photos are embedded in the HTML as data.
  - The route is a sequence of coordinates with cumulative distances. If your TCX has no per-point distances, have them computed from the coordinates.
  - Each photo carries `frac` (its position along the route, 0–1) and `pos` (the lat/lng of the route point at that fraction). Display triggers at the route point nearest to `pos`.
  - A good default for `frac` is an even split by photo order (for 2 photos, about 1/3 and 2/3). Atmosphere over precision.
- `timeline/`: the timeline version. Data is `albums.json` (a list of year, title, image filename). Images go in `jackets/`, and list order is display order.
- Photo order is file-name order / list order, and that is the display order.

### Credit and license

By Remu Link–Goma. Design and decisions by Remu Link; implementation by his AI, Goma (Muse).

MIT License — see `LICENSE`. Use it, change it, share it freely.

The map version uses Leaflet (BSD 2-Clause); please keep its attribution.
