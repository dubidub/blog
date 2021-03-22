---
title:  "《互動式地圖資料庫視覺化工具》說明"
layout: post
author: adu
---

**使用語言**: JavaScript

**工具、套件**: Leaflet

**簡介**:
使用者自行上傳地理圖資資料，藉由此互動式工具快速對照資料庫資訊與地理空間資訊。

<form action="https://dubidub.github.io/geospatial_visualization" method="get" target="_blank"><button type="submit">互動式地圖資料庫視覺化工具</button></form>

[![cover image](/geospatial_visualization/resources/cover.png)](/geospatial_visualization)



由於近來的研究內容及個人對地理空間資料分析與視覺化的興趣，常常需要使用相關的工具或套件進行資料的整理與呈現。除了使用專為地理資訊開發的GIS軟體外，最常用的即是 python 的 folium 套件、Leaflet.js、Kepler.gl等工具。

這些工具儘管進入的門檻不算太高，但對於初階的學習人員、或是僅需要觀察對照資料的地理空間位置與資料點內容的需求來說，或許GIS工具使用上仍不夠親切，更別說要另外學習程式語言了。因此基於這個前提開發了《**互動式地圖資料庫視覺化工具**》。

這個小工具不追求前述工具的強大資料分析功能，而專注在地理空間資料庫的資料呈現。使用者自行上傳資料後，小工具將資料庫中的資料點呈現在地圖上，並透過欄位的篩選讓使用者快速找到並定位目標資料點。網站的特性也讓使用者可以不需要安裝任何軟體、僅需要瀏覽器即可快速使用。


## 支援格式

| | 目前支援 | 預計未來支援 |
|---|-------------|--------------|
| 上傳檔案格式 | GeoJSON | Excel 檔案 (xls, xlsx) 、CSV |
| 地理資料格式 | 多邊形 (Polygon) | 資料點 (Points)、多線段 (Polylines) |
| 地理座標格式 | WGS84 (EPSG:4326)，即一般常用的經緯度 | 未定 |


## 互動機制 / 使用說明

### 第一步 上傳資料

要使用這個工具首先需要有地理圖資的 GeoJSON 檔案，接下來會以[台灣各縣市村里界](https://sheethub.com/data.gov.tw/%E6%9D%91%E9%87%8C%E7%95%8C%E5%9C%96(WGS84%E7%B6%93%E7%B7%AF%E5%BA%A6))檔案做示範。

點擊選擇檔案選取需要上傳的檔案，你可以使用自己的檔案，或者載入準備好的範例檔案。由於台灣村里界檔案頗大(約60MB)，因此載入時間稍長(約10秒)。載入完成後將會看到村里界線的圖資顯示在地圖上。

![step1](/geospatial_visualization/resources/step1.gif)

### 第二步 選擇資料編號欄位

資料編號欄位記錄資料庫中每筆資料的獨特編號，這個獨特編號應具有不重複的特性。指定編號欄位的目的在於告訴系統如何找到每筆資料，而不與其他資料混淆。

### 第三步 使用資料編號快速查詢圖資

指定資料欄位後下一步你可以直接輸入資料編號快速搜尋該筆資料。

![step2](/geospatial_visualization/resources/step2.gif)

### 第四步 使用其他欄位篩選資料

如果不清楚資料編號，也可以利用篩選器讓系統列出想要的符合範圍的資料後，點選資料編號以搜尋地圖定位。

![step4](/geospatial_visualization/resources/step4.gif)


## 資料安全

「**互動式地圖資料庫視覺化工具**」的開發全程使用 JavaScript 語言，而未使用任何伺服器或資料庫語言。換句話說，你所上傳的任何資料均只存放在你個人的瀏覽器內使用，不會外傳到外部的伺服器或資料庫。使用完畢後只需要重新整理或關閉網頁，所有資料均不會以任何形式進行保存。