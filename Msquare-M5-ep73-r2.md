## MSquare Programing Fullstack Course
### Episode-*73* 
### Summary For `Room(2)` 
### Create and Add checkAuth middleware
- ပြီးခဲ့တဲ့ သင်ခန်းစာမှာ server ကနေ accessToken တစ်ခု လုပ်ပြီး log in ၀င်လိုက်ချိန်မှာ response ပြန်ပေးတာကို localstroage မှာ သိမ်းခဲ့ကြပါတယ်။
- ခု သင်ခန်းစာမှာတော့ accessToken ပါလာမှသာ server ဆီမှာ data တွေ request လုပ်လို့ရအောင် ဆက်လုပ်သွားပါမယ်။
- အရင်ဆုံး database ထဲမှာ menus table တစ်ခုလုပ်ပါမယ်။

```sql
CREATE TABLE menus (
 id serial PRIMARY KEY NOT NULL,
 name text NOT NULL ,
 price INTEGER NOT NULL,
 asset_url text ,
 description text
);
```
- name = Mont Hin Khar , price = 2000 ဆိုတဲ့  menu တစ်ခု ထည့်ထားလိုက်ပါမယ်
```sql
INSERT into menus (name,price) 
VALUES ('Mont Him Khar', 2000)
```
- backend ထဲက server.ts မှာ **`/menus`**  route ကနေ ၀င်လာမယ့် request ကို အရင် လက်ခံထားလိုက်ပါမယ်

```js
// server.ts --( ---)

app.get("/menus", async (req, res) => {
  const result = await db.query("SELECT * FROM menus ");
  res.send(result.rows);
});

app.post("/auth/register", async (req, res) => {.....

app.post("/auth/login", async (req, res) => {....
         
app.listen(port, () => console.log(`Example app listening on port ${port}!`));

```
- **/menus**  route ( end point) နဲ့ request ၀င်လာရင် database က menus table မှာရှိတဲ့ data တွေကို လှမ်းယူးပြီး response ပြန် လိုက်တာဖြစ်ပါတယ်
- **/menus**  route ကို frontend ရှိ App.tsx ကနေ request တစ်ခု send လိုက်ပါမယ်။
```js
//App.tsx

import { Box, Typography } from "@mui/material";
import "./App.css";
import NavBar from "./components/NavBar";
import Register from "./components/Register";
import { useEffect } from "react";

function App() {
  const accessToken = localStorage.getItem("accessToken"); // getItem(key) = value = qwerewqrewqrewq.cxcbfhfdssfg
  console.log("from App component", accessToken);

  useEffect(() => {
    fetchData();
  }, []);

  const fetchData = async () => {
    const response = await fetch("http://localhost:5000/menus", {
      headers: {
        Authorization: `Bearer ${accessToken}`,
      },
    });
    console.log(await response.json());
  };

  return (
    <>
      <NavBar />
      <Box sx={{ mt: 5 }}>
        <Typography variant="h3">Welcome to Foodie POS</Typography>
      </Box>
    </>
  );
}

export default App;

```
### ရှင်းလင်းချက်

```
const fetchData = async () => {
    const response = await fetch("http://localhost:5000/menus", {
      headers: {
        Authorization: `Bearer ${accessToken}`,
      },
    });
    console.log(await response.json());
  };
```
- **/menus** route ဆီ get method နဲ့ request ပို့လိုက်ပြီး header အနေနဲ့  **Authorization: `Bearer ${accessToken}`** ဆိုပြီး localstroage မှာ သိမ်းထားတဲ့ accessToken ကို Bearer နဲ့ ပေါင်းပြီး Authorization အနေနဲ့ပို့လိုက်တာဖြစ်ပါတယ် 
- ( **request.body = {Authorization: "Bearer weqrewre.43asdjOdanda" }** ) အဲ့ဒီလိုပုံစံမျိုးနဲ့ ပို့လိုက်တာမျိုးပါ။

##

- backend  မှာ src folder အောက် utils folder တစ်ခု ထပ်လုပ်ပြီး auth.ts ဖိုင်တစ်ခုလုပ်ပါမယ်။
```js
// auth.ts

import { NextFunction, Request, Response } from "express";
import jwt from "jsonwebtoken";
import { config } from "../config/config";

export const checkAuth = (req: Request, res: Response, next: NextFunction) => {
  const headers = req.headers;
  const authorization = headers.authorization;
  if (!authorization) return res.send(401);
  try {
    const token = authorization.split(" ")[1];
    jwt.verify(token, config.jwtSecret);
    next();
  } catch (err) {
    res.sendStatus(401);
  }
};
```
- auth.ts မှာ checkAuth  ဆိုတဲ့ Middleware ( function) တစ်ခု သတ်မှတ်ထားပြီး export လုပ်ထားလိုက်ပါတယ်။
```
 const headers = req.headers;
  const authorization = headers.authorization;
  if (!authorization) return res.send(401);
```
- request နဲ့ ပါလာတဲ့ Header ထဲက authorization ကို လှမ်းယူလိုက်ပြီး authorization က undefined ( တန်ဖိုး မပါလာခဲ့ရင်) 401 ကို response ပြန်လိုက်တာဖြစ်ပါတယ်
```
try {
    const token = authorization.split(" ")[1];
    jwt.verify(token, config.jwtSecret);
    next();
  }
```
- authorization value ပါခဲ့ရင် access token ကို လှမ်းယူလိုက်ပြီး မှန် မမှန် verify လုပ်လိုက်ပါတယ်
- မှန်ခဲ့ရင် သူ့အနောက်မှာ ရှိနေတဲ့ middleware( function) ကို ဆက် run ခိုင်းလိုက်တာဖြစ်ပါတယ်။
- ခု ဆက်ပြီး **/menus** route မှာ request လက်ခံတဲ့အခါ access token ပါမပါ နဲ့ မှန် မမှန် စစ်ပြီးမှ response ပြန်လုပ်အောင် လုပ်ပါမယ်။

![enter image description here](https://github.com/Aungmyanmar32/msquare-m5-summaries/blob/main/Screenshot%202023-05-18%20103518.png?raw=true)
- **/menus** route နဲ့ response ပြန်တဲ့ function ကြားမှာ ခနက သတ်မှတ်ထားတဲ့ checkAuth middleware ကို ခေါ်ထားလိုက်တာဖြစ်ပြီး 
- checkAuth middleware မှာ ရေးထားတဲ့အတိုင်း access token ပါမပါ နဲ့ မှန် မမှန် စစ်ပြီးမှ next function ဖြစ်တဲ့ response ပြန်တဲ့ function ကို ဆက်ခေါ်ပေးသွားမှာဖြစ်ပါတယ်
- ဆိုလိုတာက checkAuth middleware မှာ စစ်ထားတာတွေ မှန်မှ **/menus** route ဆီ ၀င်လာတဲ့ request ကို response ပြန်ပေးမှာ ဖြစ်ပြီး error တစ်ခုခု ဖြစ်ခဲ့ရင်တော့ response ပြန်တဲ့ function ကို မခေါ်ပေးတော့ပဲ 401 ပဲ response ပြန်ပေးလိုက်မှာဖြစ်ပါတယ်။


##
### Data Modeling for database

- ဆက်ပြီးတော့ database မှာ table တွေ ချိတ်ဆက်ဖို့ လုပ်ပါမယ်။
- လက်ရှိ database မှာ menus table တစ်ခုရှိနေပါတယ်
- အဲ့ဒီ menu item တွေကို menu categories တွေ နဲ့ ချိတ်ဆက်ဖို့ လုပ်ကြည့်ပါမယ်
### example
- Most Popular , Hot Dish , Cold Dish ဆိုတဲ့ menu category သုံးခုရှိတယ် ဆိုပါစို့
- menu table ထဲက Mont Him Khar က Most Popular ရော Hot Dish ကောဖြစ်တဲ့  category  နှစ်ခုနဲ့ ချိတ်ချင်တဲ့ အခါ ( one to many relation ) ဖြစ်မှာမို့လို့ FK ကို တိုက်ရိုက်ထည့်ချိတ်ရင် data duplicate ဖြစ်လာမှာ မလို့ ကြားခံ join table တစ်ခုနဲ့ ချိတ်ဆက်ပေးရမှာဖြစ်ပါတယ်
```mermaid
graph LR
A[1.Most Popular] --> D(Join table)
B[2.Hot Dish] --> D(Join table)
D <---> E(1.Mont Him Khar)
C[3.Cold Dish] 


```

![enter image description here](https://github.com/Aungmyanmar32/msquare-m5-summaries/blob/main/mnusjoing.png?raw=true)

- အထက်ပါ အတိုင်း database မှာ table တွေ ဆောက်ကြည့်ပါမယ်။

```sql
CREATE TABLE menu_categories (
id serial PRIMARY KEY NOT NULL,
name TEXT NOT NULL
)
```
```sql
INSERT INTO menu_categories (name)
VALUES ('Most Popular'),('Hot Dish'),('Cold Dish')
```
- menu_categories table တစ်ခု လုပ်လိုက်ပြီး ('Most Popular'),('Hot Dish'),('Cold Dish') ဆိုပြီး data rows တွေ ထည့်ထားလိုက်ပါတယ်
```sql
CREATE TABLE menus_menu_categories (
id serial PRIMARY KEY NOT NULL,
menus_id INTEGER REFERENCES menus,
menu_categories_id INTEGER REFERENCES menu_categories
)
```
- menus_menu_categories ဆိုတဲ့ join table တစ်ခု လုပ်ထားပြီး
- menus_id နဲ့ menu_categories_id တွေကို FK အနေနဲ့ join table ထဲမှာ relation လုပ်ထားတာဖြစ်ပါတယ်
```sql
INSERT INTO menus_menu_categories ( menus_id,menu_categories_id)
VALUES ( 1,1),(1,2)
```
- menu_id ( 1) ကို menu_categories_id (1) နဲ့ ကော (2) နဲ့ ပါ relation လုပ်ပေးထားတာဖြစ်ပါတယ်
- အဲ့တာကို အဆင်ပြေမပြေ test ကြည့်ပါမယ်

```sql
SELECT m.name,mc.name from menus as m
INNER JOIN menus_menu_categories as mmc 
on mmc.menus_id = m.id
INNER JOIN menu_categories as mc 
on mc.id = mmc.menu_categories_id
```
- အခုလိုလေး ပြပေးတယ်ဆို အဆင်ပြေပါပြီး

![enter image description here](https://github.com/Aungmyanmar32/msquare-m5-summaries/blob/main/Screenshot%202023-05-18%20114516.png?raw=true)

##
### try this
- **/menu_categories** ဆိုတဲ့ route ကို request လုပ်ပြီး database က menu categories တွေ front end မှာ console log ထုတ်ကြည့်ပါ။
