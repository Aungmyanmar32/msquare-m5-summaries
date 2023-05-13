## MSquare Programing Fullstack Course
### Episode-*71* 
### Summary For `Room(2)`
### - Add mui drawer 
### - create route for drawer-menu items
##
### Mui app bar ( NavBar ) မှာ menu icon ကို နှိပ်လိုက်တဲ့အချိန်မှာ drawer menu list တစ်ခုကို ပြပေးနိုင်အောင် လုပ်ပါမယ်
- အရင်ဆုံး drawer မှာ ပြမယ့် menu items တွေကို array တစ်ခုထဲထည့်ပြီး သတ်မှတ်လိုက်ပါမယ်
```js
// add to NavBar.tsx

const sidebarMenuItems = [
  { id: 1, label: "Orders", icon: <LocalMallIcon />, route: "/orders" },
  { id: 2, label: "Menus", icon: <LocalDiningIcon />, route: "/menus" },
  {
    id: 3,
    label: "Menu Categories",
    icon: <CategoryIcon />,
    route: "/menu-categories",
  },
  { id: 4, label: "Addons", icon: <LunchDiningIcon />, route: "/addons" },
  {
    id: 5,
    label: "Addon Categories",
    icon: <ClassIcon />,
    route: "/addon-categories",
  },
  {
    id: 6,
    label: "Locations",
    icon: <LocationOnIcon />,
    route: "/locations",
  },
  { id: 7, label: "Settings", icon: <SettingsIcon />, route: "/settings" },
];
```
- open ဆိုတဲ့ state တစ်ခုလုပ်ပြီး မူလတန်ဖိုးအနေနဲ့ flase ကို သတ်မှတ်လိုက်ပါမယ်

![enter image description here](https://github.com/Aungmyanmar32/msquare-m5-summaries/blob/main/Screenshot%202023-05-13%20170859.png?raw=true)
- ပြီးရင် return လုပ်တဲ့နေရာမှာ App bar ရဲ့ အောက်မှာ Drawer component တစ်ခုကို ထပ်render လုပ်လိုက်ပါမယ်

![enter image description here](https://github.com/Aungmyanmar32/msquare-m5-summaries/blob/main/Screenshot%202023-05-13%20170603.png?raw=true)
- Mui Drawer component ကို render လုပ်ထားပါတယ်
- open props ကို open ( state ) အဖြစ် ပေးထားပါတယ်
- onClose မှာတော့ open ရဲ့ တန်ဖိုးကို setOpen နဲ့ false အဖြစ် update ပြန်လုပ်ထားပါတယ်
- Drawer ထဲမှာတော့ **renderDrawer** function ကို ခေါ်သုံးထားပါတယ်။
- **renderDrawer** function ကို return မလုပ်ခင်လေးမှာ သတ်မှတ်ပါမယ်

```js
// Add to NavBar.tsx
const renderDrawer = () => (
    <Box
      sx={{ width: 250 }}
      role="presentation"
      onClick={() => setOpen(false)}
      onKeyDown={() => setOpen(false)}
    >
      <List>
        {sidebarMenuItems.slice(0, 6).map((menuItem) => (
        
            <ListItem disablePadding>
              <ListItemButton>
                <ListItemIcon>{menuItem.icon}</ListItemIcon>
                <ListItemText primary={menuItem.label} />
              </ListItemButton>
            </ListItem>
          
        ))}
      </List>
      <Divider />
      <List>
        {sidebarMenuItems.slice(-1).map((menuItem) => (
         
            <ListItem disablePadding>
              <ListItemButton>
                <ListItemIcon>{menuItem.icon}</ListItemIcon>
                <ListItemText primary={menuItem.label} />
              </ListItemButton>
            </ListItem>
          
        ))}
      </List>
    </Box>
  );

```
### ရှင်းလင်းချက်
```js
 <Box
      sx={{ width: 250 }}
      role="presentation"
      onClick={() => setOpen(false)}
      onKeyDown={() => setOpen(false)}
    >
      <List>
        -------
      </List>
      </Box>
```
- List တွေထည့်ထားတဲ့ Box တစ်ခု လုပ်ထားပါတယ်
- Box ထဲက link တစ်ခုခု onClick ဒါမှမဟုတ် onKeyDown လုပ်လိုက်တိုင်း setOpen(false) ဆိုပြီး open ရဲ့ value ကို false အဖြစ် update လုပ်ထားပါတယ်။

```js
 <List>
        {sidebarMenuItems.slice(0, 6).map((menuItem) => (
        
            <ListItem disablePadding>
              <ListItemButton>
                <ListItemIcon>{menuItem.icon}</ListItemIcon>
                <ListItemText primary={menuItem.label} />
              </ListItemButton>
            </ListItem>
          
        ))}
      </List>
```
- List အထဲမှာတော့ drawer မှာ ပြမယ့် menu items တွေပါတဲ့ array  ကို loop လုပ်ပြီး slice method ကို သုံးကာ နောက်ဆုံး item ဖြစ်တဲ့ setting ကို ချန်ပြီး ပြပေးထားတာဖြစ်ပါတယ်။

```js
 <Divider />
      <List>
        {sidebarMenuItems.slice(-1).map((menuItem) => (
         
            <ListItem disablePadding>
              <ListItemButton>
                <ListItemIcon>{menuItem.icon}</ListItemIcon>
                <ListItemText primary={menuItem.label} />
              </ListItemButton>
            </ListItem>
          
        ))}
      </List>
```
- ခုနက ချန်ထားတဲ့ setting item ကို <Divider /> နဲ့ ခွဲပြီး အောက်မှာ သီးသန့်ပြထားတာဖြစ်ပါတယ်
- ခု foodie app မှာ menu icon ကို နှိပ်ကြည့်ပါက ခုလို ပြပေးမှာဖြစ်ပါတယ်။

![enter image description here](https://github.com/Aungmyanmar32/msquare-m5-summaries/blob/main/drawer1.png?raw=true)

##
### ဆက်ပြီး drawer ထဲက menu items တွေကို click လိုက်ရင် သက်ဆိုင်ရာ route တွေဆီ သွားပြီး သက်ဆိုင်တဲ့ component ကို ပြပေးနိုင်အောင် လုပ်ပါမယ်။
- ဆိုလိုတာက Orders ကို click လိုက်ရင် /orders ဆိုတဲ့ route ကို သွားလိုက်ပြီး Orders component ကို ပြပေးမှာဖြစ်ပါတယ်
- အရင်ဆုံး လိုအပ်မယ့် component တွေကို create လုပ်ပါမယ်။
- src/components အောက်မှာ
  - Menus.tsx
  - MenuCategories.tsx
  - Orders.tsx
  - Locations.tsx
  - Settings.tsx
  - Addons.tsx
  - AddonCategories.tsx
- စတဲ့ component တွေ လုပ်ပါမယ်
```js
//Orders.tsx
import React from "react";
import NavBar from "./NavBar";

const Orders = () => {
  return (
    <div>
      <NavBar />
      <h1>Orders page</h1>
    </div>
  );
};

export default Orders;

```

```js
//AddonCategories.tsx
import React from "react";
import NavBar from "./NavBar";

const AddonCategories = () => {
  return (
    <div>
      <NavBar />
      <h1>AddonCategories page</h1>
    </div>
  );
};

export default AddonCategories;
```

```js
//MenuCategories.tsx
import React from "react";
import NavBar from "./NavBar";

const MenuCategories = () => {
  return (
    <div>
      <NavBar />
      <h1>MenuCategories page</h1>
    </div>
  );
};

export default MenuCategories;

```

```js
//Addons.tsx
import React from "react";
import NavBar from "./NavBar";

const Addons = () => {
  return (
    <div>
      <NavBar />
      <h1>Addons page</h1>
    </div>
  );
};

export default Addons;
```

```js
//Locations.tsx
import React from "react";
import NavBar from "./NavBar";

const Locations = () => {
  return (
    <div>
      <NavBar />
      <h1>Locations page</h1>
    </div>
  );
};

export default Locations;
```

```js
//settings.tsx
import React from "react";
import NavBar from "./NavBar";

const Settings = () => {
  return (
    <div>
      <NavBar />
      <h1>Settings page</h1>
    </div>
  );
};

export default Settings;

```

```js
//Menus.tsx
import React from "react";
import NavBar from "./NavBar";

const Menus = () => {
  return (
    <div>
      <NavBar />
      <h1>Menus page</h1>
    </div>
  );
};

export default Menus;

```
### ဆက်ပြီး route တွေ သတ်မှတ်ပါမယ်။
```js
//index.tsx

import React from "react";
import ReactDOM from "react-dom/client";
import "./index.css";
import App from "./App";

import { createBrowserRouter, RouterProvider } from "react-router-dom";
import Register from "./components/Register";
import Login from "./components/Login";
import Orders from "./components/Orders";
import Menus from "./components/Menus";
import Addons from "./components/Addons";
import AddonCategories from "./components/AddonCategories";
import MenuCategories from "./components/MenuCategories";
import Locations from "./components/Locations";
import Settings from "./components/Settings";

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
    path: "/orders",
    element: <Orders />,
  },
  {
    path: "/menus",
    element: <Menus />,
  },
  {
    path: "/addons",
    element: <Addons />,
  },
  {
    path: "/addon-categories",
    element: <AddonCategories />,
  },
  {
    path: "/menu-categories",
    element: <MenuCategories />,
  },
  {
    path: "/locations",
    element: <Locations />,
  },
  {
    path: "/settings",
    element: <Settings />,
  },
]);

const root = ReactDOM.createRoot(
  document.getElementById("root") as HTMLElement
);
root.render(<RouterProvider router={routes} />);

```
### react-router-dom က Link ကို သုံးပြီး drawer-items တွေကို loop လုပ်ပြီး ပြတဲ့အချိန်မှာ route ကို ပါ တစ်ပါတည်း ထည့်ပေးလိုက်မှာဖြစ်ပါတယ်
![enter image description here](https://github.com/Aungmyanmar32/msquare-m5-summaries/blob/main/Screenshot%202023-05-13%20175251.png?raw=true)
- react-router-dom က Link ကို သုံးပြီ drawer-menu item တွေကို route တွေကိုပါ to နဲ့ ချိတ်ပေးလိုက်တာဖြစ်ပါတယ်။
- ဆက်ပြီးတော့ အောက်မှာ သီးသန့်ခွဲထားတဲ့ settigs အတွက်လဲ အထက်ပါအတိုင်း route ချိတ်ပေးလိုက်ပါမယ်
```js
 <Divider />
      <List>
        {sidebarMenuItems.slice(-1).map((menuItem) => (
          <Link
            to={menuItem.route}
            key={menuItem.id}
            style={{ textDecoration: "none", color: "#313131" }}
          >
          <ListItem disablePadding>
            <ListItemButton>
              <ListItemIcon>{menuItem.icon}</ListItemIcon>
              <ListItemText primary={menuItem.label} />
            </ListItemButton>
          </ListItem>
          </Link>
```
- ခု foodie app မှာ menu icon  ကို နှိပ်ပြီး ပြပေးတဲ့ menu items တွေကို နှိပ်စမ်းကြည့်ပါက သက်ဆိုင်ရာ route ဆီ ပို့ပေးတာကို မြင်ရမှာဖြစ်ပါတယ်
