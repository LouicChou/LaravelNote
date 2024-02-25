# Build Multi Vendor Ecommerce Website (2023)

Use Laragon be host. 

Laragon是整合Apache跟mysql的服務程式，方便開發PHP程式

## Change PHP Version
PHP版本下載: https://windows.php.net/downloads/releases/

將要換的PHP版本程式放在C:\laragon\www\之下，
在Laragon程式中點右鍵>PHP，即可切換版本


## VS Code useful extension
- Laravel Extension pack(整合包)

    Uninstall - EditConfig for vscode, Laravel Create view, Laravel Blade wrapper

- One Dark Pro

## Install Laravel
https://laravel.com/docs/10.x/installation

Laragon點擊下方Terminal

install Laravel tool : Enter "composer global require laravel/installer"

create new Project: Enter "laravel new [Project_Name]"

## open project
Laragon中點右鍵>PHP>www，可看到所有建立的專案

## folder structure
- [app]
    - [http]    
        - [Controllers]
        - [Middleware]
        - kernal.php - Middleware中的東西都會註冊在此檔案
    - [Models] - 檢索資料與資料庫通訊
    - [Provider] - 提供者的服務，Router, Event之類的
- [bootstrap] - 基本上不用動
    - [cache] - 系統會放一些暫存文件用於優化
- [config] - 常用
- [database]
    - [seeder] - 假數據庫
    - [migrations] - 重要! 編修資料表
- [public] - 開放檔案，如首頁index.php
- [resource]
    - [css]
    - [js]
    - [views] - 所有HTML程式都在放在這
- [routes] - 所有陸游都放在這
- [storage] - 舊版本檔案?
- [test] - 測試檔案可放在這
- [vendor] - 所有laravel依賴庫(連結外部工具庫Libraries)
- .env - 環境變數，放資料庫連接字串或密碼之類的東西
- artisan - 
- composer.json - 所有庫之間關係的內容
- package.json - 有安裝的NPM會在這

## artisan
查看artisan命令 - 在Cmder(terminal)輸入"php artisan"

常用命令:
- "php artisan serve" - 查看laragon伺服器的狀態與ip跟port，8000是laragon的預設PORT
- 所有命令後面加上"-h"就可以查看系統功能指令有哪些，譬如輸入"php artisan serve -h"
- "php artisan route:list" - 顯示所有應用程式的路由列表
- "php artisan tinker" - 進入php的即時運算功能，像是輸入"7 == "7"",會得到Ture的結果，輸入"strlen("ABCDE")"會得到5，可用來在網站運行中創建假數據測試之類的







