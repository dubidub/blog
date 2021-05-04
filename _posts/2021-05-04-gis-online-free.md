---
title:  "《GIS Online》使用說明"
layout: post
author: adu
---

**使用語言**: JavaScript

**工具、套件**: Leaflet.js

**簡介**:
GIS Online 是免費的線上地圖資訊系統。使用者可自行上傳含有地圖空間資料之資料庫、自訂圖資呈現效果、最後匯出成果HTML檔案以供保存或分享。

<form action="https://dubidub.github.io/gis-online-free/zh_tw" method="get" target="_blank"><button type="submit">GIS Online</button></form>

[![cover image](/gis-online-free/resources/cover.png)](/gis-online-free/zh_tw)



這幾個月由於需要將研究的過程、發現與分析結果視覺化呈現在地圖上，前後使用了許多不同的程式語言以及套件進行開發，並將這些工具逐步整理公開在我的Blog上。然而這些圖資視覺化經常依賴開發者本人寫入程式中，因此無論在開發時間以及客製化需求都受到很大的限制。之前開發的《[互動式地圖資料庫視覺化工具](https://dubidub.github.io/blog/geospatial-data-visualization/)》是為了解決這些問題所做的第一個嘗試。

《[GIS Online](https://dubidub.github.io/gis-online-free/zh_tw)》在之前的基礎上全面升級優化，大幅提升使用者製作圖資的自由度，並可將最後製作成果匯出HTML保存與分享。下表整理了這兩個工具的功能比較。

|---| 互動式地圖資料庫視覺化工具 | GIS Online |
|:---:|:---:|:---:|
| 上傳檔案格式 | GeoJSON | GeoJSON、CSV |
| 地理資料格式 | 多邊形 (Polygon) | 多邊形 (Polygon)<br>資料點 (Points)<br>多線段 (Polylines) |
| 地理座標格式 | WGS84 (EPSG:4326) | WGS84 (EPSG:4326) |
| 圖形格式 | 固定 | 可自訂圖案外框粗細、顏色與圖案填滿的顏色、透明度等 |
| 圖層套疊 | 無 | 有 |
| 呈現資料庫數量 | 單一 | 多個 |
| 自訂篩選條件 | 有，單一條件 | 有，使用者可自訂多欄位多筆篩選條件，並可在開始製圖前實時觀察篩選出的所有資料 | 


## 互動機制/使用說明

### 第一步 匯入資料

使用者自行上傳帶有地圖資訊欄位的檔案，目前支援的檔案格式為GeoJSON以及CSV，均是除了SHP外最常見的資料格式。

![blog-1-landing](/gis-online-free/resources/blog-1-landing.png)

如果手邊沒有檔案的話，使用者也可以載入預建好的範例資料庫。接下來將以**2020年台灣死亡交通事故(A1)資料庫**（資料來源：[政府資料開放平台](https://data.gov.tw/dataset/12197)）做示範。

![blog-2-load](/gis-online-free/resources/blog-2-load.png)

載入資料庫成功後，該資料庫就會以卡片方式呈現在控制面板上，使用者可以任意新增資料庫 (如果你的瀏覽器跑得動的話)。載入的資料庫使用者可以選取檢視按鈕來查看資料庫內容。

![blog-3-multi-datasets](/gis-online-free/resources/blog-3-multi-datasets.png)

![blog-4-view-database](/gis-online-free/resources/blog-4-view-database.png)

### 第二步 新增圖層

如果您選取預建好的資料庫，那麼在載入成功的同時系統會自動幫您匯入預設的圖層，您可以選擇新增圖層或修改既有的圖層。選取新增圖層後，系統會跳出圖層設定視窗，您可以重新命名圖層名稱以及篩選需要呈現的資料。接著選取您要呈現的圖資格式（點、線、多邊形），並輸入資料庫中儲存對應的地圖資訊的欄位。以交通事故資料點為例，便是分別輸入資料點的緯度與經度欄位。

![blog-5-add-layer](/gis-online-free/resources/blog-5-add-layer.png)

最後依照您的喜好指定圖案外框與填滿的顏色與其他格式。確認後系統便會開始製圖，並將新增的圖層顯示在控制面板的資料庫卡片中。您可以在同一個資料庫中建立多個圖層，搭配篩選不同資料與指定圖案顏色，您可建立多樣的圖資呈現方式。這些圖層可以隨時隱藏、修改或刪除。

![blog-6-multi-layers](/gis-online-free/resources/blog-6-multi-layers.png)


### 第三步　匯出成果檔案

在完成匯入所有資料庫並建立好圖層後，您可以將成果匯出成為HTML保存或分享給別人，如此一來當您需要重複編輯資料時便不需要每次都從零開始。
匯出資料時您可以選擇是否保留控制面板。建議如果您只是需要單純分享給他人圖資內容的話可以選擇隱藏面板，以免資料被重新編輯。


## 結語

希望這個工具可以某種程度幫助到您。如果您發現這個工具的任何bug、或對工具使用上有任何改善建議，或者希望可以在未來新增某項功能，都歡迎您隨時透過Email（dub.wu.dub@gmail.com）或加入我的[Linkedin](https://www.linkedin.com/in/adu-t-h-wu-5121767b/)與我聯繫!

