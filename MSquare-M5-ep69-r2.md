## MSquare Programing Fullstack Course
### Episode-*69* 
### Summary For `Room(2)`
## Connet to databse using pg-pool
- ပြီခဲ့တဲ့သင်ခန်းစာက စခဲ့တဲ့ project ကို databaseနဲ့ ချိတ်ဆက်မှာဖြစ်ပါတယ်
- အရင်ဆုံး postgres မှာ database အသစ်တစ်ခုလုပ်ပါမယ်
```sql
CREATE DATABASE foodie_pos_db
```
- အဲ့ဒီ database ထဲ ၀င်ပြီး
- users ဆိုတဲ့ table တစ်ခု create လုပ်ပါမယ်။

```sql
\c foodie_pos_db
```
```sql
CREATE TABLE users (
id serail PRIMARY KEY NOT NULL,
name TEXT NOT NULL,
email TEXT UNIQUE NOT NULL,
password TEXT NOT NULL
)
```
- email column မှာ UNIQUE လို့ type လုပ်ထားတာက တူတဲ့ email နှစ်ခု database မှာ ရှိလို့မရအောင် အထူးပြုထားတာဖြစ်ပါတယ်။
- ဆိုလိုတာက aungaung@gmail.com ဆိုပြီး database မှာ သိမ်းထားပြီးသားရှိမယ်ဆိုရင် နောက်ထပ်တစ်ခါ aungaung@gmail.com နဲ့ register ထပ်လုပ်လို့မရအောင် တားဆီးထားလိုက်တာဖြစ်ပါတယ်။
- express server ကနေ databse ( postgres) ကို ချိတ်ဆက်ဖို့ ***`pg`*** ဆိုတဲ့ node module ကို အသုံးပြုရပါမယ်။
```console
// backend(main)

npm i pg
npm i -D @types/pg
```
- backend folder အောက်မှာ src folder တစ်ခု  ဆောက်ပါမယ်။
- src folder ထဲမှာ db folder တစ်ခု ထပ်လုပ်ပါမယ်။
- db folder ထဲမှာမှ db.ts ဆိုတဲ့ ဖိုင်တစ်ခုလုပ်ပါမယ်။
```sql
project/backend/src/db/db.ts
```
```js
// db.ts
import { Pool } from "pg";

export const db = new Pool({
  host: "localhost",
  port: 5432,
  user: "postgres",
  password: "test",
  database: "foodie_pos_db",
  max: 20,
  idleTimeoutMillis: 30000,
  connectionTimeoutMillis: 2000,
});

```
- password နေရာမှာ မိမိ postgres ရဲ့ password ကို ထည်ပေးပါ။
- databaseနေရာမှာ ခုနက create လုပ်လိုက်တဲ့ database name  ကို ထည့်ပေးပါ။
- server.ts မှာ request နဲ့အတူပါလာတဲ့ data တွေကို database မှာ သိမ်းဖို့ လုပ်ပါမယ်။
```js
import cors from "cors";
import express from "express";
import { db } from "./src/db/db";
const app = express();
const port = 5006;
app.use(cors());
app.use(express.json());

app.post("/auth/register", async (req, res) => {
  const { name, email, password } = req.body;
  if (!name || !email || !password) return res.sendStatus(400);

  const text =
    "INSERT INTO users(name, email, password) VALUES($1, $2, $3) RETURNING *";
  const values = [name, email, password];

    const result = await db.query(text, values);
    
    const userInfo = result.rows[0];
   
    res.send(userInfo);
  
});

app.listen(port, () => console.log(`Example app listening on port ${port}!`));

```
> ရှင်းလင်းချက်

    if (!name || !email || !password) return res.sendStatus(400);

- တကယ်လို့ request နဲ့ ၀င်လာတဲ့ data တွေ မပြည့်စုံခဲ့ရင် Http status code 400 ( Bad request ) ကို response ပြန်ပေးလိုက်တာဖြစ်ပါတယ်။
```js
const text =
    "INSERT INTO users(name, email, password) VALUES($1, $2, $3) RETURNING *";
     const values = [name, email, password];

    const result = await db.query(text, values);
    res.send(result.rows[0])
```
- text ဆိုတဲ့ variable မှာ sql query ( code) တွေ ထည့်သိမ်းထားတာဖြစ်ပါတယ်
- **INSERT INTO users(name, email, password)** ဆိုတာက users table ထဲက  name, email, password ဆိုတဲ့ column တွေ မှာ ထည့်မှာဖြစ်ပါတယ်
- နောက်က values နေရာမှာ ထည့်မယ့် data တွေကို ရေးပေးရမှာဖြစ်ပါတယ်။
- name ရဲ့ တန်ဖိုးက $1  / eamil ရဲ့ တန်ဖိုးက$2 / password ရဲ့ တန်ဖိုးက$3 ဆိုပြီး ထည့်လိုက်တာဖြစ်ပါတယ်။
- $1 $2 $3 နေရာမှာ အောက်က ကြေငြာထားတဲ့ values array ထဲက တန်ဖိုးတွေ အစဥ်တိုင်း ၀င်လာမှာဖြစ်ပါတယ်
```js
 const values = [name, email, password];
 // values = [$1:name,$2:email,$3:password]
 ```
 - တစ်ခါတည်း မရေးထည့်ပဲ $1 $2 $3 ဆိုပြီး value array ထဲကနေ တစ်ဆင့်ပြန်ခေါ်သုံးတာကို parameterize query လို့ ခေါ်ပါတယ်။
 - မသမာသူများက မလိုလားအပ်တဲ့ sql query တွေကို request ကနေ တစ်ဆင့် ပေးပို့လာမယ့် problem ကနေ safe ဖြစ်အောင်လို့ အခုလို အသုံးပြုပေးရပါတယ်။

`const result = await db.query(text, values)`;
- db.ts ထဲက export လုပ်ထားတဲ့ db ကို အသုံးပြုထားပါတယ်။
- db.query method ကို သုံးပြီး parameter အနေနဲ့ text နဲ့ values တွေကို ထည့်ပေးလိုက်ပါတယ်။
- အထက်မှာ ရှင်းပြခဲ့သလိုပဲ database ထဲက users table မှာ သွားသိမ်းပေးမှာဖြစ်ပါတယ်
- db.query က promise ကို return ပြန်တာမလို့ async await ကို အသုံးပြုပေးရမှာဖြစ်ပါတယ်။

- sql query မှာ `RETURNING *` ဆိုပြီး လုပ်ထားမလို့  result ထဲက index 0 ဖြစ်တဲ့ rows တစ်ခုကိုပဲ response ပြန်လိုက်တာဖြစ်ပါတယ်
- ခု app မှာ user info တွေ ဖြည့်ပြီး register လုပ်ကြည့်ပါက ထည့်လိုက်တဲ့ အချက်အလက်အတိုင်း log ထုတ်ပေးတာကို မြင်ရမှာ ဖြစ်ပါတယ်။
- တစ်ခု ရှိတာက email တူတဲ့ user တွေကို Register လုပ်ကြည့်ပါက error တက်ပြီး backend server ပါ သေသွားတာတဲ့ error ကို ကြုံရမှာဖြစ်ပါတယ်
- အဆိုပါ error ကို try catch နဲ့ ဖြေရှင်းပါမယ်
```js
app.post("/auth/register", async (req, res) => {
  const { name, email, password } = req.body;
  if (!name || !email || !password) return res.sendStatus(400);

  const text =
    "INSERT INTO users(name, email, password) VALUES($1, $2, $3) RETURNING *";
  const values = [name, email, password];

  try {
    const result = await db.query(text, values);
    const userInfo = result.rows[0];
    delete userInfo.password;
    res.send(userInfo);
  } catch (error) {
    console.log(error);
    res.sendStatus(500);
  }
});
```
- error ဖြစ်ခဲ့ရင် status 500 ကို server error အနေနဲ့ ပို့ပေးလိုက်တာဖြစ်ပါတယ်။
- အခု ဆိုရင် email တူနေတဲ့ users တွေ ထည့်စမ်းကြည့်ရင်တောင် ဆာဗာက မသေတော့ပဲ error ကို ပြပေးမှာဖြစ်ပါတယ်။
