## MSquare Programing Fullstack Course
### Episode-*65* 
### Summary For `Room(1)` intermediate Class
## Authorization 
### အရင် သင်ခန်းစာမှာ register လုပ်လိုက်တဲ့ user infoကို databse မှာ သိမ်းခဲ့ကြပါတယ်
- သိမ်းလိုက်တဲ့ အချိန်မှာ password ကို plain text အနေနဲ့ပဲ သိမ်းလိုက်တာဟာ လုံခြုံ မှု အားနည်းပါတယ်။
- ဒါကြောင့် password တွေကို hash decrypt လုပ်ပြီးမှ သိမ်းလေ့ရှိကြပါတယ်။
-  password တွေကို hash decrypt လုပ်ဖို့ အတွက် ***bcrypt*** ဆိုတဲ့ node module တစ်ခုကို အသုံးပြုပါမယ်
```js
npm i bcrypt
npm i -D @types/bcrypt
```
- server.ts မှာ request နဲ့အတူပါလာတဲ့ data ထဲက password ကို hash လုပ်ပါမယ်။

![enter image description here](https://github.com/Aungmyanmar32/msquare-m5-summaries/blob/main/Screenshot%202023-05-12%20212327.png?raw=true)
- line 14 မှာ bcrypt.hash() ကို သုံးပြီး password ကို hashPassword အဖြစ်ပြောင်းလိုက်ပါတယ်
- database မှာ သိမ်းတဲ့ အခါ password ရိုးရိုး ကို မသိမ်းတော့ပဲ hashedPassword ကိုပဲ သိမ်းလိုက်တာဖြစ်ပါတယ်။
- ခု user ( name =**new1**, eamil =**new1@web.de**, password = **aungaung**) အသစ်တစ်ယောက်ကို register လုပ်ကြည့်လိုက်ပါက database က users table ထဲမှာ password ကို hash code အနေနဲ့ သိမ်းပေးထားတာကို မြင်ရမှာ ဖြစ်ပါတယ်။

![enter image description here](https://github.com/Aungmyanmar32/msquare-m5-summaries/blob/main/Screenshot%202023-05-12%20213303.png?raw=true)

- အခု Login Component ကနေ ၀င်လာမယ့် request ကို server မှာ လက်ခံထားတဲ့နေရာမှာ
- email နဲ့ password မှန် မမှန် စစ်ပြီး response ပြန်ပါမယ်။

![enter image description here](https://github.com/Aungmyanmar32/msquare-m5-summaries/blob/main/Screenshot%202023-05-12%20221453.png?raw=true)
```js
const { email, password } = req.body;
  if (!email || !password) return res.sendStatus(400);
```
- request body ထဲက email နဲ့ password ကို ရွေးယူလိုက်ပြီး တစ်ခုခု မရှိခဲ့ရင် bad request ကို response ပြန်ပြီး အောက်ကကုဒ်တွေ ဆက်မrun တော့ပဲ return လုပ်လိုက်ပါတယ်။
```js
const userResult = await db.query("select * from users where email = $1", [email] );

if (!userResult.rows.length) return res.sendStatus(401);
  ```
  - database ထဲက users table မှာ သိမ်းထားတဲ့ email တွေနဲ့ request က ၀င်လာတဲ့ email တူ မတူ တိုက်စစ်လိုက်ပြီး တူတာမရှိဘူးဆိုရင်တော့ 401 unauthorize ကိုပဲ response လုပ်လိုက်ပြီး အောက်ကကုဒ်တွေ ဆက်မrun တော့ပဲ return လုပ်လိုက်ပါတယ်။

```js
const user = userResult.rows[0];
  const hashedPassword = user.password;
  const isCorrectPassword = await bcrypt.compare(password, hashedPassword);
```
  - email တူတဲ့ user ရှိခဲ့ရင်တော့ အဲ့ဒီ user ရဲ့  hashedpassword နဲ့ request ၀င်လာတဲ့ password တူမတူ  bcrypt.compare(_) method ကိုသုံးပြီး စစ်ပါတယ်။
  -  bcrypt.compare(_) method က စစ်တဲ့နှစ်ခု တူခဲ့ရင် true ထုတ်ပေးပြီး မတူဘူးဆိုရင် false ထုတ်ပေးမှာဖြစ်ပါတယ်။
  
  ```
  return isCorrectPassword ? res.sendStatus(200) : res.sendStatus(401);
```
- နောက်ဆုံးအနေနဲ့ password (true)တူခဲ့ရင်   200 ok ကို response ပြန်ပေးမှာဖြစ်ပြီး (false)မတူဘူးဆိုရင်တော့ 401 unauthorize ကို  ပြန်ပေးမှာဖြစ်ပါတယ်။
- Login Component က SignIn function မှာလည်း အနည်းငယ်ပြင်ရေးပါမယ်။

```js
// Login.tsx

const SignIn = async () => {
    const isValid = user.email.length > 0 && user.password.length > 0;
    if (!isValid) return setOpen(true);
    try {
      const response = await fetch(`${config.apiBaseUrl}/auth/login`, {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify(user),
      });
      const data = await response.json();

      if (response.ok) {
        navigate("/settings");
      }
    } catch (err) {
      setOpen(true);
      console.log("Error here: ", err);
    }
  };
```
##
##  Using *JWT* ( *J*son *W*eb *T*oken )

- user log in လုပ်လိုက်တဲ့အချိန်မှာ JWT access token တစ်ခုလုပ်ပြီး ကျန် တဲ့ request တွေ လက်ခံတဲ့ route တွေ မှာ access token ရှိမှ request လုပ်လို့ရအောင် လုပ်မှာဖြစ်ပါတယ်။
```console
// npm install

npm i jsonwebtoken dotenv
npm i -D @types/jsonwebtoken 
```
- server.ts မှာ import လုပ်ပါမယ်

```js
import dotenv from "dotenv";
dotenv.config;

import jwt from "jsonwebtoken";
```
-  backend မှာ .env နဲ့ src folder တစ်ခုလုပ်ပြီး config.ts file တစ်ခုလုပ်ပါမယ်။

```js
// backend/.env

JWT_SECRET=T7VkV0vJVeoG4xJAAJh0gntb8f6UPhTlhH3iDOkYuUv4mUey0xxN7mChSyxWCi4YZKWxzYpRfaHZyTNw2Jjfj9SGHG7DmVnaj7zT
```
```js
//backend/src/config.ts

interface Config {
  jwtSecret: string;
}

export const config: Config = {
  jwtSecret: process.env.JWT_SECRET || "",
};

```
- ပြီးရင် /auth/login route မှာ post request လက်ခံတဲ့ code  ကို အနည်းငယ်ပြင်ပါမယ်

```js


app.post("/auth/login", async (req, res) => {
  const { email, password } = req.body;
  console.log(req.body);

  if (!email || !password) return res.sendStatus(400);

  const userResult = await db.query("select * from users where email = $1", [
    email,
  ]);

  if (!userResult.rows.length) return res.sendStatus(401);

  const user = userResult.rows[0];
  const hashedPassword = user.password;

  const isCorrectPassword = await bcrypt.compare(password, hashedPassword);

  if (!isCorrectPassword)
    return res.sendStatus(401).send("Invalid Crendentials");

  const userToken = { id: user.id, name: user.name, email: user.email };
  const accessToken = jwt.sign(userToken, config.jwtSecret);

  res.send({ accessToken });
});
```
- user ရဲ့ id name email တွေကို userToken နဲ့ သိမ်းထားပြီး config က jwtSecret နဲ့ပေါင်းကာ accessToken တစ်ခုလုပ်ပြီး response ပြန်လိုက်တာဖြစ်ပါတယ်။
- ခု database မှာ ရှိပြီးသား user တစ်ယောက်ကို login ၀င်ကြည့်ပါက network response မှာ accessToken ကို မြင်ရမှာဖြစ်ပါတယ်။
![enter image description here](https://github.com/Aungmyanmar32/msquare-m5-summaries/blob/main/Screenshot%202023-05-15%20065719.png?raw=true)
- ရလာတဲ့ ACCESS TOKEN  ကို အသုံးပြုပြီး route တွေကို protect လုပ်ပါမယ်။
- backend မှာ auth middleware တစ်ခုလုပ်ပါမယ်။
```
backend / src / auth /auth.ts
```
```js
//auth.ts

import { NextFunction, Request, Response } from "express";
import jwt from "jsonwebtoken";
import { config } from "../config";

export const checkAuth = (req: Request, res: Response, next: NextFunction) => {
  const authorization = req.headers["authorization"];

  const accessToken = authorization && authorization.split(" ")[1];

  if (!accessToken) return res.sendStatus(401);

  try {
    jwt.verify(accessToken, config.jwtSecret);
    console.log("access token ok..");
    next();
  } catch (err) {
    console.log(err);

    res.sendStatus(401);
  }
};


```
- request နဲ့ ပါလာတဲ့ header ထဲမှာ authorization ပါမပါ စစ်လိုက်ပြီး ပါလာခဲ့ရင်
jwt.verify ကို သုံးပြီး စစ်လိုက်ပါမယ်။
- အဆင်ပြေခဲ့ရင် next function ကိုခေါ်ပေးလိုက်မှာဖြစ်ပါတယ်။
- next function ဆိုတာက checkAuth  function ကို run ပြီး အဆင်ပြေခဲ့ရင် သူ့အနောက်မှာရှိတဲ့ callback function ကို ဆက်ခေါ်ပေးသွားမှာဖြစ်ပါတယ်။
- အခု အဲ့ဒီ auth middleware ကို sever မှာ ရှိတဲ့ data တွေ request လုပ်တဲ့ middleware ( post, get , etc..)တွေမှာ ခေါ်သုံးလိုက်ပါမယ်

![enter image description here](https://github.com/Aungmyanmar32/msquare-m5-summaries/blob/main/Screenshot%202023-05-15%20100549.png?raw=true)

- ပုံထဲကလို request တွေ ၀င်လာပြီး response မလုပ်ခင်လေးမှာ 
- checkAuth  function ကို run ပြီး အဆင်ပြေခဲ့ရင် သူ့အနောက်မှာရှိတဲ့ async function ကို ဆက်ခေါ်ပေးသွားမှာဖြစ်ပါတယ်။
- ဆက်ပြီး login component ကနေ login ၀င်လိုက်တဲ့အခါ ရလာမယ့် accessToken ကို context ထဲမှာ သိမ်းပြီး request လုပ်တဲ့အခါ  header အနေနဲ့ပြန်သုံးပေးရမှာ ဖြစ်ပါတယ်။
``` js
//AppContext.tsx

import { createContext, useEffect, useState } from "react";
import {
  Addon,
  AddonCategories,
  Location,
  Menu,
  MenuCategories,
  MenusLocation,
} from "../types/types";
import { config } from "../configs/config";

interface AppContextTypes {
  menus: Menu[];
  menuCategories: MenuCategories[];
  addons: Addon[];
  addonCategories: AddonCategories[];
  locations: Location[];
  menusLocations: MenusLocation[];
  accessToken: string;
  updateData: (value: any) => void;
  fetchData: () => void;
}
const defaultAppContext = {
  menus: [],
  menuCategories: [],
  addons: [],
  addonCategories: [],
  locations: [],
  menusLocations: [],
  accessToken: "",
  updateData: (value: any) => {},
  fetchData: () => {},
};

export const AppContext = createContext<AppContextTypes>(defaultAppContext);

const AppProvider = (props: any) => {
  const [data, updateData] = useState(defaultAppContext);

  useEffect(() => {
    if (data.accessToken) {
      fetchData();
    }
  }, [data.accessToken]);

  const fetchData = async () => {
    const response = await fetch(`${config.apiBaseUrl}/data`, {
      headers: {
        Authorization: `Bearer ${data.accessToken}`,
      },
    });
    const responseJson = await response.json();
    console.log(responseJson);

    const {
      menus,
      menuCategories,
      addons,
      addonCategories,
      locations,
      menusLocations,
    } = responseJson;

    updateData({
      ...data,
      menus,
      menuCategories,
      addons,
      addonCategories,
      locations,
      menusLocations,
    });
  };

  return (
    <AppContext.Provider value={{ ...data, updateData, fetchData }}>
      {props.children}
    </AppContext.Provider>
  );
};
export default AppProvider;

```
- access token ကို context ထဲမှာ သိမ်းလိုက်ပြီး server ဆီ request လုပ်တဲ့အခါ headers အနေနဲ့ ထည့်ပေးလိုက်တာဖြစ်ပါတယ်။
- Login component က SingIn function မှာလည်း access token ကို လက်ခံပြီး context မှာ update လုပ်ပေးလိုက်မှာဖြစ်ပါတယ်
``` js
// Login.tsx --> 
 const contextData = useContext(AppContext);
  const { updateData, ...data } = contextData;
 

  const SignIn = async () => {
    const isValid = user.email.length > 0 && user.password.length > 0;
    if (!isValid) return setOpen(true);
    try {
      const response = await fetch(`${config.apiBaseUrl}/auth/login`, {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify(user),
      });
      const data = await response.json();

      if (response.ok) {
        updateData({ ...contextData, accessToken: data.accessToken });
        navigate("/settings");
      }
    } catch (err) {
      setOpen(true);
      console.log("Error here: ", err);
    }
  };
......
......
```
