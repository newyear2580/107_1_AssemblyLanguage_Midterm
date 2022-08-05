# 107_1_AssemblyLanguage_Midterm
107-1 組合語言 期中專案

> 老師：朱守禮  
> 組別：第九組  
> 班級：資訊二甲  
> 學生：10627109  陳力維  
> 學生：10627110  王昱凱  
> 學生：10627150  林易賢  

## 背景
主程式MAIN包含兩個函數：NAME, ID，共同構成一個能列印組別、組員名字、與學號，並完成學號加總運算的專案。所有程式均需以ARM Assembly完成。  
- 程式需符合「Project基本要求」：
  - 撰寫 3 個Function：NAME, ID, 與MAIN，並分成三個組合語言檔案name.s,id.s與main.s。
    - Name函數需要印出組別與組員名單。
    - Id函數功能是個別輸入組別學號，並印出學號與學號總和。
    - Main函數需要整合前兩個函數的功能，並印出組員學號、姓名與學號總和。
  - 使用 Load/Store指令中，10種定址模式當中的3 種以上。
  - 使用 Data Processing 指令中，13 種Operand2 格式的當中1 種以上。
  - 程式中需包含 3 道以上的非`Branch`指令的Conditional Execution。
  - 在 name.s 程式中需包含1 道一定要執行的指令：`adcs r0, r1, r2`。

## 方法
- name.s  
asciz規劃記憶體空間，把需要印出的字串放入。用`label`指向該記憶體位置，以虛擬指令`ldr  r0, =label` 將位置load到`r0`內，然後再呼叫`printf`印出字串，重複此步驟至所有要求內容印完。
- id.s  
asciz規劃記憶體空間，把需要印出的字串放入。用`label`指向該記憶體位置，以虛擬指令`ldr  r0, =label` 將位置load到`r0`內，然後再呼叫`printf`印出字串，重複此步驟至所有要求內容印完。用`scanf`與`printf`前給一個像是`%d`、`%s`等的型別，再給一個記憶體位置，這樣才能順利`scanf`或`printf`，另外，`scanf`後,再將位置的值assign給暫存器，供後來要用的時候可以用。在呼叫`add`的時候，欲相加的兩個暫存器所存放的值應為數字而不是位置。
- main.s  
整合上述兩函式功能，運用`bl`指令呼叫上述兩函式，暫存器共用，所以在呼叫函數前，若是有不想要改到的值，應該在進函式後第一步先把這些值備份到stack中，函式執行完再還原回去。  

我們在主程式`main`一開始，將零放入`r0`中，再用`subs , r0 , #1`，我們用這些命令的目的是想要看看其他條件執行(conditional execution)，再執行完上述指令後，`CPSR`的`N`值為`1`，`Z`值為`0`，所以我們在下一道指令`ldrmi  r0 , =fun2`，加上`mi`這個條件之後再執行，結果如我們所料，程式能順利印出`fun2`的內容。  
![image](https://user-images.githubusercontent.com/51331397/183023434-0b5ee3f6-c085-4de7-aa3f-a7ef9765aa3e.png)
