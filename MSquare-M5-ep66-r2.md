## MSquare Programing Fullstack Course
### Episode-*66* 
### Summary For `Room(2)`
### Connecting tables in sql database
- SQL database တွေဟာ relational database အမျိုးအစားတွေ ဖြစ်ကြပါတယ်
-  SQL database ထဲရှိ table တွေဟာ တစ်ခုနဲ့ တစ်ခု ချိတ်ဆက်ပြီး အသုံးပြုလို့ ရပါတယ်
### why relation?
- SQL မှာ dataတွေကို table နဲ့ သိမ်းလေ့ရှိပါတယ်
- တကယ်လို့ data တွေကို table တစ်ခုထဲ စုပြီး သိမ်းထားမယ်ဆိုရင် 
  - data တွေ ထပ်လာမျိုး ရှိလာပါမယ် ( data duplicate)
  - အဲ့ဒီ table ထဲက data တစ်ခုခုရှာမယ်ဆို သိမ်းထားတာ တွေ များနေတာမလို့ အချိန်ကြာပါမယ်။
  - မလိုအပ်ပဲ resource တွေ အများကြီး သုံးရပါမယ်
- ဒါကြောင့်မလို့ SQL မှာ table တွေ အများကြီး လုပ်ပြီး တစ်ခုနဲ့ တစ်ခု ချိတ်ဆက်ပြီး သုံးလေ့ရှိပါတယ်။

##
### *primary key*(PK) and *foreign key*(FK) 
- table တစ်ခု ကို နောက် table တစ်ခုနဲ့ ချိတ်ဆက်သုံးတဲ့ အခါ *foreign key* အနေနဲ့ ချိတ်ဆက်ပေးလေ့ရှိပါတယ်။
### နမူနာ
![enter image description here](https://res.cloudinary.com/practicaldev/image/fetch/s--UlPvjfOb--/c_limit,f_auto,fl_progressive,q_auto,w_880/https://i.postimg.cc/cHDd4FyP/one-to-many-erd-generic.png)
- parent table ကို child table နဲ့ ချိတ်ဆက်လိုတဲ့အခါ parent ရဲ့  id ( PK ) ကို child table ထဲမှာ ( FK ) အနေနဲ့ column တစ်ခု ထည့်ပြီး ချိတ်ဆက်ပေးရပါမယ်။

##
### Relationship in SQL
- SQL dbမှာ table တွေ ချိတ်ဆက်တဲ့ အခါ  အများအားဖြင့် relation သုံးမျိုးဖြင့် ချိတ်ဆက် အသုံးပြုကြပါတယ်
### one to one relation 
### one to many relation
### many to many relation
##
### one to one relation 
- data တစ်ခု ကနေ  တစ်ခု ဆီကိုပဲ ချိတ်ဆက်တာကို ဆိုလိုတာပါ။
- ဥပမာ ။   ။
   ![enter image description here](https://vertabelo.com/blog/one-to-one-relationship-in-database/1-to-1-pk-fk.png)
   - country နဲ့ capital table နှစ်ခုရှိတယ်ဆိုပါစို့
   - အဲ့ဒီနိုင်ငံ အတွက် မြို့တော်ဟာ တစ်မြို့ပဲ ရှိတာမလို့ country id ( PK ) ကို capital မှာ FK အဖြစ် တစ်ခုပဲ ချိတ်ဆက်ထားရမှာကို ဆိုလိုတာပါ
   ##
   ### one to many relation
   - တစ်ခုကနေ အများကို ချိတ်ဆက်ပေးတာဖြစ်ပါတယ်။
   - ZOOM meeting တစ်ခုကို user အများ join ထားသလိုမျိုးပါ။
   - နောက် ဥပမာ တစ်ခုက Facebook မှာ user တစ်ယောက်က status post အများကြီး တင်ထားသလိုမျိုးပါ။
  
```mermaid
graph LR
A[user] --> B(one to many)

B--> C((Post 1))
B--> D((Post 2))
B --> E((Post 3))
```
##
### many to many relation
- အများနဲ့ အများ ကိုချိတ်ဆက်တာဖြစ်ပါတယ်
```mermaid
graph LR
A[stationA] ---> B(many to many)---> C(Buss 1)
D[stationB] ---> B(many to many)---> E(Buss 2)
F[stationC] ---> B(many to many)---> G(Buss 3)
A-->E
A-->G
D-->C
D-->G
F-->E
F-->C




```
- station A က bus 1 ,2,3 နဲ့ ချိတ်ဆက်ထားသလို
- bus 1 ကလည်း station A,B,C ကို ချိတ်ဆက်ထားပါတယ်
- ကျန်တဲ့ station တွေကလည်း bus အားလုံးနဲ့ ချိတ်ဆက်ထားသလို
- bus အားလုံးကလည်း station အားလုံးနဲ့ ချိတ်ဆက်ထားတာမလို့၏
- many to many relationship ဖြစ်ပါတယ်။
##
## data modeling
- project တစ်ခုကို စလုပ်တော့မယ်ဆိုရင်
### database ကို ဘယ်လိုပုံစံ ထားမယ်
### ဘယ် table တွေက ဘယ် table တွေနဲ့ ချိတ်ဆက်ထားရမယ်
### ဘယ်table တွေကိုတော့ ကြားခံ join table တွေနဲ့ ချိတ်ဆက်ပေးရမယ်
### data တွေ duplicate မဖြစ်အောင် ဘယ်လိုလုပ်သင့်သလဲ
- စသည်ဖြင့် **အရင် ကြိုပြီး plan ချ စဥ်းစား**ရလေ့ရှိပါတယ်။
- အဲ့ဒါကို **data modeling** (**data normalization**)လို့ ခေါ်ပါတယ်။
- အရမ်းကို အရေးကြီးတဲ့ အဆင့် ဖြစ်ပါတယ်။
- **data modeling** မှန်ကန်မှသာ App က အဆင်ပြေပြေ အသုံးပြုနိုင်မှာ ဖြစ်ပါတယ်။
##
### လက်တွေ့ အနေနဲ့  table နှစ်ခုကို relation လုပ်ကြည့်ကြပါမယ်။
- အရင်ဆုံး table တွေ မလုပ်ခင် data modeling ကို စဥ်းစားကြည့်ရအောင်
- ခု ကျနော်တို့ မှာက 
   - users ဆိုတဲ့ table တစ်ခုမှာ user တွေ ရှိပါမယ်
   
|  id       |name|eamil  |
|------|---------|----------|
|1|aung|aung@gmail.com|
|2|koko|koko@gmail.com|

   - အဲ့ဒီ user တွေက ပုံတွေတင်မယ် ဆိုပါစို့
   - user တစ်ယောက်ကနေ တစ်ခုထပ်ပိုတဲ့ ပုံတွေ တင်တဲ့အခါ **table တစ်ခုထဲမှာ ပဲချိတ်ဆက်မယ်ဆို** ခုလို **data duplicate**  တွေ ဖြစ်လာမှာဖြစ်ပါတယ်

|  id   |name|eamil  |img_id  |img_url  |
|------|---------|----------|----------|----------|
|1|aung|aung@gmail.com|1|test url link1|
|1|aung|aung@gmail.com|2|test url link2|
|1|aung|aung@gmail.com|3|test url link3|
|1|aung|aung@gmail.com|4|test url link4|
|2|koko|koko@gmail.com|5|test url link5|
|2|koko|koko@gmail.com|6|test url link6|
|2|koko|koko@gmail.com|7|test url link7|
|2|koko|koko@gmail.com|8|test url link8|
|2|koko|koko@gmail.com|9|test url link9|

  - အဲ့ဒိလို မဖြစ်ရလေအောင် photos ဆိုတဲ့ table တစ်ခု ထပ်လုပ်လိုက်ပြီး သက်ဆိုင်ရာ user တွေရဲ့ id ( PK) ကို (FK) အနေနဲ့ ချိတ်ဆက်ပေးလိုက်ရင် ခုလို အဆင်ပြေပြေ နဲ့ ချိတ်လို့ရသွားမှာ ဖြစ်ပါတယ်
  - User to Photos ( **one**) **to**  ( **many** ) **relation**
  
  users 
|  id (PK)    |name|eamil  |
|------|---------|----------|
|1|aung|aung@gmail.com|
|2|koko|koko@gmail.com|

  photos
|  id (PK)    |img_url|user_id (FK)|
|------|---------|----------|
|1|test url link1|1|
|2|test url link2|1|
|3|test url link3|1|
|4|test url link4|1|
|5|test url link5|2|
|6|test url link6|2|
|7|test url link7|2|
|8|test url link8|2|
|9|test url link9|2|

- data modeling လုပ်ပြီး ဘယ်လို ချိတ်ရမလဲ သိပြီးဆိုတော့ table တွေ create လုပ်ပါမယ်။
```sql
//psql
postgres=# \c happy_pos2_db
You are now connected to database "happy_pos2_db" as user "postgres".
happy_pos2_db=# CREATE TABlE users (
happy_pos2_db(# id serial primary key,
happy_pos2_db(# name text not null,
happy_pos2_db(# email text not null);
```
- ဆက်ပြီး users table ထဲ rows ( data) တွေ ထည့်လိုက်ပါမယ်
```sql
happy_pos2_db=# INSERT INTO users (name,email) VALUES ('aung','aung@web.de'),('koko','koko@web.de');
INSERT 0 2
happy_pos2_db=#
```
- 
- ခု users table မှာ rows နှစ်ခု ရှိလာပြီး ဖြစ်ပါတယ်။
 ```sql
 happy_pos2_db=# SELECT * FROM users;
 id | name |    email
----+------+-------------
  1 | aung | aung@web.de
  2 | koko | koko@web.de
(2 rows)
```
- ဆက်ပြီး photo table ကို ဆက်လုပ်ပါမယ်
```sql
CREATE TABLE photos (
id serial primary key not null,
img_url text not null,
user_id integer REFERENCES users
);
```
- user_id ကို FK အနေနဲ့ ထည့်ထားပြီး users table ကို REFERENCES လုပ်ထားတာက user_id တွေက users table ထဲက PK ဖြစ်တဲ့ id နဲ့ ချိတ်ဆက်ထားကြောင်း လုပ်ထားတာဖြစ်ပါတယ်။
- ဆက်ပြီး data modeling လုပ်တုန်းကလို data တွေ ထည့်လိုက်ပါမယ်။
```sql
INSERT INTO photos ( img_url,user_id) 
VALUES('url 1',1),
('url2',1),
('url3',1),
('url4',1),
('url5',2),
('url6',2),
('url7',2),
('url8',2);
```
![enter image description here](https://raw.githubusercontent.com/Aungmyanmar32/msquare-m5-summaries/main/Screenshot%202023-05-02%20075412.png)
- အခုဆိုရင် users table နဲ့ photos table ကို one to many ချိတ်ဆက်လို့ ပြီးပါပြီး။

##
### ဆရာ နမူနာပြသွားတဲ့ table plus tool ကို သုံးချင်ရင် [ဒီမှာ ယူပါ။](https://download.pesk.top/eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJleHAiOjE2ODMwNzEyODMsInN1YiI6IlRhYmxlUGx1cyA1LjMuMyBbcGVza3RvcC5jb21dLnJhciIsInNvdXJjZSI6Imh0dHBzOlwvXC9wZXNrdG9wLmNvbVwvIn0.LiRwnEEij-whhEAi5rIpJuFt8DmL-NLHUxNDO9ZZUS4)
- conect လုပ်နည်း ကို အောက်မှာကြည့်ပါ

![enter image description here](https://cdn.discordapp.com/attachments/1074204233588543560/1102746375956996116/Screenshot_2023-05-02_062655.png)

![enter image description here](https://cdn.discordapp.com/attachments/1074204233588543560/1102746601925120010/Screenshot_2023-05-02_063030.png)

- CONNECT မလုပ်ခင်မှာ test အရင် လုပ်ကြည့်ပါ ။
- ok ပြီးဆို connect လုပ်ပြီး database ကို ချိတ်ဆက်ပါ
![enter image description here](https://cdn.discordapp.com/attachments/1074204233588543560/1102746981945839716/Screenshot_2023-05-02_063350.png)
