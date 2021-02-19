---
title:  "台灣都市計畫特定專用區快速查詢系統"
layout: post
author: adu
---

**使用語言**: Python, JavaScript

**工具、套件**: Folium

**資料來源**: 委託方提供

**需求**: 輸入都市計畫特定專用區地塊的 ObjectID 以快速查詢該地塊資料，包含以地圖顯示位置、使用分區、面積、開發情況等。

<form action="https://dubidub.github.io/industry_land/specificPurposeDistricts" method="get" target="_blank"><button type="submit">特定專用區快速查詢系統</button></form>

[![cover image](/industry_land/SPDcover.png)](/industry_land/specificPurposeDistricts)


## 匯入資料: SHP or GeoJSON

委託方提供的資料為全台灣約 3000 筆都市計畫特地專用區地塊的資料，內容包含有 ObjectID、地塊座標、使用分區、面積、開發情況等，資料格式則是常見的 Shapefile。Shapefile (SHP) 是 ESRI 公司開發的空間資料格式，可用來儲存幾何圖形 (點、線、多邊形) 的位置 (座標) 及相關屬性。Shapefile 也是目前國內外最被廣泛使用的地理資訊系統 (GIS) 資料交換格式。

在處理 shapefile 過程中遭遇了些問題:

**1.	解碼:**
委託方提供的編碼格式為系統預設的UTF8，然而在匯入資料時無論使用UTF8或BIG5都無法順利解碼。

**2.	網頁前端開發整合:**
Shapefile 依賴 GIS 軟體，無法直接於網頁開啟，儘管有如 PostGIS 或 PostgreSQL 此類可儲存空間地理資料的資料庫。

    {'Color1': '0,127,255',
     'Color2': '',
     'OBJECTID': 76,
     'Shape_STAr': 2969.63745117,
     'Shape_STLe': 466.119057123,
     'ä½¿ç\x94¨å\x88\x86': 'é\x86«ç\x99\x82å°\x88ç\x94¨å\x8d\x80(è¨»)',
     'å\x82\x99è¨»': '',
     'å\x88\x86å\x8d\x80ä»£': 'B15000',
     'å\x88\x86å\x8d\x80æ¬¡': 'é\x86«ç\x99\x82å°\x88ç\x94¨å\x8d\x80',
     'å\x88\x86å\x8d\x80ç°¡': 'é\x86«(è¨»)',
     'å\x88\x86å\x8d\x80ç·¨': '',
     'å\x88\x86å\x8d\x80é¡\x9e': 'é\x86«ç\x99\x82å°\x88ç\x94¨å\x8d\x80',
     'å\x8f\x83è\x80\x83é\x9d¢': '2969.63',
     'å\x9c\x96ç¤ºæ\x96\x87': '',
     'å®¹ç©\x8dç\x8e\x87': '',
     'å»ºè\x94½ç\x8e\x87': '',
     'æ\x95´é\x96\x8bæ\x83\x85å': '',
     'è¨\x88ç\x95«_1': 'å\x9fºé\x9a\x86å¸\x82ä¸»è¦\x81è¨\x88ç\x95«',
     'è¨\x88ç\x95«_12': '',
     'è¨\x88ç\x95«å\x8d\x80': 'C0105',
     'è¨\x88ç\x95«æ¡\x88': '',
     'è®\x8aæ\x9b´_1': '',
     'è®\x8aæ\x9b´å\x89\x8d': '',
     'è³\x87æ\x96\x99å\x93\x81': '',
     'è³\x87æ\x96\x99æ\x97¥': '',
     'é\x99\x84å¸¶æ¢\x9d': ''}

考量這次任務規模，決定僅以前端語言開發，不涉後端或資料庫語言，故使用 GeoJSON 檔案格式。GeoJSON 是一種基於 JSON 的地理空間數據交換格式，同樣地可儲存幾何物件的地理要素與屬性。由於他的本質仍是 JSON ，因此更適合用於網頁開發。

GeoJSON 的限制在於僅能使用 WGS1984 座標參考系統，因此在面積計算或小範圍地區的精密定位有其侷限。然此次任務 GeoJSON 毫無疑問是更適合的。

匯入委託方重新提供的GeoJSON檔案，解碼上沒有任何問題，資料均可正常顯示，僅需微調修正資料欄位名稱即可。

    {'crs': {'properties': {'name': 'urn:ogc:def:crs:OGC:1.3:CRS84'},
      'type': 'name'},
     'features': [{'geometry': {'coordinates': [[[[120.57213650661448,
            23.870855909090338],
           [120.57161062408662, 23.871583777543588],
           [120.5716835621168, 23.871629765437515],
           [120.57191162391041, 23.87176981840338],
           [120.57245728353861, 23.872103214205765],
           [120.57307550235205, 23.871247551449443],
           [120.57311476438699, 23.871192905144188],
           [120.57213650661448, 23.870855909090338]]]],
        'type': 'MultiPolygon'},
       'properties': {'OBJECTID': 34217,
        'area': 11298,
        'layer': '彰化縣',
        '使用分': '產業服務專用區',
        '分區次': '產業服務專用區',
        '分區類': '特定服務專用區',
        '參考面': '11365.04',
        '整開情�': '已完成',
        '比率': 99.4,
        '計畫_1': '高速鐵路彰化車站特定區'},
       'type': 'Feature'},


## 生成地圖: Folium

完成資料匯入與簡單資料整理後，下一步是生成底圖。使用 GeoJSON 的好處在於目前多數用來構建網頁地圖的應用程式 (Leaflet, Kepler.gl, etc.) 均可迅速支援。比較後使用 Folium 這個基於 Leaflet 的套件將 GeoJSON 資料快速視覺化呈現於地圖上。

    import folium
    SPZ_map = folium.Map(location=[23.79, 120.65], tiles='cartodbpositron', zoom_start=8)
    folium.GeoJson(SPZ, name='SPZ').add_to(SPZ_map)

僅需要三行程式碼，voilà!

![generate_map](/industry_land/images/generate_map.png)


## 建立互動查詢: JavaScript

有了這個已經載入 GeoJSON 資料的地圖網頁後，接下來的任務便直接移動到網頁前端編寫 JavaScript 的程式碼建立互動查詢。

### 1.	資料輸入方式: 彈出視窗

第一步在網頁載入時要求使用者輸入地塊的 Object ID 進行查詢。

![input_data](/industry_land/images/input_data.png)

### 2.	輸入錯誤

若使用者輸入 ID 不正確或是未輸入任何 ID 時將跳出錯誤信息。如要再次輸入 ID 則須重新載入網頁。

![input_error](/industry_land/images/input_error.png)

### 3.	輸入正確

輸入正確代碼後，地圖將自動 zoom in 到該地塊位置，該地塊並以其他顏色強調呈現。

![input_successful](/industry_land/images/input_successful.png)

### 4.	加入物件資料彈出視窗

最後加入點擊地塊後便會出現該地塊所有屬性資料即大功告成。

![popup](/industry_land/images/popup.png)
