# Linux_learn_awk
awk ( 讀取列 )
參考 : 
* http://tuxgraphics.org/~guido/scripts/awk-one-liner.html
* https://www.baeldung.com/linux/print-lines-between-two-patterns
* https://www.cyberciti.biz/faq/sed-display-text/


垂直加總資料
---
參考 : 
* https://blog.longwin.com.tw/2009/02/bash-sum-use-awk-2009/

awk '{sum += $1} END {print sum}'

若數字太大，出現 E 的符號，可以用 printf 來印

awk '{sum += $10} END {printf "%f\n", sum}'

條件
---
awk '{if ($2 > 300 && $2 < 500) {print $0}}' datafile # if 限定條件

計算
---
```Bash
awk的方式
[root@db22 ~]# echo `awk -v x=1 -v y=5 'BEGIN{printf "%.2f\n",x/y}'`
0.20
```
    說明：通過命令行傳遞參數列表，gawk從該列表中獲取參數值：x=2.45，y=3.123。
    乘法運算完成後，printf函數格式化並顯示運算結果，保留小數點後兩位數，
    並將輸出賦給變量product。

判斷 字串 是在第幾個欄位
---
參考 : https://unix.stackexchange.com/questions/206520/determining-column-number-using-string-in-awk
awk 搭配 for , if , printf

範例 iostat 要找尋 %util

Device r/s w/s rkB/s wkB/s rrqm/s wrqm/s %rrqm %wrqm r_await w_await aqu-sz rareq-sz wareq-sz svctm %util

```Bash
iostat -x 1 6 > $dir/$io_chk

x=cat $dir/$io_chk | grep "%util" | awk '{ for(i=1;i<=NF;i++) 
{ if ($i == "%util") printf("%d", i-1) } exit 0 }'

x=$(( $x + 1 ))
```

全欄位撈出 並顯示
---
awk -F : '{out=""; for(i=2;i<=NF;i++){out=out"\n"$i}; print out}'

顯示最後一個欄位
---
awk '{print $NF}' file.txt


參考 : 

* https://www.baeldung.com/linux/print-lines-between-two-patterns
* https://www.cyberciti.biz/faq/sed-display-text/

顯示關鍵字之間字詞
---

情境 1 :
> 要找顯示版本資訊內容
> 如圖 version 與 version 之間是版本資訊

![image](https://user-images.githubusercontent.com/96226780/202674404-09bfcf39-63aa-41ee-985b-7e439b0facf2.png)

- 方案 1 : awk

```bash
awk '/開頭/, /結尾/' file
```

```bash
awk '/version \(s\)/, /version \(e\)/' r.h
```

- 方案 2 : sed

`sed -n '/開頭/, /結尾/p' file`

`sed -n '/version (s)/, /version (e)/p' r.h`

![image](https://user-images.githubusercontent.com/96226780/202674501-a0d53bff-9853-459c-9ead-8539be7ce180.png)

情境 2 : 
>要找尋 lsof 顯示 檔案目錄以及對應檔案

![image](https://user-images.githubusercontent.com/96226780/202677114-d1faf108-9dd7-467e-8569-19a322d786f2.png)

- 方案 3 : grep

`grep -o -P '(?=開頭).*(?=尾部)'`

![image](https://user-images.githubusercontent.com/96226780/202674675-52c50d9f-31c0-426b-8837-6f22a03b395c.png)



