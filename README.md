**Authors: 107321047 107321046 22組**        
 
**Input/Output unit:**                 
 - 8x8 LED 矩陣，用來顯示對戰畫面。下圖為按下 clear 的初始畫面。
	- ![圖片](/images/img1.jpg)
 - 七段顯示器，用來顯示存活時間。LED燈，用來顯示血量。
	- ![圖片](/images/img2.jpg)
**功能說明:**         
     紅色為人物，要上下左右閃躲來自四面八方的綠色炸彈以及定點炸彈，碰到炸彈會扣血，血量歸零即遊戲結束。

**程式模組說明:**              
　 
   
 >  output reg [7:0] DATA_R, DATA_G, DATA_B           //分別控制個顏色的亮燈情形   
 　 output reg [2:0] COMM                             //控制8*8LED矩陣哪一行亮        
　  output reg EN                                     //8*8LED的使用必須為1     
    output reg [3:0] Life                            //控制血量          
    output reg [6:0] d7_1                            //控制7段顯示器        
    output reg [1:0] COMM_CLK                        //控制哪幾顆7段顯示器         
    input CLK, clear,                                //控制頻率的CLOCK以及重新開始按鍵        
  > input Left, Right, Up,Down,                      //控制上下左右        
    
   

1.人物以及掉落物更新模組
  - reg [7:0] people 為記錄人物的位置
  - e.g: people={00111111} =>代表某一行的第6以及第7燈LED燈會亮
  - reg [7:0] plate 為障礙物的位置 
  - 此模組會依據 input { Left,Right,Up,Down }，分別更新people的值 
  - 障礙物的值當由上跑至下或由左往右完成後，會再隨機產生一組位置，進行下一次的掉    落物。  

2.遊戲模組
  - 將people輸出給DATA_R，就能顯示人物的位置 (顏色為紅色)
  - 將plate 輸出給DATA_G，就能顯示掉落物的位置 (顏色為綠色)
  - reg [2:0] COMM 控制哪一行LED燈亮 e.g:011 => 3 => 代表第4行LED燈會亮
  - 此模組會從000 加到 111 使8排LED都亮，使用適當的頻率並利用視覺佔留，就能感覺8排LED是同時亮的。也會在此控制血量 Life=1111時，為4格血，LED燈亮4 bits 。


3.計時
- 利用CLK累加，並使用秒數轉7段顯示器模組，最後再用7段顯示器暫留模組進行輸出     

4.PIN角對應表                                  
    ![圖片](/images/img3.png)
    ![圖片](/images/img4.png)  
    
**Demo video**
- https://drive.google.com/file/d/1YVclNP5fJDwicwQuKd08m4VIE4x9nCq/view?usp=sharing



