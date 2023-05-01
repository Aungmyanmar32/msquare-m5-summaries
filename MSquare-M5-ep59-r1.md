## MSquare Programing Fullstack Course
### Episode-*50* 
### Summary For `Room(1)` intermediate Class
##
### Start real-world project ( happy-pos)
- တကယ့် project တစ်ခု ကို စပြီး setup လုပ်တော့မှာဖြစ်ပါတယ်။
- **happy-pos** project folder တစ်ခုလုပ်ပြီး
   - react app ကို frontend ဆိုတဲ့ folder မှာ install လုပ်လိုက်ပါမယ်
   
```console
 npx create-react-app frontend --template typescript
```
- ဆက်ပြီး backend folder တစ်ခုလုပ်ပြီး express server ကို install လုပ်လိုက်ပါမယ်
```console
mkdir backend && cd backend
npm init -y
npm i express @types/express ts-node-dev pg
touch server.ts
```
```js
//server.ts
import express from "express";
const app = express();
const port = 5000;

app.get("/", (req, res) => res.send("Hello World!"));
app.listen(port, () => console.log(`Example app listening on port ${port}!`));

```
   - ပြီးရင် happy-pos folder မှာ git init လုပ်ပြီး node module folder ကို ingnore ထားကာ github repo အသစ်တစ်ခုနဲ့ ချိတ်ဆက်ပေးထားလိုက်ပါမယ်။
   ##
### create layout component in react
- frontend ထဲက src အောက်မှာ components folder တစ်ခု ဆောက်ပါမယ်။
- components folder ထဲမှာ Layout.tsx ကို ထပ်လုပ်မှာဖြစ်ပါတယ်။
```js
//Layout.tsx

import React from "react";
import { JsxElement } from "typescript";

interface Props {
  children: string | JSX.Element | JSX.Element[];
}

const Layout = (props: Props) => {
  return <div>{props.children}</div>;
};

export default Layout;
```
- အဲ့ဒီ Layout component ကို app component  ထဲမှာ သုံးလိုက်ပါမယ်။
```js
//App.tsx

import React from "react";
import logo from "./logo.svg";
import "./App.css";
import Layout from "./components/Layout";

function App() {
  return (
    <div className="App">
      <Layout>
        <h1>Hello</h1>
      </Layout>
    </div>
  );
}

export default App;

```
- ဆက်ပြီး project မှာ MUI ကို သုံးမှာမလို့ install လုပ်ပေးရပါမယ်
```console
npm i @mui/material @emotion/react @emotion/styled @mui/icons-material
```
- ဆက်ပြီး Mui app bar ကို သုံးပြီး NavBar component တစ်ခုလုပ်ပါမယ်
```js
// Navbar.tsx
import * as React from "react";
import AppBar from "@mui/material/AppBar";
import Box from "@mui/material/Box";
import Toolbar from "@mui/material/Toolbar";
import Typography from "@mui/material/Typography";
import Button from "@mui/material/Button";
import IconButton from "@mui/material/IconButton";
import MenuIcon from "@mui/icons-material/Menu";

export default function NavBar() {
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
            News
          </Typography>
          <Button color="inherit">Login</Button>
        </Toolbar>
      </AppBar>
    </Box>
  );
}
```
- Navbar component ကို Layout component မှာ သုံးလိုက်ပါမယ်
```js
//Layout.tsx

import React from "react";
import { JsxElement } from "typescript";
import NavBar from "./NavBar";

interface Props {
  children: string | JSX.Element | JSX.Element[];
}

const Layout = (props: Props) => {
  return (
    <div>
      <NavBar />
    </div>
  );
};

export default Layout;
```
##
### ဆက်ပြီးတော့ menus , menuCategories,addons,addonCategories ဆိုတဲ့ component လေးခုလုပ်ပါမယ်
```console
/happy-pos/frontend/src/components 

//terminal
touch Menus.tsx MenuCategories.tsx Addons.tsx AddonCategories.tsx
```
```js
//AddonCategories.tsx
import React from "react";

const AddonCategories = () => {
  return <div>Addon Categories</div>;
};

export default AddonCategories;

```
```js
//Addon.tsx

import React from "react";

const Addons = () => {
  return <div>Addons</div>;
};

export default Addons;

```
```js
//MenuCategories.tsx

import React from "react";

const MenuCategories = () => {
  return <div>Menu Categories</div>;
};

export default MenuCategories;

```
```js
//Menus.tsx

import React from "react";

const Menus = () => {
  return <div>Menus</div>;
};

export default Menus;

```
##
### Route component တစ်ခု မှာ route တွေလုပ်ပြီး အထက်က component လေးခုကို link လုပ်လိုက်ပါမယ်။
- route တွေ အသုံးပြုမှာ မလို့ react-router-dom ကို install လုပ်ပေးလိုက်ပါမယ်။
```sh
npm install react-router-dom
```
- Route component တစ်ခုလုပ်ပြီး link လုပ်ပါမယ်။
```js
//Routes.tsx

import React from "react";
import { Link, BrowserRouter } from "react-router-dom";

const Routes = () => {
  return (
    <>
      <Link to="/">Home |</Link>
      <Link to="/menus"> Menu |</Link>
      <Link to="/menu-categories"> Menu Categories |</Link>
      <Link to="/addons"> Addon |</Link>
      <Link to="/addon-categories"> Addon Categories</Link>
    </>
  );
};

export default Routes;

```
- ပြီးရင် index.tsx မှာ react router တွေ လုပ်ပါမယ်။
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
  <>
    <RouterProvider router={router} />
  </>
);

// If you want to start measuring performance in your app, pass a function
// to log results (for example: reportWebVitals(console.log))
// or send to an analytics endpoint. Learn more: https://bit.ly/CRA-vitals
reportWebVitals();

```
- Layout component မှာ Routes component ကို render လုပ်လိုက်ပါမယ်။
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
      <Routes />
    </div>
  );
};

export default Layout;

```
- အဲ့ဒါဆိုရင် အောက်ကပုံလို မြင်ရမှာ ဖြစ်ပါတယ်။

![enter image description here](https://raw.githubusercontent.com/Aungmyanmar32/msquare-m5-summaries/main/Screenshot%202023-05-01%20125451.png)

##
### create context and provider
- react app ထဲမှာ data တွေ ခေါ်သုံးနိုင်ဖို့ context တစ်ခု လုပ်ပါမယ်။
- ts နဲ့ ရေးမှာမလို့ type တွေအတွက် သီးသန့်folder တစ်ခု လုပ်ပြီး type လုပ်ပါမယ်
```console
cd frontend/
cd src/
mkdir types && cd types
touch types.ts
```

```js
//types.ts

interface BaseTypes {
  name: string;
}

export interface Menu extends BaseTypes {
  price: number;
}

export interface MenuCategories extends BaseTypes {}

export interface Addon extends BaseTypes {
  price: number;
  isAvailable: boolean;
  addonCategoriesIds: string[];
}

export interface AddonCategories extends BaseTypes {
  isRequired: boolean;
}

```
- context တစ်ခု create လုပ်ပါမယ်။
- src/contexts/Appcontext.tsx ဖိုင်တစ်ခုလုပ်ပါ။
```js
//AppContext.tsx

import { createContext, useState } from "react";
import { Addon, AddonCategories, Menu, MenuCategories } from "../types/types";

interface AppContextTypes {
  menus: Menu[];
  menuCategories: MenuCategories[];
  addons: Addon[];
  addonCategories: AddonCategories[];
}
const defaultAppContext = {
  menus: [],
  menuCategories: [],
  addons: [],
  addonCategories: [],
};

export const AppContext = createContext<AppContextTypes>(defaultAppContext);

const AppProvider = (props: any) => {
  const [data, setData] = useState(defaultAppContext);

  return (
    <AppContext.Provider value={{ ...data }}>
      {props.children}
    </AppContext.Provider>
  );
};
export default AppProvider;

```
