# ch5_note
###### tags: `Linux`
# 5.1 權限在目錄與檔案應用上的意義
- 權限設定有三種身分，每種身分又有三種權限(rwx)，不過按照檔案類型的不同(`-d`)，每種權限又有不同的意義
## 5.1.1 目錄檔與一般檔的權限意義
### 權限對檔案的重要性
- 檔案是實際含有資料的地方，包括文字檔、資料庫內容檔、二進位可執行檔等等。因此，權限對於檔案來說:
    - r: 可以讀取檔案的實際內容，如讀取文字檔的文字內容
    - w: 可以編輯、新增或者是修改該檔案的內容(但不包含刪除該檔案)
    - x: 該檔案具有可以被系統執行的權限
### 權限對目錄的重要性
- 檔案實際存放資料，目錄主要的內容在紀錄檔名清單，如果是針對目錄時，rwx意義如下:
    - r: 具有讀取目錄結購清單的權限，表示該用戶可以查詢該目錄下的檔名資料
    - w: 表示用戶具有異動該目錄結購清單的權限
        - 建立新的檔案與目錄
        - 刪除已經存在的檔案與目錄(不論該檔案的權限為何)
        - 將已存在的檔案或目錄進行更名
        - 搬移該目錄內的檔案、目錄位置
    - x:
        - 目錄的x代表的是使用者能否進入該目錄成為工作目錄的用途
## 5.1.2 使用者操作功能
# 5.2 程序管理初探
- 在linux系統運作中，所有在系統上面運作的都是透過觸發程式成為記憶體中的程序後才能順利的操作系統
## 5.2.1 什麼是程式(program)與程序(process)
- 程式: 通常為binary program，放置在儲存媒體中，為實體檔案的型態存在
- 程序: 程式被觸發後，執行者的權限與屬性、程式的程式碼與所需資料等都會被載入記憶體中作業系統並給予這個記憶體內的單元一個識別碼(PID)，可以說，程序就是一個正在運作中的程式
- 同一支程式有時候會被多個使用者執行觸發，因此系統當中可能會有多個程式碼相同的程序存在，此時就需要知道PID
- PID是系統處理記憶體內程序的最重要的參考依據
### 父程序與子程序
- 例子:
    - office依附在圖形介面上，因此一定要啟動X server才有辦法順利運作office
    - 此時可以說Xserver是父程序，office是子程序
    - 不同的使用者登入都是執行`/bin/bash`這個shell，系統管理需要管理員身分的bash才能夠運作
        - 使用su來切換身分時，su會給予一個具有root權限的bash環境，那麼使用者登入的bash就被稱為父程序，由su-取得後的bash就是子程序
## 5.2.2 觀察程序的工具指令
- 程序的觀察大多使用`ps, pstree, top`等指令
### `ps -l`: 僅觀察bash自己相關的程序
- `ps -l`: 僅觀察bash自己相關的程序
    - F(flag): 代表程序的總結旗標，常見為4代表root
    - S(state): 狀態列
        - R(Running): 運作中
        - S(Sleep): 該程式目前在睡眠狀態，但可以被喚醒
        - D: 不可被喚醒的睡眠狀態，通常這隻程式可能在等待I/O的情況
        - T: 停止狀態(stop): 可能是在工作控制(背景暫停)或除錯(traced)狀態
        - z: 殭屍狀態，程序已經終止但卻無法被移除至記憶體外
    - UID/PID/PPID: 代表此程序被該UID所有有/程序的PID號碼/此程序的父程序PID號碼
    - C: CPU使用率(百分比)
    - PRI/NI(Priority/Nice)的縮寫，代表此程序被CPU所執行的優先順序，數值越小代表該程序越快被CPU執行
    - ADDR/SZ/WCHAN: 都與記憶體有關
        - ADDR是kernel function，指出該程序在記憶體的哪個部分如果是個running的程序，一般就會顯示`-`
        - SZ代表此程序用掉多少記憶體
        - WCHAN表示目前程序是否運作中，同樣的，若為-表示正在運作中
    - TTY: 登入者的終端機位置，若為遠端登入則使用動態終端介面(pts/n)
    - TIME: 使用掉的CPU時間，注意，是此程序實際花費CPU運作的時間，而不是系統時間
    - CMD: 就是command的縮寫，造成此程序的觸發指令為何
### 使用pstree與ps aux觀察全系統的程序
- `pstree`與`ps aux`
- `pstree -A`(以ASCII的顯示字元輸出)
- 若需要加上PID與使用者資料，可以直接使用`-up`來加入
- `ps aux`
### top動態觀察程序
- `top`
## 5.2.3 程序的優先序PRI與NI
- 使用nice管理程序優先序
## 5.2.4 bash的工作管理
- 工作管理是用在bash環境下的，也就是說當用戶登入系統取得bash shell之後，在單一終端機介面下同時進行多個工作的行為管理
    - 這些工作所觸發的程序必須來自於你shell的子程序(只管理自己的bash)
    - 前景: 你可以控制與下達指令的這個環境稱為前景的工作
    - 背景: 可以自行運作的工作，你無法使用ctrl+c終止它，可使用bg/fg呼叫該工作
    - 背景中執行的程序不能等待terminal/shell的輸入
        - command&: 直接將command丟到背景中執行，若有輸出，最好使用資料流重導向輸出到其他檔案
        - ctrl+z將目前正在前景中的工作丟到背景中暫停
        - jobs -l 列出目前的工作資訊
        - fg %n 將第n個在背景當中的工作移到前景來操作
        - bg %n將第n個在北京當中的工作變成執行中
# 5.3 特殊權限SUID/SGID/SBIT的功能
- 某些權限主要是針對運作當下所需要的權限來設計的，這些權限無法以傳統權限來歸類，且與操作者的所需要的特殊權限有關，這就是SUID、SGID、SBIT
## 5.3.1 SUID/SGID/SBIT的觀察與功能說明
### SUID的功能與觀察
- 系統的密碼紀錄在`/etc/shadow`內，但是使用者並沒有權限可以更改，不過一般用戶確實有自己修改密碼的需求
- 此時linux使用一種成為set UID(SUID)的技術來處理這方面的疑問
    - 系統設定一個SUID的權限旗標到passwd執行檔上，當使用者執行passwd指令時，就能夠藉由SUID來切換執行權限
        - SUID權限僅對二進位程式有效
        - 執行者對於該程式需要有x的可執行權限
        - 本權限僅在執行該程式的過程中有效
        - 執行者將具有該程式擁有者的權限
### SGID的功能與觀察
- 對檔案來說，SGID的功能為:
    - SGID對二進程式有用
    - 程式執行者對於該程式來說，須具備x的權限
    - 執行者在執行的過程中將會獲得該程式群組的支援
### 總結:
- 介紹了程式以及程序
- 幾個觀察程序的指令
    - top、ps、pstree、
- 設定程序的優先序，以及前景背景執行
- 特殊權限: SUID/SGID/SBIT的介紹