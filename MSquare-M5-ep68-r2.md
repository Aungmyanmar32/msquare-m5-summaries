## MSquare Programing Fullstack Course
### Episode-*68* 
### Summary For `Room(2)`
## Setup *POS* app (Real World Project)
- အခု သင်ခန်းစာက စပြီး တကယ့် လက်တွေ့ project တစ်ခုကို လုပ်သွားကြမှာဖြစ်ပါတယ်။
- အရင်ဆုံး github repo တစ်ခုကို တည်ဆောက်ပြီး clone လုပ်ထားပါ။
- ရလာတဲ့ folder ထဲမှာ react app တစ်ခုကို frontend အမည်ပေးပြီး create လုပ်ပါ။
```colsole
npx create-react-app frontend  --template typescript
```
- ပြီးရင် backend folder တစ်ခု လုပ်ပြီး express server တစ်ခု လုပ်ထားပါမယ်
- စစစစစစ
- ဆက်ပြီး project folder အောက်မှာပဲ **.gitignore** ဖိုင်တစ်ခုလုပ်ပြီး 
```
**/node_modules 
**/builds
```
- ကို ထည့်ပေးလိုက်ပါမယ်။
- စစစစစစ
### frontend folder ထဲက **.gitignore** ဖိုင် ကို ဖျက်ပေးလိုက်ပြီး

```sql
cd ..
git add .
git commit
git push 

```
- လုပ်ပေးလိုက်ပါမယ်
##
### front-end Setup
- frontend  / src အောက်မှာ components folder တစ်ခုလုပ်ပါမယ်။
- components folder ထဲမှာ NavBar component တစ်ခု လုပ်ပါမယ်။
- NavBar component ထဲမှာ MUI က App bar  ကို အသုံးပြုပါမယ်။
 ```js
 import * as React from "react";
import AppBar from "@mui/material/AppBar";
import Box from "@mui/material/Box";
import Toolbar from "@mui/material/Toolbar";
import Typography from "@mui/material/Typography";
import IconButton from "@mui/material/IconButton";
import MenuIcon from "@mui/icons-material/Menu";
import AccountCircle from "@mui/icons-material/AccountCircle";
import Switch from "@mui/material/Switch";
import FormControlLabel from "@mui/material/FormControlLabel";
import FormGroup from "@mui/material/FormGroup";
import MenuItem from "@mui/material/MenuItem";
import Menu from "@mui/material/Menu";

const NavBar = () => {
  const [auth, setAuth] = React.useState(true);
  const [anchorEl, setAnchorEl] = React.useState<null | HTMLElement>(null);

  const handleChange = (event: React.ChangeEvent<HTMLInputElement>) => {
    setAuth(event.target.checked);
  };

  const handleMenu = (event: React.MouseEvent<HTMLElement>) => {
    setAnchorEl(event.currentTarget);
  };

  const handleClose = () => {
    setAnchorEl(null);
  };

  return (
    <Box sx={{ flexGrow: 1 }}>
      <AppBar position="static">
        <Toolbar>
          <IconButton
            size="large"
            edge="start"
            color="inherit"
            aria-label="menu"
            sx={{ mr: 2 }}
          >
            <MenuIcon />
          </IconButton>
          <Typography variant="h6" component="div" sx={{ flexGrow: 1 }}>
            Foodie POS
          </Typography>
          {auth && (
            <div>
              <IconButton
                size="large"
                aria-label="account of current user"
                aria-controls="menu-appbar"
                aria-haspopup="true"
                onClick={handleMenu}
                color="inherit"
              >
                <AccountCircle />
              </IconButton>
              <Menu
                id="menu-appbar"
                anchorEl={anchorEl}
                anchorOrigin={{
                  vertical: "top",
                  horizontal: "right",
                }}
                keepMounted
                transformOrigin={{
                  vertical: "top",
                  horizontal: "right",
                }}
                open={Boolean(anchorEl)}
                onClose={handleClose}
              >
                <MenuItem onClick={handleClose}>Profile</MenuItem>
                <MenuItem onClick={handleClose}>My account</MenuItem>
              </Menu>
            </div>
          )}
        </Toolbar>
      </AppBar>
    </Box>
  );
};

export default NavBar;
```
> ရှင်းလင်းချက်

- Mui က app bar component တစ်ခုကို copy ထည့်လိုက်တာဖြစ်ပြီး
menu icon နောက်က Typography မှာ မိမိ app name ကိုပဲ ထည့်ပေးလိုက်တာဖြစ်ပါတယ်။

###  ဆက်ပြီး Register component တစ်ခုလုပ်ပါမယ်။
```js
import { Box, Button, TextField } from "@mui/material";
import { useState } from "react";

const Register = () => {
  const [user, setUser] = useState({ name: "", email: "", password: "" });

  const register = async () => {
    const response = await fetch("http://localhost:5000/auth/register", {
      method: "POST",
      headers: {
        "Content-Type": "application/json",
      },
      body: JSON.stringify(user),
    });
    console.log(await response.json());
  };

  return (
    <Box sx={{ display: "flex", flexDirection: "column", mt: 5 }}>
      <TextField
        label="Name"
        variant="outlined"
        sx={{ minWidth: "300px", mb: 2 }}
        onChange={(evt) => setUser({ ...user, name: evt.target.value })}
      />
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
      <Button variant="contained" onClick={register}>
        Register
      </Button>
    </Box>
  );
};

export default Register;
```
- name ,email , password ဆိုတဲ့ text input သုံးခုလုပ်ထားပြီး input တွေမှာ change လိုက်တိုင်း setUser function ကို ခေါ်ပြီး user state ကို update လုပ်ထားတာဖြစ်ပါတယ်။
- နောက်ထပ် Register button တစ်ခု ထည့်ပေးထားပြီး click လုပ်လိုက်တဲ့အခါ
- server ဆီ POST method နဲ့ request ပို့လိုက်ပြီး user ရဲ့ value တွေကိုပါ payload ( body) အနေနဲ့ ထည့်ပေးထားတာဖြစ်ပါတယ်။
- လုပ်ထားတဲ့ NavBar  နဲ့ Register component နှစ်ခုလုံးကို App.tsx မှာ render လုပ်ပေးလိုက်ပါမယ်။
```js
//App.tsx

import React from "react";
import logo from "./logo.svg";
import "./App.css";
import NavBar from "./components/NavBar";
import Register from "./components/Register";

function App() {
  return (
    <div className="App">
      <NavBar />
      <div className="container">
        <Register />
      </div>
```
- အထက်ပါ request ကို backend server မှာ လက်ခံပါမယ်။
```js
// backend/server.ts

import cors from "cors";
import express from "express";
import { db } from "./src/db/db";
const app = express();
const port = 5006;
app.use(cors());
app.use(express.json());

app.post("/auth/register", async (req, res) => {
  const { name, email, password } = req.body;
 
    res.send({name,email,password});
  
});

app.listen(port, () => console.log(`Example app listening on port ${port}!`));

```
- **/auth/register** route ကနေ post method နဲ့ ၀င်လာတဲ့ request ကို လက်ခံလိုက်ပါတယ်
- request နဲ့ အတူပါလာတဲ့ data ( body) ကို **{ name, email, password }** ဆိုပြီး သိမ်းလိုက်ပါတယ်
- ပြီးတော့ ရလာတဲ့ data ကိုပဲ response ပြန်လိုက်တာဖြစ်ပါတယ်
- ခု foodie pos app မှာ user info တွေ ရိုက်ထည့်ပြီး register လုပ်ကြည့်ပါက ထည့်လိုက်တဲ့ data အတိုင်း console မှာ log ထုတ်ပေးနေတာကို မြင်ရမှာပါ။
