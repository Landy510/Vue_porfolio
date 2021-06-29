# 電商網站: Berserker Fitness
![Berserker Fitness](https://upload.cc/i1/2021/04/22/y1iqKv.png "Berserker Fitness")

## 專案發想
因為自己本身喜歡健身，加上自己居住的地方，附近的健身房大多都沒有屬於自己的健身房網站，所以，想要自己先來做一個健身房的網站。未來希望有機會可以為這些健身房量身打造屬於他們的官網。 

在這個網站中，可以將網站管理者自己想要的課程利用後台上架。若使用者在前台選購了想要的課程，並完成付款後，網站管理者可以在後台看到完成付款的使用者相關資料。
## 專案Demo連結
[Berserker_Fitness](https://landy510.github.io/Display_Vue_porfolio/)	

## 修正功能
### 用不一樣的方法來修正課程頁面路由不會改變的問題
在課程頁面，改由 beforeRouteUpdate 功能，來修正，在相同元件下，改變路由但是頁面內容不會重新渲染的狀況。