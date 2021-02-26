---
title:  "全球航線快速查詢系統（Flight Routes Finder）"
layout: post
author: adu
---

**使用語言**: JavaScript, Python

**工具、套件**: Leaflet

**資料來源**: OpenFlights

**簡介**:
簡單輸入起訖地機場，即可查詢兩點間所有理論上存在的航線（最多中轉兩次）。

<form action="https://dubidub.github.io/flight_routes_finder" method="get" target="_blank"><button type="submit">全球航線快速查詢系統</button></form>

[![cover image](/flight_routes_finder/resources/twostops.png)](/flight_routes_finder)



## 資料取得與初步整理: OpenFlights

全球航線快速查詢系統（Flight Routes Finder）使用的機場、航空公司以及航線資料均來自於 [OpenFlights](https://openflights.org/data.html) 這個開源的工具。其中，[機場資料庫](https://raw.githubusercontent.com/jpatokal/openflights/master/data/airports.dat)有 7,698 筆資料，包含機場名稱、IATA 代號、所在城市與國家以及經緯度等資料；[航空公司資料庫](https://raw.githubusercontent.com/jpatokal/openflights/master/data/airlines.dat)有 6,162 筆資料，包含各航空公司名稱、IATA 代號與所在國家等。

![all airports](/flight_routes_finder/resources/airports.png)

最重要的[航線資料庫](https://raw.githubusercontent.com/jpatokal/openflights/master/data/routes.dat)共有 67,663 筆資料，包含起訖機場與航空公司等資料。**需要特別注意，OpenFlights 用來獲取航線資訊的服務已於 2014 年停止更新**，非常可惜；因此，系統的搜尋結果將可能會出現早已停飛的歷史航線資料，或是 2014 年後新增的航線。

進一步觀察航線資料庫，將近 7 萬筆的航線資料 548 個不同的航空公司執飛（同樣地，可能出現某些早已停飛的航空公司，如 US Airways），往返於全球 3,321 個機場。這些航線資料中僅有11筆顯示有中轉站，其餘皆為直飛航班；換句話說，對於中停至少一站以上的航線，這裡必須自行撰寫航段組成的演算法。

![all routes](/flight_routes_finder/resources/routes.png)


## 航線規劃與資料結構: Python, Javascript

給定出發地與到達地的航線規劃至少需要回答兩個問題：

1.	兩地之間存在直飛、中轉一次、中轉兩次或更多的航班？
2.	如果兩地間不存在直飛航班的話，如何找出所有到達地與出發地之間的最短路線？

由 OpenFlights 提供的資料庫已經解決了第一個問題中的直飛航班。至於中轉次數的問題，可使用廣度優先搜尋演算法簡單解決，同時，在進行搜尋演算時將不重複的路線紀錄下來，當搜尋結束時，第二個問題亦已解決。

**問題在於運算時間**。

由於用來放置網頁的 GitHub Pages 僅支援靜態網頁，故航線、機場、航空公司資料都需要在使用時載入。若僅載入 OpenFlights 的直飛航線資料、於使用者輸入起訖點後再進行航線規劃演算的話，當兩地需要中轉兩次的情況下，運算時間粗估將介於10-20秒，嚴重影響使用者體驗；儘管對於直飛或中轉一次航班的運算時間幾乎可忽略不計。另外的方法則是預先建立好的中轉航線資料檔案以供使用時載入，然而存放兩次中轉的資料檔案過於龐大（粗估超過600MB）而使效能不佳。

評估後決定使用折衷方案：預先建立好中轉一次的資料，搭配直飛航線來計算兩次中轉的航線。經過測試，運算時間相當令人滿意。


## 地圖呈現: Leaflet.js

到目前為止，航線查詢系統已可順利找出兩點間所有理論上可能的航線（直航、中轉一次、中轉兩次）以及執飛各該航段的航空公司。接下來的問題是資料呈現給使用者的方式。線上訂票網站如 Booking.com（Kayak）或 SkyScanner 在顯示機票搜尋結果多使用登機牌的方式表列包含如票價、飛行時間、中停等待時間等資訊。

由於 Flight Routes Finder 需要呈現的資料相對單純，故重點在於航段的表現，因此選擇使用地圖以直觀呈現各航線。

Leaflet 提供給網頁應用來建構地圖的開源 Javascript 庫，使用者包括 FourSquare、Pinterest、Flickr 等；我在過去文章裡也常用來作分析資料的呈現。Leaflet 使用上相當簡單，只需要簡單幾行程式碼就能在網頁中插入如下圖的線段、資料點與彈出視窗。然 Leaflet 的限制在於僅供顯示經度 -180 到 180 度的區間，因此在沒有另加處理的情況下將無法呈現跨越換日線的圖形，是美中不足之處。

![leaflet](/flight_routes_finder/resources/leaflet.png)


## 互動機制: Javascript

有了航線規劃功能以及可供呈現的底圖後，接著編寫提供使用者能自行輸入起訖地查詢的互動機制。

### 1. 輸入出發地: 顯示所有直達與中轉一次機場

在選擇好出發地機場後，地圖會呈現該機場所有直達航線以及中轉一次可到達的機場，使用者可直接拉拽地圖查看。

![input origin](/flight_routes_finder/resources/inputorigin.png)

### 2. 輸入到達地

***直飛***

在兩地有直航班次的情況下，地圖將直接彈出視窗呈現執飛航空公司以及兩地距離等資訊。其中距離的計算使用兩點間的大圓弧線，以避免直接使用經緯度產生的誤差。然*需要注意的是此處距離並不等同於實際飛行路線的距離*。

![direct flight](/flight_routes_finder/resources/direct.png)

***中轉***

當兩地間不存在直達航線時將呈現中轉航線，並優先顯示距離最短的航線。使用者可點選其他中轉地查看其他航線。

![one stop](/flight_routes_finder/resources/onestop.png)

中轉兩次的航線通常會顯得更為眼花撩亂。

![two stops](/flight_routes_finder/resources/twostops.png)

搜尋功能還算比較簡單而陽春，並未特別編寫支援一般ＯＴＡ的常見功能，如航空聯盟篩選、對有直航的航線強制搜尋中轉航線、同城市不同機場的中轉航線等進階功能（如由HND前往NRT轉機、DMK往BKK轉機等）。


## 寫在後面

儘管曾身為 pointimize 的一份子，但在這次開發 Flight Routes Finder 過程中並未使用任何來自 pointimize 的資料或程式碼。Pointimize 是功能遠為強大的里程機票與現金機票的比價搜尋引擎，在提供實時現金票價的同時，還使用自身建立的里程機票規則演算法計算兌換各家機票所需的里程數。

簡單的說，Flight Routes Finder 是一個快速查詢兩機場間理論上存在的直飛以及中轉兩次以內的歷史航線的工具，或者稱其為對 OpenFlights 航線資料庫的視覺化工具可能更為貼切。
