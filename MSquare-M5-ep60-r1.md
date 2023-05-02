## MSquare Programing Fullstack Course
### Episode-*60* 
### Summary For `Room(1)` intermediate Class
##
### Add ***mui drawer*** in project
- NavBar component မှာ ***mui drawer*** တစ်ခု ထည့်ပါမယ်
- draw မှာ ပြပေးမယ့် item တွေကို array တစ်ခု လုပ်ပြီး loop လုပ်ကာ ပြပေးမှာဖြစ်ပါတယ်
```js
import AppBar from "@mui/material/AppBar";
import Box from "@mui/material/Box";
import Toolbar from "@mui/material/Toolbar";
import Typography from "@mui/material/Typography";
import Button from "@mui/material/Button";
import IconButton from "@mui/material/IconButton";
import MenuIcon from "@mui/icons-material/Menu";
import LocalDiningIcon from "@mui/icons-material/LocalDining";
import CategoryIcon from "@mui/icons-material/Category";
import SettingsIcon from "@mui/icons-material/Settings";
import ClassIcon from "@mui/icons-material/Class";
import LunchDiningIcon from "@mui/icons-material/LunchDining";
import {
  List,
  ListItem,
  ListItemButton,
  ListItemIcon,
  ListItemText,
  Divider,
  Drawer,
} from "@mui/material";
import { useContext, useState } from "react";
import { Link } from "react-router-dom";
import { AppContext } from "../contexts/AppContext";

//link and label for drawer
const sidebarMenuItems = [
  { id: 1, label: "Menus", icon: <LocalDiningIcon />, route: "/menus" },
  {
    id: 2,
    label: "Menu Categories",
    icon: <CategoryIcon />,
    route: "/menu-categories",
  },
  { id: 3, label: "Addons", icon: <LunchDiningIcon />, route: "/addons" },
  {
    id: 4,
    label: "Addon Categories",
    icon: <ClassIcon />,
    route: "/addon-categories",
  },
  { id: 5, label: "Settings", icon: <SettingsIcon />, route: "/settings" },
];

export default function NavBar() {
  const [open, setOpen] = useState<boolean>(false);

  // drawer open and close
  const toggleDrawer =
    (open: boolean) => (event: React.KeyboardEvent | React.MouseEvent) => {
      if (
        event.type === "keydown" &&
        ((event as React.KeyboardEvent).key === "Tab" ||
          (event as React.KeyboardEvent).key === "Shift")
      ) {
        return;
      }
      setOpen(open);
    };

    // show items in drawer
  const drawerContent = () => (
    <Box
      sx={{ width: 250 }}
      role="presentation"
      onClick={toggleDrawer(false)}
      onKeyDown={toggleDrawer(false)}
    >
      <List>
        {sidebarMenuItems.slice(0, 4).map((item) => (
          <Link
            to={item.route}
            key={item.id}
            style={{ textDecoration: "none", color: "#313131" }}
          >
            <ListItem disablePadding>
              <ListItemButton>
                <ListItemIcon>{item.icon}</ListItemIcon>
                <ListItemText primary={item.label} />
              </ListItemButton>
            </ListItem>
          </Link>
        ))}
      </List>
      <Divider />
      <List>
        {sidebarMenuItems.slice(-1).map((item) => (
          <Link
            to={item.route}
            key={item.id}
            style={{ textDecoration: "none", color: "#313131" }}
          >
            <ListItem disablePadding>
              <ListItemButton>
                <ListItemIcon>{item.icon}</ListItemIcon>
                <ListItemText primary={item.label} />
              </ListItemButton>
            </ListItem>
          </Link>
        ))}
      </List>
    </Box>
  );

  return (
    <Box sx={{ flexGrow: 1 }}>
      <AppBar position="static">
        <Toolbar>
          <IconButton
            onClick={() => setOpen(true)}
            size="large"
            edge="start"
            color="inherit"
            aria-label="menu"
            sx={{ mr: 2 }}
          >
            <MenuIcon />
          </IconButton>
          <Typography variant="h6" component="div" sx={{ flexGrow: 1 }}>
            News
          </Typography>
          <Button color="inherit">Login</Button>
        </Toolbar>
      </AppBar>
      
      {/* render drawer content */}
      <Drawer anchor="left" open={open} onClose={toggleDrawer(false)}>
        {drawerContent()}
      </Drawer>
    </Box>
  );
}

```
- drawer ကအဆင်ပြေသွားပါပြီး 
- 
- ခု ဘယ် route ကို ပဲ ၀င်၀င် AppBar component ကို ပြပေးနိုင်အောင် လုပ်ပါမယ်
```js
//Layout.tsx
import React from "react";
import { JsxElement } from "typescript";
import NavBar from "./NavBar";
import Routes from "./Routes";

interface Props {
  children: string | JSX.Element | JSX.Element[];
}

const Layout = (props: Props) => {
  return (
    <div>
      <NavBar />
      {props.children}
    </div>
  );
};

export default Layout;

```
- ကျန်တဲ့ component တွေကို layout component နဲ့ wrap လုပ်ပေးရမှာဖြစ်ပါတယ်။
```js
//AddonCategories.tsx
import React from "react";
import Layout from "./Layout";

const AddonCategories = () => {
  return (
    <Layout>
      <div>Addon Categories</div>
    </Layout>
  );
};

export default AddonCategories;
```
```js
//Addon.tsx

import React from "react";
import Layout from "./Layout";

const Addons = () => {
  return (
    <Layout>
      <div>Addon page</div>
    </Layout>
  );
};

export default Addons;
```
```js
//MenuCategories.tsx

import React from "react";
import Layout from "./Layout";

const MenuCategories = () => {
  return (
    <Layout>
      <div>Menu Categories</div>
    </Layout>
  );
};

export default MenuCategories;

```
```js
//Menus.tsx

import React from "react";
import Layout from "./Layout";

const Menus = () => {
  return (
    <Layout>
      <div>Menu Page</div>
    </Layout>
  );
};

export default Menus;

```
- ခုဆို ဘယ်route ကိုပဲ ၀င်၀င် AppBar  ကို ပြပေးနေတာမြင်ရမှာပါ
## 
### ဆက်ပြီး AppBar မှာပြတဲ့ News ဆိုတဲ့နေရာမှာ လက်ရှိရောက်နေတဲ့ route ကို ပြပေးမှာဖြစ်ပါတယ်။

![enter image description here](https://raw.githubusercontent.com/Aungmyanmar32/msquare-m5-summaries/main/Screenshot%202023-05-02%20142725.png)
- react app မှာ စမ်းသပ်ကြည့်ပါက ၀င်လိုက်တဲ့ route အလိုက် title ကို ပြပေးမှာဖြစ်ပါတယ်။

![enter image description here](https://raw.githubusercontent.com/Aungmyanmar32/msquare-m5-summaries/main/Screenshot%202023-05-02%20142812.png)
##
### ဆက်ပြီး menu component မှာ menu တွေ create လုပ်နိုင်အောင် လုပ်ကြပါမယ်။
```js
//Menus.tsx

import React, { useState } from "react";
import Layout from "./Layout";
import { Box, Button, TextField } from "@mui/material";
import { Menu } from "../types/types";

const Menus = () => {
  const [menu, setMenu] = useState<Menu>({
    name: "",
    price: 0,
  });

  const createMenu = () => {
    if (!menu.name) return alert("please enter menu name");
    console.log(menu);
  };

  return (
    <Layout>
      <Box
        sx={{
          display: "flex",
          flexDirection: "column",
          maxWidth: 300,
          m: "0 auto",
        }}
      >
        <h1 style={{ textAlign: "center" }}>Create a new menu</h1>
        <TextField
          label="Name"
          variant="outlined"
          type="text"
          sx={{ mb: 2 }}
          onChange={(evt) => setMenu({ ...menu, name: evt.target.value })}
        />
        <TextField
          label="Price"
          variant="outlined"
          type="number"
          sx={{ mb: 2 }}
          onChange={(evt) =>
            setMenu({ ...menu, price: parseInt(evt.target.value, 10) })
          }
        />
        <Button variant="contained" onClick={createMenu}>
          Create
        </Button>
      </Box>
    </Layout>
  );
};

export default Menus;

```
- menu state တစ်ခု လုပ်ထားပြီး မူလတန်ဖိုးအဖြစ် name  ကို " " နဲ့ price  ကို 0 ပေးထားပါတယ်
- text field တွေမှာ ရိုက်ထည့်လိုက်တာတွေကို setMenu နဲ့ update လုပ်ပြီး menu မှာ သိမ်းလိုက်တာဖြစ်ပါတယ်။

##
### သိမ်းလိုက်တဲ့ menu item တွေကို server ဆီ ပို့လိုက်ပါမယ်
![enter image description here](https://raw.githubusercontent.com/Aungmyanmar32/msquare-m5-summaries/main/Screenshot%202023-05-02%20145833.png)

```js
//backend console
npm i cors @types/cors
```
```js
//backend/server.ts
//server.ts
//server.ts
import express, { Request, Response } from "express";
import cors from "cors";
const app = express();
const port = 5000;

app.use(express.json());
app.use(cors());

app.post("/menu", (req: Request, res: Response) => {
  console.log(req.body);
  res.send({ message: "New Menu item added" });
});

app.listen(port, () => console.log(`Example app listening on port ${port}!`));



```
- အကုန်ပြင်ဆင်ပြီးပြီ ဆိုရင် menu တစ်ခု create လုပ်ကြည့်လိုက်ပါမယ်

![enter image description here](https://raw.githubusercontent.com/Aungmyanmar32/msquare-m5-summaries/main/Screenshot%202023-05-02%20150454.png)

- create ကို နှိပ်လိုက်တဲ့အခါ  server က response ပြန်လာတဲ့ new menu item added ကို ပြပေးမှာဖြစ်ပါတယ်။
- server မှာလည်း log ထုတ်ပေးတာကို မြင်ရမှာပါ။

![enter image description here](https://raw.githubusercontent.com/Aungmyanmar32/msquare-m5-summaries/main/Screenshot%202023-05-02%20150505.png)

## 
### connect to database
- ခုနက post နဲ့ ၀င်လာတဲ့ data တွေကို database ဆီ ချိတ်ဆက်ပြီး သိမ်းပါမယ်
- backend မှာ db folder တစ်ခုလုပ်ပါမယ်။
- အဲ့အထဲမှာ db.ts လုပ်ပြီး pg module ကို သုံးကာ connect လုပ်ပါမယ်။
```js
//backend/db/db.ts
import { Pool } from "pg";

export const db = new Pool({
  host: "localhost",
  user: "postgres",
  password: "your-password",
  database: "happy_pos_db",
  max: 20,
  idleTimeoutMillis: 30000,
  connectionTimeoutMillis: 2000,
});
```
```js
//backend/server.ts

//server.ts
import express, { Request, Response } from "express";
import cors from "cors";
import { db } from "./db/db";
const app = express();
const port = 5000;

app.use(express.json());
app.use(cors());

app.post("/menu", async (req: Request, res: Response) => {
  const { name, price } = req.body;
  const text =
    "INSERT INTO menus(name, price) VALUES($1, $2) RETURNING *";
  const values = [name, price];
  const result = await db.query(text, values);
  res.send({ result });
});

app.listen(port, () => console.log(`Example app listening on port ${port}!`));

```
- ခနက လိုပဲ menu တစ်ခု create လုပ်ကြည့်ပြီး database မှာ  သွားကြည့်ပါက Ice tea ဆိုပြီး menu တစ်ခု၀င်လာတာကို မြင်ရမှာဖြစ်ပါတယ်။
![enter image description here](https://raw.githubusercontent.com/Aungmyanmar32/msquare-m5-summaries/main/Screenshot%202023-05-02%20150454.png)

![enter image description here](https://raw.githubusercontent.com/Aungmyanmar32/msquare-m5-summaries/main/Screenshot%202023-05-02%20151951.png)

