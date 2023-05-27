## MSquare Programing Fullstack Course
### Episode-*74* 
### Summary For `Room(2)` 
### log in & log out 
- ဒီနေ့သင်ခန်းစာမှာတော့ user တစ်ယောက် log in / log out လုပ်လို့ရအောင် လေ့လာသွားကြပါမယ်
- အရင်ဆုံး navbar component မှာ log in ခလုတ်လေးတစ်ခု ထည့်လိုက်ပါမယ်

![enter image description here](https://cdn.discordapp.com/attachments/1076154921562411008/1111360998067159131/image.png)
```js
           <Typography
              variant="h6"
              component="div"
              sx={{ cursor: "pointer", userSelect: "none" }}
              onClick={() => {
                navigate("/login");
              }}
            >
              Log in
            </Typography>
```
- login ဆိုတဲ့ div  တစ်ခု လုပ်ထားပြီး နှိပ်လိုက်တာနဲ့ login route ကို ပို့လိုက်ပြီး login component ကို ပြထားတာဖြစ်ပါတယ်
- အခု app မှာ သွားကြည့်လိုက်ရင် App Bar ညာဘက်ေထာင့်မှာ Log in ဆိုပြီး ပြပေးနေမှာဖြစ်ပါတယ်
- ဆက်ပြီး user က login ၀င်ထားပြီးပြီဆိုရင် အဲ့ဒီနေရာမှာ log out ကို အစားထိုးပြပေးပြီး log out ( log in မ၀င်ထားရင်တော့ ) log in ကို ပြပေးမှာဖြစ်ပါတယ်
![enter image description here](https://cdn.discordapp.com/attachments/1076154921562411008/1111356144804450364/image.png)
```js
 {accessToken ? (
            <Typography
              variant="h6"
              component="div"
              sx={{ cursor: "pointer", userSelect: "none" }}
              onClick={() => {
                localStorage.removeItem("accessToken");
                navigate("/logout");
              }}
            >
              Log out
            </Typography>
          ) : (
            <Typography
              variant="h6"
              component="div"
              sx={{ cursor: "pointer", userSelect: "none" }}
              onClick={() => {
                navigate("/login");
              }}
            >
              {window.location.pathname === "/login" ? "" : "Login"}
            </Typography>
          )}
```
- accessToken ရှိမရှိ စစ်လိုက်ပြီး
- မရှိခဲ့ရင် login လို့ပြပြီး log in လုပ်ခိုင်းမှာဖြစ်ပြီး
-  ရှိခဲရင် log out လို့ပြပေးမှာဖြစ်ပါတယ်
- Log out ကို နှိပ်လိုက်တဲ့အခါ local storage မှာ သိမ်းထားတဲ့ access token ကို  ဖျက်လိုက်ပြီး logout component ကို ပြပေးနိုင်ဖို့ **`/logout`** route ကို navigate လုပ်လိုက်တာ ဖြစ်ပါတယ်
- logout component နဲ့ /logout တွေ လုပ်ပေးရပါမယ်
```js
//src/components/
//Logout.tsx
import { Typography } from "@mui/material";
import Layout from "./Layout";

const Logout = () => {
  return (
    <Layout>
      <Typography variant="h4" sx={{ textAlign: "center", mt: 5 }}>
        You are now logged out.
      </Typography>
    </Layout>
  );
};

export default Logout;
```
```js
import React from "react";
import ReactDOM from "react-dom/client";
import "./index.css";
import App from "./App";
import { RouterProvider, createBrowserRouter } from "react-router-dom";
import Register from "./components/Register";
import Login from "./components/Login";
import Logout from "./components/Logout";
import AppProvider from "./contexts/AppContext";
import Menus from "./components/Menus";

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
  {
    path: "/logout",
    element: <Logout />,
  },
 
]);

const root = ReactDOM.createRoot(
  document.getElementById("root") as HTMLElement
);
root.render(
  <AppProvider>
    <RouterProvider router={routes} />
  </AppProvider>
);
```
- ခူ foodie pos app မှာ စစ ၀င်လာတာနဲ့ login ကို လုပ်ခိုင်းမှာ ဖြစ်ပြီး log in ၀င်လိုက်တာနဲ့ app bar မှာ log out လုပ်လို့ရမယ့် ဟာလေး ပြောင်းပြပေးမှာဖြစ်ပြီး  log out ကို နှိပ်လိုက်တာနဲ့ log out component ဆီ ရောက်သွားသလိုapp bar မှာ log ငည လုပ်လို့ရမယ့် ဟာလေး ပြောင်းပြပေးမှာဖြစ်ပါတယ်
