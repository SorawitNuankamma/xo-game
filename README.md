# เกม XO

## วิธีเล่น
สามารถเล่นเกมได้ที่ => https://xo-game-10945.web.app/ .
หรือเล่นเกมได้โดยการ clone project และทำการ npm start ก็ทำได้เช่นกัน.

## ข้อมูลทางเทคนิค

app ของ react ถูก host ที่ firebase.
แอพ backend ถูก host ที่ heroku โดย backend เป็น nodejs กับ express โดยใช้ mongodb atlas เป็นตัว database.
โดยมี repo อยู่ที่ https://github.com/SorawitNuankamma/xo-expressapp.

โดยในส่วนของ backend เป็น express app ที่รับ request ในการ GET replay  และ POST replay ลง database


### ข้อมูลทางเทคนิคของเกม

โครงสร้างของ board ของเกมสามารถปรับได้จาก 3*3 เป็น n*m โดยที่ n,m สูงสุดที่ 100 ( สามารถปรับเพิ่มได้ แต่อาจจะ render แย่และประสิทธิภาพต่ำ).

board จะเก็บข้อมูลในรูปแบบ Array ซ้อน array.
เช่น [[0,0,0],.
     [0,0,0],.
     [0,0,0]].

จะสามารถเข้าถึงข้อมูลได้โดยการทำ table[i][j] โดย i,j แทน row และ column.
แต่ละ element ใน table จะถูก render เป็น controlled component ของ react ที่จะเรียก callback function ที่ parent component ให้มาได้


โดยตัวเกมจะมีอยู่ 3 โหมดคือ
1. play : โดยผู้เล่นสลับเป็น player1 และ player2.
2. play with AI : ผู้เล่นเป็น player 1 และ AI จะควบคุม player 2. 
3. replay : โปรแกรมจะทำการเล่นตามที่มีการบันทึกจาก gameReplay ครั้งละ 1 วินาที.

### ข้อมูลโหมด play

จะเริ่มต้นด้วยการให้ player 1 กด ซึ่ง component ช่องจะเรียก callback function จาก parent เพื่อทำการเปลี่ยน state ของ table. และจะเปลี่ยนไปเป็น player 2 ไปจนกว่าจะมีใครคนใดคนหนึ่งชนะหรือถ้าไม่มีใครชนะจะถือว่าเสมอ.

program จะเช็คการชนะทุกๆตำแหน่งล่าสุด โดยเช็ค 4 แนว.
1.แนวนอน : เช็คทุกๆหลักในแถวของตำแหน่งล่าสุดว่าเป็นค่าเดียวกับ player ไหม หากใช่ ผู้เล่นชนะ.
2.แนวตั้ง : เช็คทุกๆแถวในหลักของตำแหน่งล่าสุดว่าเป็นค่าเดียวกับ player ไหม หากใช่ ผู้เล่นชนะ.
3.แนวทแยงซ้าย : เช็คทุกตำแหน่งในแนวทแยงว่าเป็นค่าเดียวกับ player ไหม และตำแหน่งต้องเป็นแนวทแยงเท่านั้น หากใช่ ผู้เล่นชนะ. 
4.แนวทแยงขวา : เช็คทุกตำแหน่งในแนวทแยงว่าเป็นค่าเดียวกับ player ไหม และตำแหน่งต้องเป็นแนวทแยงเท่านั้น หากใช่ ผู้เล่นชนะ .

### ข้อมูลโหมด play with AI

จะเหมือนโหมด play เพียงแค่ player2 จะถูกตัดสินใจโดย AI ซึ่งมีหลักการดังนี้.

1.หาก round 2 สุ่มตำแหน่งเริ่มต้น.
2.พิจารณาทุกตำแหน่งข้างๆของตำแหน่ง AI หากมีจุดที่ทำให้ชนะ AI จะเข้าไปกดจุดนั้น.
3.พิจารณาทุกตำแหน่งข้างๆของตำแหน่งของเรา หากมีจุดที่ทำให้เราชนะ AI จะเข้าไปกดจุดนั้น.
4.หากยังไม่มีจุดไหนชนะ AI จะพิจารณาทั้ง 4 แนวว่าแนวไหนยังทำให้ชนะได้ และเข้าไปกดจุดนั้น.
5.หากโดนปิดกั้นทุกทิศทาง AI จะสุ่มตำแหน่งกดใหม่.

### ข้อมูลโหมด play with AI

จะเหมือนโหมด play เพียงแค่ player2 จะถูกตัดสินใจโดย AI ซึ่งมีหลักการดังนี้.

1.หาก round 2 สุ่มตำแหน่งเริ่มต้น.
2.พิจารณาทุกตำแหน่งข้างๆของตำแหน่ง AI หากมีจุดที่ทำให้ชนะ AI จะเข้าไปกดจุดนั้น.
3.พิจารณาทุกตำแหน่งข้างๆของตำแหน่งของเรา หากมีจุดที่ทำให้เราชนะ AI จะเข้าไปกดจุดนั้น.
4.หากยังไม่มีจุดไหนชนะ AI จะพิจารณาทั้ง 4 แนวว่าแนวไหนยังทำให้ชนะได้ และเข้าไปกดจุดนั้น.
5.หากโดนปิดกั้นทุกทิศทาง AI จะสุ่มตำแหน่งกดใหม่.

### ข้อมูลโหมด replay

โปรแกรมจะทำการเล่นตามที่มีการบันทึกจาก gameReplay ครั้งละ 1 วินาที โดยจะไล่จากค่าใน Array gameHistory ใน gameReplay .ในระหว่างนี้ช่องจะไม่สามารถกดได้.









