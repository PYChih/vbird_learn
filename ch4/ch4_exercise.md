# ch4_exercise
###### tags: `Linux`
# 例題
- 4-4: 若有一個檔案的類型與權限資料為`-rwxr0xr--`，請說明意義為何
    - `-`代表一般檔案
    - `rwx`代表root可讀可寫可執行
    - `r0x`代表group可讀可執行
    - `r--`代表其他人只能讀
- 4-6: 請使用getfacl查詢/etc/rc1.d, /etc/passed, /dev/vda, /dev/zero個別代表什麼類型的檔案?同時使用ls-l搭配看最前面的檔案類型字元為何
- 4-9: 讓daemon可讀、可寫、可執行checking，讓加入bin群組的用戶可唯獨該檔案，讓其他人沒有權限
    - `chmod 740 [filename]`
- 4-10:
    - 使用root身分移動工作目錄到`/dev/shm`
    - 將`/etc/fstab`複製到`/dev/shm`下
    - 將`/dev/shm/fstab`更改檔名成為newfs
    - 讓newfs的用戶成為sshd、群組成為wheel
    - sshd這個帳號可讀、可寫newfs，wheel群組成員僅可讀，其他人則無任何權限
    - 讓這個檔案的日期設定為前一天的13:30
    - 讓所有的人都可以執行newfs這個檔案(請使用符號法，同時不要更動到既有的權限)
- 4-11:
    - 建立名為myuser2的帳號，該帳號密碼為mypassWORD
    - 建立名為myuser2的帳號，該帳號密碼為mypassWORD
    - 觀察myuser2與myuser3的id情況
    - 觀察`/home`與`/var/spool/mail`這兩個目錄的內容，是否有名為`myuser2`及`myuser3`的檔名存在
    - 使用userdel myuser2刪除帳號(注意，不要加上-r的參數)
    - 再次觀察`/home`與`/var/spool/mail`內容，`myuser2`檔名是否存在?該檔名的權限為何
    - 重新建立名為myuser2的帳號，密碼亦同為mypassWORD，嘗試討論建立國城中出現的問題原因為何
    - 請在tty2以後的終端機，使用myuser2登入系統，登入後是否出現問題?為什麼?
    - 再次使用userdel -r的方式刪除myuser2與myuser3，是否能夠順利刪除?
    - 若無法順利刪除帳號，請以手動的方式自行刪除餘留的使用者加目錄與郵件檔案
- 4-13
    - 使用usermod -G student prouser1將prouser1加入student群組的支援
    - 使用id prouser1發現什麼?原本的progroup是否依舊存在?
    - 請使用usermod --help查詢-a的選項功能為何?
        - append group
    - 請使用usermod -a-Gprogroup prouser1來延伸給予群組
    - 再次使用id prouser1查閱目前prouser1是否有支援三個群組
- 4-15
    - 讓more可以使用mymore執行
        - 把more複製到home下的bin
        - 並且改名為mymor
# 課後練習
- 系統內有名為 /examdata/exam.check 的檔案，這個檔案的 (1)擁有者、 (2)群組 各為何？且 (3)權限是幾分？
    - ls-l查看
    - 第一個是擁有者，第二個是權限
    - 分數用r4w2x1算三個(ugo)
- 承上，該檔案的檔案類型是什麼？
    - 第一個符號(通常是-或d)
- 承上，請問 student 對於 exam.check 這個檔案來說，具有什麼權限？(寫下 rwx 或 --- 等權限標誌即可)
    - student是other只有r--
- 請『依序』進行如下的帳號管理任務：
- 建立三個用戶，帳號名稱分別為： examuser1, examuser2, examuser3 ，三個人都加入 examgroup 的次要群組支援，同時三個用戶的密碼都是『 ItIsExam 』。 (i 與 e 都是大寫字元)
    - `useradd [username]`
    - `groupadd [groupname]`
    - `usermod -G [groupname] [username]`
    - `echo [passwd] | passwd --stdin [username]`
- 建立一個用戶，帳號名稱為 examuser4，密碼為『 ItIsExam 』但這個帳號沒加入 examgroup 群組
    - `useradd [username]`
- 請刪除系統中的 examuser5 這個帳號，同時將這個帳號的家目錄與郵件檔案同步刪除。
    - `userdel -r [username]`
- 有個帳號 myuser1 不小心被管理員刪除了，但是這個帳號的家目錄與相關郵件都還存在。請參考這個帳號可能的家目錄所保留的 UID 與 GID， 並嘗試以該帳號原有的 UID/GID 資訊來重建該帳號。而這個帳號的密碼請給予 ItIsExam 的樣式。(相關建置帳號的指令，請參考 man useradd 等線上文件的說明)
    - 使用 groupadd -g 從GID新增群組
    - 使用 useradd -u 從UID新增user
- 讓 examuser1 額外加入 student 這個群組，亦即 examuser1 至少有加入 examgroup 與 student 群組
    - `usermod -a -G examuser1 student`
- 請進行如下的權限管理任務：
- 使用 root 將 /etc/securetty 複製給 examuser4，且這個帳號要能夠完整使用該檔案才行。
- 建立一個空的檔案，檔名為 /srv/examcheck.txt，這個檔案可以讓 examuser1 完整的使用，而 examuser2 與 examuser3 可以讀取，但不能執行與寫入， 至於 examuser4 什麼權限都沒有。
- examgroup 群組的成員想要共用 /srv/examdir 目錄，而沒有加入 examgroup 的其他人不具備任何權限，應該如何處理？
- /usr/local/bin/mymore 複製來自 /bin/more，但我只想要讓 examgroup 的成員能夠執行 /usr/local/bin/mymore 這個指令，其他人不能執行這個指令。
- 建立一個名為 /examdata/change.txt 的空檔案，這個檔案的擁有者為 sshd，擁有群組為 users，sshd 可讀可寫，users 群組成員可讀， 其他人沒權限。且這個檔案的修改日期請調整成 2012 年 12 月 21 日 (日期正確即可，時間隨便)