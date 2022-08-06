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

## 結果（程式驗證結果）
![image](https://user-images.githubusercontent.com/51331397/183242680-50e1f8e6-a257-4a04-9bb1-6ac15e063482.png)
![image](https://user-images.githubusercontent.com/51331397/183242684-c40b88a4-946e-47d8-828e-cf9b5e46593f.png)

## 討論
### main.s  
如下圖，name函式的記憶體位置為`0x865c`，執行name函式時返回位置為暫存器`lr`所記錄，值為`0x85a8`，暫存器`pc`所存的位置`0x8660`為執行name函式後下一個欲執行指令的位置。
![image](https://user-images.githubusercontent.com/51331397/183242913-118d4fd5-131b-4288-a81d-a45f1fa20e71.png)  
如下圖，id函式的記憶體位置為`0x8450`，執行id函式時返回位置為暫存器`lr`所記錄，值為`0x85b8`，暫存器`pc`所存的位置`0x8454`為執行id函式後下一個欲執行指令的位置。
![image](https://user-images.githubusercontent.com/51331397/183242957-cb11efd0-4f48-4c9a-9bce-398ed6c6b3df.png)
### name.s  
組別編號(“Team 09\n”)放置在起始位址`0x10b66`，結束位址`0x10b6d`。  
![image](https://user-images.githubusercontent.com/51331397/183243063-fbea0fba-a0de-44e2-8224-f44b9cbbf7c1.png)  
第一位組員名字(“Chen Li Wei\n”)放置在起始位址`0x10b6f`，結束位址`0x10b7a`。  
![image](https://user-images.githubusercontent.com/51331397/183243105-ed232add-a61a-4ed0-9bb7-f28124e67b21.png)  
第二位組員名字(“Wong Yu Kai\n”)放置在起始位址`0x10b7c`，結束位址`0x10b87`。  
![image](https://user-images.githubusercontent.com/51331397/183243135-849ed928-0f68-4dfc-b2f0-17c83983ce64.png)  
第三位組員名字(“Lin Yi Xian\n”)放置在起始位址`0x10b89`，結束位址`0x10b94`。  
![image](https://user-images.githubusercontent.com/51331397/183243159-500e00a9-11fa-46c2-8131-f7625a2baf77.png)  
### id.s  
第一位組員學號(10627109 0x00a22825)放置在起始位址`0x10890`，結束位址`0x10893`。  
![image](https://user-images.githubusercontent.com/51331397/183243245-f6ac115c-58ed-4292-8ed9-052539b7c2aa.png)  
第二位組員學號(10627110 0x00a22826)放置在起始位址`0x10894`，結束位址`0x10897`。  
![image](https://user-images.githubusercontent.com/51331397/183243285-dc6a6997-6b8c-4ab4-86bc-a0319d4d02f8.png)  
第三位組員學號(10627150 0x00a2284e)放置在起始位址`0x10898`，結束位址`0x1089b`。  
![image](https://user-images.githubusercontent.com/51331397/183243313-3a7113e6-178c-4a0c-b6c2-7dd5ceca1a92.png)  
學號總和(31881369 0x01e67899)放置在起始位址`0x1088c`，結束位址`0x1088f`。  
![image](https://user-images.githubusercontent.com/51331397/183243413-4d12d5db-ec89-4ddf-a448-792bcba18d21.png)  

## 結論
開始寫project前，我們對於組合語言的認識還是一知半解，考試也都考爛爛的，對於各種指令在做甚麼都不是很了解，在寫此次作業的過程中，撞了不少牆，常常因為語法不熟悉而想法與寫出來的結果不一樣，卡了好幾小時，後來經摸索與詢問同學，逐漸對語法更加熟捻，才能完成作業。

## 未來展望
經過這次其中project的洗禮，我們對組合語言有更進一步的認識，像是使code block Debug模式去觀察register與記憶體的內容變化，透過這個方式我們對組合語言部分只有更加直觀的認識，希望在期末project可以更加的熟悉組合語言的指令，以期以後能以組合語言實現曾寫之C程式。
