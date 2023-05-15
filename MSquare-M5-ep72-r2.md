## MSquare Programing Fullstack Course
### Episode-*72* 
### Summary For `Room(2)` 
## Authorization 
- ပြီးခဲ့တဲ့ သင်ခန်းစာမှာpassword ကို hash လုပ်ပြီး သိမ်းခဲ့ကြပါပြီး
-  ခု သင်ခန်းစာမှာတော့ login information တွေ မှန်တဲ့ user ကို token ( key) တစ်ခုထုတ်ပေးလိုက်ပြီး local storage မှာ သိမ်းပေးလိုက်ပါမယ်
- အဲ့ဒီ token ရှိမှ server ဆီက data တွေ fetch လို့ရအောင် နောက်သင်ခန်းစာတွေမှာ ဆက်လုပ်သွားပါမယ်
- ဒီနေ့ သင်ခန်းစာမှာ တော့
1. jsonwebtoken, @types/jsonwebtoken, dotenv
2. jwt secret --> .env, import dotenv in server.ts --> dotenv.config() on top
3. process.env.JWT_SECRET
4. config --> config.jwtSecret
5. auth/login --> bcrypt compare --> isCorrectPassword --> delete user.password
6. isCorrectPassword ? jwt.sign(user, config.jwtSecret) --> token
7. res.send({accessToken})
8. frontend --> localStorage.setItem("accessToken", accessToken)
- စတာတွေ လေ့လာသွားပါမယ်
- 
 ##  Using *JWT* ( *J*son *W*eb *T*oken )
 ### syntax 
 ### create token
 **`jwt.sign( object , secret-key )`**
- user log in လုပ်လိုက်တဲ့အချိန်မှာ JWT access token တစ်ခုလုပ်ပြီး response ပြန်ပေးလိုက်မှာဖြစ်ပါတယ်။
```console
// npm install

npm i jsonwebtoken dotenv
npm i -D @types/jsonwebtoken 
```
- server.ts မှာ import လုပ်ပါမယ်

```js
import dotenv from "dotenv";
dotenv.config();

import jwt from "jsonwebtoken";
import { config } from "./src/config/config";
```
-  backend မှာ .env နဲ့ src folder တစ်ခုလုပ်ပြီး config.ts file တစ်ခုလုပ်ပါမယ်။

```js
// backend/.env

JWT_SECRET=T7VkV0vJVeoG4xJAAJh0gntb8f6UPhTlhH3iDOkYuUv4mUey0xxN7mChSyxWCi4YZKWxzYpRfaHZyTNw2Jjfj9SGHG7DmVnaj7zT
```
- .env ဖိုင်ထဲမှာ JWT_SECRET တစ်ခုသတ်မှတ်လိုက်တာဖြစ်ပါတယ်။
```js
//backend/src/config.ts

interface Config {
  jwtSecret: string;
}

export const config: Config = {
  jwtSecret: process.env.JWT_SECRET || "",
};

```
- .env ဖိုင်ထဲမှာ သတ်မှတ်ထားတဲ့ JWT_SECRET ကို config object ထဲမှာ jwtSecret ဆိုပြီး သိမ်းထားပါတယ်
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
  delete user.password;
 
  const isCorrectPassword = await bcrypt.compare(password, hashedPassword);
 
  if (isCorrectPassword) {
    const accessToken = jwt.sign(user, config.jwtSecret);
    return res.send({ accessToken });
  }
  return res.sendStatus(401);
});
```
- ရှင်းလင်းချက်
```
if (isCorrectPassword) {
    const accessToken = jwt.sign(user, config.jwtSecret);
    return res.send({ accessToken });
  }
  return res.sendStatus(401);
```
- password မှန်ခဲ့ရင် `jwt.sign(user, config.jwtSecret)` ဆိုပြီး **user object နဲ့  config ထဲက secret key** ကို ပေါင်းကာ **accessToken** တစ်ခုလုပ်ပြီး response ပြန်ပေးလိုက်တာဖြစ်ပါတယ်
##
- server ကနေပြန်လာမယ့် **accessToken** ကို Login component က Login function မှာ လက်ခံပြီး localStroage မှာ သိမ်းလိုက်ပါမယ်။

```js
const login = async () => {
    const response = await fetch("http://localhost:5006/auth/login", {
      method: "POST",
      headers: {
        "Content-Type": "application/json",
      },
      body: JSON.stringify(user),
    });
    if (response.ok) {
      const responseData = await response.json();
      const accessToken = responseData.accessToken;
      localStorage.setItem("accessToken", accessToken);
      navigate("/");
    }
  };
```
- database  မှာ register လုပ်ထားတဲ့ user email  နဲ့  password ကို မှန်အောင်ထည့်ပြီး login ၀င်ကြည့်ပါက browser application အောက်က local Stroage မှာ အခုလို ပေါ်လာမှာဖြစ်ပါတယ်

![enter image description here](https://github.com/Aungmyanmar32/msquare-m5-summaries/blob/main/Screenshot%202023-05-15%20211825.png?raw=true)
- ပြီးရင် App component မှာ **`localStorage.getItem`** ကို သုံးပြီး accessToken ကို log ထုတ်ကြည့်ပါမယ်။

```js
//App.tsx
import { Box, Typography } from "@mui/material";
import "./App.css";
import NavBar from "./components/NavBar";
import Register from "./components/Register";

function App() {
  const accessToken = localStorage.getItem("accessToken");
  console.log("App component", accessToken);
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

```
 const accessToken = localStorage.getItem("accessToken");
  console.log("App component", accessToken);
```
- localStorage.getItem ကို သုံးပြီး accessToken တန်ဖိုးကို လှမ်းယူကာ log ထုတ်ထားတာပဲဖြစ်ပါတယ်

![enter image description here](https://github.com/Aungmyanmar32/msquare-m5-summaries/blob/main/Screenshot%202023-05-15%20211304.png?raw=true)
