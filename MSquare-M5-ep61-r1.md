## MSquare Programing Fullstack Course
### Episode-*61* 
### Summary For `Room(1)` intermediate Class
##
### fetch data from database and update context value

- happy pos app ကို စလိုက်တာနဲ့ db ထဲမှာ ရှိတဲ့ data တွေ fecth လုပ်ပြီး ပြပေးနိုင်အောင် လုပ်ကြပါမယ်။
- အရင်ဆုံး .env  ဖိုင် တစ်ခု လုပ်ပြီး api endpoint  တွေကို သတ်မှတ်ပါမယ်။
```js
// env

REACT_APP_API_BASE_URL = http://localhost:5000
```
- ပြီးရင် src folder အောက်မှာ configs folder တစ်ခုလုပ်ပြီး config file တစ်ခု ထပ်လုပ်ပါမယ်။

```js
// src/configs/config.ts

interface Config {
  apiBaseUrl: string;
}

export const config: Config = {
  apiBaseUrl: process.env.REACT_APP_API_BASE_URL || "",
};
```
- ဆက်ပြီးတော့ cnotext component မှ တဆင့် backend server ဆီ request လုပ်ပြီး response ပြန်လာတဲ့ data ကို context value အဖြစ် update လုပ်မှာ ဖြစ်ပါတယ်
```js
//AppContext.tsx

import { createContext, useEffect, useState } from "react";
import { Addon, AddonCategories, Menu, MenuCategories } from "../types/types";
import { config } from "../configs/config";

interface AppContextTypes {
  menus: Menu[];
  menuCategories: MenuCategories[];
  addons: Addon[];
  addonCategories: AddonCategories[];
  updateData: (value: any) => void;
  fetchData: () => void;
}
const defaultAppContext = {
  menus: [],
  menuCategories: [],
  addons: [],
  addonCategories: [],
  updateData: (value: any) => {},
  fetchData: () => {},
};

export const AppContext = createContext<AppContextTypes>(defaultAppContext);

const AppProvider = (props: any) => {
  const [data, setData] = useState(defaultAppContext);

  useEffect(() => {
    fetchData();
  }, []);

  const fetchData = async () => {
    const response = await fetch(`${config.apiBaseUrl}/data`);
    const responseJson = response.json();
    console.log(responseJson);
  };

  return (
    <AppContext.Provider value={{ ...data, updateData: setData }}>
      {props.children}
    </AppContext.Provider>
  );
};
export default AppProvider;

```

- ပြီးရင် express server မှာ request ကို လက်ခံပြီး database နဲ့ ဆက်သွယ် ကာ response ပြန်ပေးမှာဖြစ်ပါတယ်. 
```js
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
  const text = "INSERT INTO menus(name, price) VALUES($1, $2) RETURNING *";
  const values = [name, price];
  const result = await db.query(text, values);
  res.send({ result });
});

app.get("/data", async (req: Request, res: Response) => {
  const menus = await db.query("select * from menus");
  const menuCategories = await db.query("select * from menu_categories");
  const addons = await db.query("select * from addons");
  const addonCategories = await db.query("select * from addon_categories");

  res.send({
    menus: menus.rows,
    menuCategories: menuCategories.rows,
    addons: addons.rows,
    addonCategories: addonCategories.rows,
  });
});

app.listen(port, () => console.log(`Example app listening on port ${port}!`));
```
- /data route ကို request ၀င်လာရင် databse ဆီ query လှမ်းပို့ပြီး data တွေ ယူကာ ရလာတဲ့ data တွေကို response လုပ်ပေးလိုက်တာဖြစ်ပါတယ်။
- ခု context ကို အသုံပြုနိုင်ဖို့ index.tsx မှာ AppProvider နဲ့ wrap လုပ်ပေးလိုက်ပါမယ်။
```js
//index.tsx

import React from "react";
import ReactDOM from "react-dom/client";
import "./index.css";
import App from "./App";
import reportWebVitals from "./reportWebVitals";
import { createBrowserRouter, RouterProvider } from "react-router-dom";
import Menus from "./components/Menus";
import MenuCategories from "./components/MenuCategories";
import Addons from "./components/Addons";
import AddonCategories from "./components/AddonCategories";
import AppProvider from "./contexts/AppContext";

const router = createBrowserRouter([
  {
    path: "/",
    element: <App />,
  },
  {
    path: "/menus",
    element: <Menus />,
  },
  {
    path: "/menu-categories",
    element: <MenuCategories />,
  },
  {
    path: "/addons",
    element: <Addons />,
  },
  {
    path: "/addon-categories",
    element: <AddonCategories />,
  },
]);

const root = ReactDOM.createRoot(
  document.getElementById("root") as HTMLElement
);
root.render(
  <AppProvider>
    <RouterProvider router={router} />
  </AppProvider>
);

// If you want to start measuring performance in your app, pass a function
// to log results (for example: reportWebVitals(console.log))
// or send to an analytics endpoint. Learn more: https://bit.ly/CRA-vitals
reportWebVitals();
```
- ခု app ကို refresh လုပ်ကြည့်လိုက်ပါက useEffect ကနေ server ဆီ request လုပ်လိုက်ပြီး response ပြန်လာတဲ့ data ကို ;log ထုတ်ပေးတာ မြင်ရမှာပါ။
- အဲ့ဒီရလာတဲ့ data တွေကို context value အဖြစ် update လုပ်မှာဖြစ်ပါတယ်။

```js
//AppContext.tsx

import { createContext, useEffect, useState } from "react";
import { Addon, AddonCategories, Menu, MenuCategories } from "../types/types";
import { config } from "../configs/config";

interface AppContextTypes {
  menus: Menu[];
  menuCategories: MenuCategories[];
  addons: Addon[];
  addonCategories: AddonCategories[];
  updateData: (value: any) => void;
  fetchData: () => void;
}
const defaultAppContext = {
  menus: [],
  menuCategories: [],
  addons: [],
  addonCategories: [],
  updateData: (value: any) => {},
  fetchData: () => {},
};

export const AppContext = createContext<AppContextTypes>(defaultAppContext);

const AppProvider = (props: any) => {
  const [data, updateData] = useState(defaultAppContext);

  console.log("context value is", data);

  useEffect(() => {
    fetchData();
  }, []);

  const fetchData = async () => {
    const response = await fetch(`${config.apiBaseUrl}/data`);
    const responseJson = await response.json();
    console.log(responseJson);

    const { menus, menuCategories, addons, addonCategories } = responseJson;

    updateData({
      ...data,
      menus,
      menuCategories,
      addons,
      addonCategories,
    });
  };

  return (
    <AppContext.Provider value={{ ...data, updateData,fetchData }}>
      {props.children}
    </AppContext.Provider>
  );
};
export default AppProvider;
```
- server ဆီ fetch လုပ်ပြီး ရလာတဲ့ data တွေကို updateData ကို အသုံးပြုပြီး state ကို update လုပ်ပေးလိုက်တာဖြစ်ပါတယ်
- အခုဆို context value မှာလည်း database က data တွေ ကို သိမ်းထားနိုင်ပြီးဖြစ်ပါတယ်။

##

### Menus component မှာ menu item တစ်ခု create လုပ်လိုက်တိုင်း serveru  data တွေကို fetch ပြန်လုပ်ပြီး create လုပ်လိုက်တဲ့ menu item ပါ ပါလာအောင် လုပ်ပါမယ်
```js
//Menus.tsx

import React, { useContext, useState } from "react";
import Layout from "./Layout";
import { Box, Button, TextField } from "@mui/material";
import { Menu } from "../types/types";
import { AppContext } from "../contexts/AppContext";

const Menus = () => {
  const [menu, setMenu] = useState<Menu>({
    name: "",
    price: 0,
  });

  const contextValue = useContext(AppContext);
  const { updateData, fetchData, menus } = contextValue;

  const createMenu = async () => {
    if (!menu.name) return alert("please enter menu name");
    const response = await fetch("http://localhost:5000/menu", {
      method: "POST",
      headers: {
        "Content-Type": "application/json",
      },
      body: JSON.stringify(menu),
    });
    const data = await response.json();
    fetchData();
    setMenu({ name: "", price: 0 });
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
```
  const contextValue = useContext(AppContext);
  const { updateData, fetchData, menus } = contextValue;
  ```
  - App context ကို အသုံးပြုလိုက်ပြီး ရလာတဲ့ context value ထဲက `{ updateData, fetchData, menus }` တို့ကို ဆွဲထုတ်လိုက်တာပါ။
  ```
  const createMenu = async () => {
    if (!menu.name) return alert("please enter menu name");
    const response = await fetch("http://localhost:5000/menu", {
      method: "POST",
      headers: {
        "Content-Type": "application/json",
      },
      body: JSON.stringify(menu),
    });
    const data = await response.json();
    fetchData();
    setMenu({ name: "", price: 0 });
  };
```
- create menu function မှာ fetchData ကို ခေါ်ပေးလိုက်တာမလို့ menu တစ်ခု create လုပ်တိုင်း server ဆီ fetch လုပ်ကာ update ဖြစ်တဲ့ data ကို ရမှာဖြစ်ပါတယ်။
- ရလာတဲ့ data တွေထဲက menus မှာပါတဲ့ items တွေကို UI မှာ ပြနိုင်အောင် လုပ်ကြည့်ပြီး delete လုပ်လို့ ရအောင်ပါ ရေးကြည့်ကြပါ။
### Hint for show and delete menu
```js
//frontend
 <Box
        sx={{
          display: "flex",
          flexDirection: "column",
          marginX: "auto",
          marginY: 2,
          alignItems: "center",
        }}
      >
        <h1> Menu List</h1>
        <Stack direction="column" spacing={1}>
          {menus.map((menu) => (
            <Box key={menu.id}>
              <Chip
                sx={{ cursor: "pointer" }}
                label={menu.name}
                variant="outlined"
                onDelete={() => handleDelete(menu.id)}
              />
            </Box>
          ))}
        </Stack>
      </Box>

//fetch function
const handleDelete = async (menuId: number | undefined) => {
    alert("you delete this menu");
    await fetch(`${config.apiBaseUrl}/menus/${menuId}`, {
      method: "DELETE",
    });
    fetchData();
  };
   
```
      
      
### Server
```js
app.delete("/menus/:menuId", async (req: Request, res: Response) => {
  const menuId = parseInt(req.params.menuId, 10);
  if (!menuId) return res.send("menu id is required");
  const text = "DELETE FROM menus WHERE id =($1) RETURNING *";
  const values = [menuId];
  const result = await db.query(text, values);
  res.send({ result });
});
```
##
### ဆက်ပြီး location table တစ်ခုလုပ်ပြီး menu tableနဲ့ join လုပ်ပါမယ်။
```sql
CREATE TABLE locations (
id serial PRIMARY KEY,
name text NOT NULL,
address text 
)
```
```sql
CREATE TABLE menus_locations (
id serial PRIMARY KEY,
menu_id INTEGER REFERENCES menus,
location_id INTEGER REFERENCES locations,
is_available boolean 
)
```
- location table မှာ location တစ်ချို့ ထည့်ပြီး menu ထဲက items နဲ့ ချိတ်ဆက်ပါမယ်

