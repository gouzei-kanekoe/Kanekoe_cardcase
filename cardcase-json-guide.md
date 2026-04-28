# 名片盒 JSON 定義草稿

這份文件整理 `cardcase-cards.json` 的資料格式，供之後製作離線編輯器使用。畫面樣式由 `cardcase.html` 控制，JSON 只負責可編輯內容。

## 基本結構
 
```json
{
  "version": 1,
  "meta": {},
  "cards": []
}
```

## meta 欄位

| 欄位 | 用途 | 預設值 |
| --- | --- | --- |
| `ownerName` | 名片盒持有人名稱 | `鐘　聲` |
| `boxTitle` | 頁面標題、右側標題、手機標題 | `鐘　聲　的　名　片　盒` |
| `introMark` | 開場動畫文字 | `名片盒` |
| `idleQuote` | 未選取名片時的大字區文字 | `這就是我的名片盒：翻閱名片來認識鐘聲，還有其推薦作家。` |
| `emptyCaption` | 未選取名片時的提示 | `請點擊頭像` |
| `dataLoadFailedCaption` | JSON 讀取失敗時的提示 | `名片資料載入失敗` |
| `flipHint` | 名片下方翻面提示 | `點擊名片可翻面` |
| `frontHint` | 名片正面右下提示 | `點擊翻面` |
| `drawerLabel` | 手機版名片選單文字 | `名片選單` |
| `drawerOpenText` | 手機版選單開啟文字 | `收合` |
| `drawerCountSuffix` | 手機版選單數量後綴 | `張` |
| `noLinksText` | 背面沒有網站時的文字 | `（尚無連結）` |

## card 欄位

每張名片都是 `cards` 陣列中的一個物件。

| 編號 | 欄位 | 用途 | 編輯器建議 |
| --- | --- | --- | --- |
| - | `id` | 名片唯一 ID | 自動產生，不讓使用者重複 |
| - | `slot` | 名片格子順序 | 用 1 起算，可拖曳排序 |
| - | `isSelf` | 是否為主名片 | checkbox |
| 01 | `image` | 正面頭像、右側格子縮圖 | 圖片路徑或 `null` |
| 01 | `imageAlt` | 沒有圖片時的替代文字 | 建議 1 到 2 字 |
| 02 | `name` | 姓名、格子名稱、下方 caption | 必填 |
| 03 | `role` | 抬頭、身份、分類 | 空白時可輸出 `——` |
| 04 | `intro` | 正面介紹與下方大字展示 | 可換行，輸出為 `\n` |
| 05 | `backImage` | 背面自定義圖片 | 沒填時沿用 `image` |
| 05 | `backImageOpacity` | 背面圖片透明度 | 預設 `1`，編輯器可顯示為 100% |
| 06 | `links` | 背面自定義網站 | 最多 4 項，空項目不顯示 |

## 通用 UI 行為

- 名片格子 hover 時，下方 caption 暫時顯示該格子的 `name`。
- 名片格子 keyboard focus 時，下方 caption 也顯示該格子的 `name`。
- 游標或焦點離開格子後，caption 回到目前已選名片的 `name`。
- 如果目前沒有選取名片，caption 回到 `meta.emptyCaption`。
- 如果 JSON 讀取失敗，caption 回到 `meta.dataLoadFailedCaption`。
- 名片清單上下邊緣使用淡出遮罩，未完整呈現的格子以半透明方式露出，避免硬切邊。

## 01 圖片規則

- `image` 有值時，正面頭像和右側格子都顯示圖片。
- `image` 為 `null` 或空字串時，正面頭像顯示 `imageAlt`，右側格子顯示 `name`。
- `imageAlt` 是圖片替代文字，也是在沒有圖片時的頭像短字。

## 05 背面圖片規則

- `backImage` 有值時，背面顯示該圖片。
- `backImage` 沒有值時，背面沿用 01 的 `image`。
- `backImage` 和 `image` 都沒有值時，不顯示背面圖片。
- `backImageOpacity` 使用 `0` 到 `1`，例如 `1` 是 100%，`0.35` 是 35%。
- HTML 目前也可讀 `0` 到 `100` 的數字，讀取時會自動轉成比例。

## 06 網站連結規則

`links` 最多顯示 4 項。每一項包含：

| 欄位 | 用途 | 範例 |
| --- | --- | --- |
| `type` | 網站類型或平台名稱 | `Penana` |
| `text` | 顯示在名片上的文字 | `@Kanekoe` |
| `url` | 實際連結 | `https://www.penana.com/user/Kanekoe/` |

規則：

- 空白項目不顯示。
- 超過 4 項時，HTML 只讀前 4 項。
- `url` 空白時會使用 `#`，不跳轉到外部網站。
- 舊資料的 `label` 仍可被 HTML 讀取，但新 JSON 建議統一使用 `type`。

## 完整範例

```json
{
  "version": 1,
  "meta": {
    "ownerName": "鐘　聲",
    "boxTitle": "鐘　聲　的　名　片　盒",
    "introMark": "名片盒",
    "idleQuote": "這就是我的名片盒：翻閱名片來認識鐘聲，還有其推薦作家。",
    "emptyCaption": "請點擊頭像",
    "dataLoadFailedCaption": "名片資料載入失敗",
    "flipHint": "點擊名片可翻面",
    "frontHint": "點擊翻面",
    "drawerLabel": "名片選單",
    "drawerOpenText": "收合",
    "drawerCountSuffix": "張",
    "noLinksText": "（尚無連結）"
  },
  "cards": [
    {
      "id": "kanekoe",
      "slot": 1,
      "isSelf": true,
      "image": null,
      "imageAlt": "鐘",
      "name": "鐘　聲",
      "role": "文字創作者",
      "intro": "塵世中的文字創作者\n性別認同是小海豹。",
      "backImage": null,
      "backImageOpacity": 1,
      "links": [
        {
          "type": "Penana",
          "text": "@Kanekoe",
          "url": "https://www.penana.com/user/Kanekoe/"
        },
        {
          "type": "CXC",
          "text": "@Kanekoe",
          "url": "https://cxc.today/zh/@Kanekoe"
        },
        {
          "type": "Discord",
          "text": "見字寫文",
          "url": "#"
        }
      ]
    }
  ]
}
```

## 離線編輯器建議

編輯器第一版可以只做內容欄位，不開放版面與動畫設定。

建議介面：

1. 左側：名片清單，可新增、刪除、排序。
2. 右側：目前名片表單，分成正面、背面、網站三區。
3. 正面區：`image`、`imageAlt`、`name`、`role`、`intro`。
4. 背面區：`backImage`、`backImageOpacity`。
5. 網站區：固定 4 組 `type/text/url`，空白組不輸出。
6. 匯出：產生完整 `cardcase-cards.json`。

輸出前建議檢查：

- `id` 不重複。
- `slot` 依畫面順序重排。
- `links` 移除全空項目，最多保留 4 項。
- `backImageOpacity` 限制在 `0` 到 `1`。
- 沒有圖片時，`imageAlt` 至少保留 1 個字。
