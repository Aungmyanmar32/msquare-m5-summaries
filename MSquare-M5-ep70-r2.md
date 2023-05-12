## MSquare Programing Fullstack Course
### Episode-*70* 
### Summary For `Room(2)`
### - change and save password as hash in database
### - create login feature
##
### အရင် သင်ခန်းစာမှာ register လုပ်လိုက်တဲ့ user infoကို databse မှာ သိမ်းခဲ့ကြပါတယ်
- သိမ်းလိုက်တဲ့ အချိန်မှာ password ကို plain text အနေနဲ့ပဲ သိမ်းလိုက်တာဟာ လုံခြုံ မှု အားနည်းပါတယ်။
- ဒါကြောင့် password တွေကို hash decrypt လုပ်ပြီးမှ သိမ်းလေ့ရှိကြပါတယ်။
-  password တွေကို hash decrypt လုပ်ဖို့ အတွက် bcrypt ဆိုတဲ့ node module တစ်ခုကို အသုံးပြုပါမယ်
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

##
### ဆက်ပြီးတော့ register လုပ်ပြီးသား user တစ်ယောက် က login ၀င်လို့ရအောင် လုပ်ပါမယ်။
- login ဆိုတဲ့ component တစ်ခုကို  လုပ်ပါမယ်
 `frontend/src/component/Login.tsx`
 ```js
//Login.tsx

import { Box, Button, TextField } from "@mui/material";
import { useState } from "react";
import { useNavigate } from "react-router-dom";

const Login = () => {
  const navigate = useNavigate();
  const [user, setUser] = useState({ email: "", password: "" });

  const login = async () => {
    const response = await fetch("http://localhost:5000/auth/login", {
      method: "POST",
      headers: {
        "Content-Type": "application/json",
      },
      body: JSON.stringify(user),
    });
    if (response.ok) {
      navigate("/");
    }
  };

  return (
    <Box sx={{ display: "flex", flexDirection: "column", mt: 5 }}>
      <TextField
        label="Email"
        variant="outlined"
        sx={{ minWidth: "300px" }}
        onChange={(evt) => setUser({ ...user, email: evt.target.value })}
      />
      <TextField
        label="Password"
        variant="outlined"
        sx={{ minWidth: "300px", my: 2 }}
        type="password"
        onChange={(evt) => setUser({ ...user, password: evt.target.value })}
      />
      <Button variant="contained" onClick={login}>
        Login
      </Button>
    </Box>
  );
};

export default Login;
```
### ရှင်းလင်းချက်
```
const navigate = useNavigate();
```
- react ရဲ့  useNavigate() hook ကို သုံးထားပါတယ်။
- navigate( "/route")  ဆိုပြီး ခေါ်လိုက်တာနဲ့ **(**   **)** ထဲမှာ ထည့်ပေးလိုက်တဲ့ route ကို အလိုအလျောက် သွားပေးမှာဖြစ်ပါတယ်

```
  const [user, setUser] = useState({ email: "", password: "" });
```
- user state တစ်ခု လုပ်ထားပြီး မူလတန်ဖိုး အနေနဲ့ email and password ပါတဲ့ object တစ်ခုအဖြစ် သတ်မှတ်ထားပါတယ်။
```js
const login = async () => {
    const response = await fetch("http://localhost:5000/auth/login", {
      method: "POST",
      headers: {
        "Content-Type": "application/json",
      },
      body: JSON.stringify(user),
    });
    if (response.ok) {
      navigate("/");
    }
  };
```
- login ဆိုတဲ့ function တစ်ခု သတ်မှတ်ထားပါတယ်။
- အထဲမှာတော့ server ဆီ post request လုပ်ထားပြီး user state value ကို body အနေနဲ့ ထည့်ပေးလိုက်ပါတယ်။
- response ပြန်လာတဲ့အခါ က  **ok**  ရဲ့  တန်ဖိုးက true ဖြစ်ခဲ့ရင် `"/"` home page ဆီ navigate လုပ်ပေးထားပါတယ်။

```js
<TextField
        label="Email"
        variant="outlined"
        sx={{ minWidth: "300px" }}
        onChange={(evt) => setUser({ ...user, email: evt.target.value })}
      />
      <TextField
        label="Password"
        variant="outlined"
        sx={{ minWidth: "300px", my: 2 }}
        type="password"
        onChange={(evt) => setUser({ ...user, password: evt.target.value })}
      />
```
- text input နှစ်ခုလုပ်ထားပြီး  user က တစ်ခုခု ရိုက်ထည့်လိုက်တာနဲ့  user state ထဲက value တွေကို update လုပ်ထားပါတယ်

```js
<Button variant="contained" onClick={login}> Login </Button>
```
- Login button တစ်ခု လုပ်ထားပြီး နှိပ်လိုက်တာနဲ့ login function ကို ခေါ်ပြီး server ဆီ request လုပ်မှာ ဖြစ်ပါတယ်။
- အခု Login Component ကနေ ၀င်လာမယ့် request ကို server မှာ လက်ခံကာ
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
- server မှာ အဆင်သင့်ဖြစ်ပါပြီး
##
### frontend က index.tsx မှာ route တွေ လုပ်ပါမယ်
```js
import React from "react";
import ReactDOM from "react-dom/client";
import "./index.css";
import App from "./App";
import { RouterProvider, createBrowserRouter } from "react-router-dom";
import Register from "./components/Register";
import Login from "./components/Login";

const routes = createBrowserRouter([
  {
    path: "/",
    element: <App />,
  },
  {
    path: "/register",
    element: <Register />,
  },
  {
    path: "/login",
    element: <Login />,
  },
]);

const root = ReactDOM.createRoot(
  document.getElementById("root") as HTMLElement
);
root.render(<RouterProvider router={routes} />);
```
- react router dom ကို သုံးပြီး 
    - `"/"`  route ထဲကို ရောက်ရင် App component 
    - - `"/register"`  route ထဲကို ရောက်ရင် Register component 
    - - `"/login"`  route ထဲကို ရောက်ရင် login component 
 - စတာတွေကို ပြထားတာဖြစ်ပါတယ်။
 - App component မှာ လည်း အနည်းငယ် ပြင်ရေးပါမယ်။
 ```js
 import React from "react";
import logo from "./logo.svg";
import "./App.css";
import NavBar from "./components/NavBar";
import Register from "./components/Register";
import { Box, Typography } from "@mui/material";

function App() {
  return (
    <div className="App">
      <NavBar />
      <Box sx={{ mt: 5 }}>
        <Typography variant="h3">Welcome to Foodie POS</Typography>
      </Box>
    </div>
  );
}
```
- ခု  foodie app မှာ ခုနက register လုပ်ထားတဲ့ user( name =**new1**, eamil =**new1@web.de**, password = **aungaung**) ကို login ပြန်၀င်ကြည့်ပါမယ်
- Foodie app ( browser) မှာ `/login` route ကို သွားပါမယ်
- eamil နဲ့ password ကို ရိုက်ထည့်ပြီး login နှိပ်လိုက်တာနဲ့ home page ဆီ ရောက်သွားတာကို မြင်ရမှာဖြစ်ပါတယ်။

![enter image description here](https://github.com/Aungmyanmar32/msquare-m5-summaries/blob/main/Screenshot%202023-05-12%20225629.png?raw=true)

![enter image description here](https://github.com/Aungmyanmar32/msquare-m5-summaries/blob/main/Screenshot%202023-05-12%20225638.png?raw=true)
