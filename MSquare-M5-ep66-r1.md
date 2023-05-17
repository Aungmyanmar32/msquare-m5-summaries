## MSquare Programing Fullstack Course
### Episode-*66* 
### Summary For `Room(1)` intermediate Class
## Protected Routes
### log in ၀င်ထားတဲ့ user ဖြစ်မှသာ menu, setting အစရှိတဲ့ route တွေ ၀င်လို့ရအောင် protect လုပ်မှာဖြစ်ပါတယ်။
- frontend ထဲက src အောက်မှာ Routes folder တစ်ခုလုပ်ပါမယ်။
- Routes folder ထဲမှာမှ  Router.tsx နဲ့ PrivateRoute component နှစ်ခု လုပ်ပါမယ်။
```js
// Router.tsx

import { BrowserRouter, Route, Routes } from "react-router-dom";
import PrivateRoute from "./PrivateRoute";
import App from "../App";
import Menus from "../components/Menus";
import { MenuDetail } from "../components/MenuDetail";
import MenuCategories from "../components/MenuCategories";
import Addons from "../components/Addons";
import AddonCategories from "../components/AddonCategories";
import Setting from "../components/Setting";
import Login from "../components/Login";
import Register from "../components/Register";

const Router = () => {
  return (
    <BrowserRouter>
      <Routes>
        <Route element={<PrivateRoute />}>
          <Route path="/" Component={App} />
          <Route path="/menus" Component={Menus} />
          <Route path="/menus/:menuId" Component={MenuDetail} />
          <Route path="/menu-categories" Component={MenuCategories} />
          <Route path="/addons" Component={Addons} />
          <Route path="/addon-categories" Component={AddonCategories} />
          <Route path="/settings" Component={Setting} />
        </Route>
        <Route path="/login" Component={Login} />
        <Route path="register" Component={Register} />
      </Routes>
    </BrowserRouter>
  );
};

export default Router;

```
- **`/login`** **`/register`** route နှစ်ခုက လွဲ ပြီး ကျန်တဲ့ route တွေကို **PrivateRoute** အောက်မှာ ထည့်ပေးထားပါတယ်။
```js
//PrivateRoute.tsx

import { useContext } from "react";
import { AppContext } from "../contexts/AppContext";
import { Navigate, Outlet } from "react-router-dom";

const PrivateRoute = () => {
  const { accessToken } = useContext(AppContext);
  return accessToken ? <Outlet /> : <Navigate to={"/login"} />;
};

export default PrivateRoute;

```
- PrivateRoute.tsx ထဲမှာတော့ accessToken ရှိမရှိ စစ်လိုက်ပြီး ရှိခဲ့ရင်တော့ သူ့အောက်မှာ ထည့်ပေးထားတဲ့ route တွေဆီ react-router-dom က Outlet component ကို သုံးပြီး သွားခွင့်ပြုလိုက်ပြီး  မရှိခဲ့ရင်တော့ log in ၀င်ခိုင်းလိုက်တာဖြစ်ပါတယ်။
- index.tsx မှာလည်း route တွေ create လုပ်စရာမလိုတော့ပဲ Router component ကိုပဲ render လုပ်ပေးလိုက်ရင် ရပါပြီး

```js
//index.tsx

import ReactDOM from "react-dom/client";
import "./index.css";
import AppProvider from "./contexts/AppContext";
import Router from "./Routes/Router";

const root = ReactDOM.createRoot(
  document.getElementById("root") as HTMLElement
);
root.render(
  <>
    <AppProvider>
      <Router />
    </AppProvider>
  </>
);
```
- ခု happy pos app ကို စမ်းကြည့်ပါက ပထမအကြိမ် ဘယ် route ကိုပဲ သွားသွား login ၀င်ခိုင်းမှာ ဖြစ်ပြီး login ၀င်ပြီးလို့ accessToken ရလာမှသာ ကျန်တဲ့ route တွေဆီ သွားလို့ရမှာ ဖြစ်ပါတယ်။
##
- ဆက်ပြီးတော့ user တစ်ယောက် log in ၀င်လိုက်ပြီး ဆိုရင် Nav bar ရဲ့  ညာဘက်က Log in ဆိုတာကို Log out ဆိုပြီး ပြပေးမှာဖြစ်ပါတယ်
- အဲ့ဒီ Log out ကို နှိပ်လိုက်ရင်လဲ Logout component တစ်ခု ပြပေးမှာဖြစ်ပါတယ်။
-  Logout component တစ်ခု လုပ်ပြီး router မှာလည်း /logout route တစ်ခု ထပ်ထည့်ပေးလိုက်ပါမယ်။
```js
//Logout.tsx

import { useContext, useEffect } from "react";
import Layout from "./Layout";
import { AppContext, defaultAppContext } from "../contexts/AppContext";

const Logout = () => {
  const { updateData } = useContext(AppContext);

  useEffect(() => {
    updateData(defaultAppContext);
  }, []);

  return (
    <Layout>
      <h1> You are Logged Out...</h1>
    </Layout>
  );
};

export default Logout;


```
```js
// Router.tsx

import { BrowserRouter, Route, Routes } from "react-router-dom";
import PrivateRoute from "./PrivateRoute";
import App from "../App";
import Menus from "../components/Menus";
import { MenuDetail } from "../components/MenuDetail";
import MenuCategories from "../components/MenuCategories";
import Addons from "../components/Addons";
import AddonCategories from "../components/AddonCategories";
import Setting from "../components/Setting";
import Login from "../components/Login";
import Register from "../components/Register";
import Logout from "../components/Logout";

const Router = () => {
  return (
    <BrowserRouter>
      <Routes>
        <Route element={<PrivateRoute />}>
          <Route path="/" Component={App} />
          <Route path="/menus" Component={Menus} />
          <Route path="/menus/:menuId" Component={MenuDetail} />
          <Route path="/menu-categories" Component={MenuCategories} />
          <Route path="/addons" Component={Addons} />
          <Route path="/addon-categories" Component={AddonCategories} />
          <Route path="/settings" Component={Setting} />
        </Route>
        <Route path="/login" Component={Login} />
        <Route path="/logout" Component={Logout} />
        <Route path="register" Component={Register} />
      </Routes>
    </BrowserRouter>
  );
};

export default Router;

```
- ဆက်ပြီးတော့ NavBar component မှာလည်း useContext နဲ့ accesstoken ကို လှမ်းယူလိုက်ပြီး Log in / Log out စာကို ပြောင်းပြီး ပြပေးမှာဖြစ်ပါတယ်။

![enter image description here](https://github.com/Aungmyanmar32/msquare-m5-summaries/blob/main/66-r1.png?raw=true)

- ခု happy pos app ကို စမ်းကြည့်ပါက log in/out စာလေး ပြောင်းပြီး ပြပေးတာကို မြင်ရမှာဖြစ်ပါတယ်။

![enter image description here](https://github.com/Aungmyanmar32/msquare-m5-summaries/blob/main/66-r1-2.png?raw=true)
##
### User  Login လုပ်တဲ့အခါ log in button ကို click စရာမလိုပဲ နဲ့ enter key ကို နှိပ်လိုက်ချိန်မှာလည်း log in ၀င်လို့ရအောင် လုပ်ပေးလိုက်ခြင်းအားဖြင့် user experience ကို ပိုကောင်းစေပါတယ်

![enter image description here](https://github.com/Aungmyanmar32/msquare-m5-summaries/blob/main/Screenshot%202023-05-17%20101913.png?raw=true)
##
### ဆက်ပြီးတော့ `/orders`  route တစ်ခုလုပ်ပြီး orders တွေ ပြပေးနိုင်အောင် လုပ်ပါမယ်။
- လောလောဆယ်မှာတော့ dummy data တစ်ခုကိုပဲ mui table ကို သုံးပြီး order အနေနဲ့ ပြထားမှာဖြစ်ပါတယ်။
- components folder အောက်မှာ Orders component တစ်ခု လုပ်ပါမယ်။
```js
//Orders.tsx

import Table from "@mui/material/Table";
import TableBody from "@mui/material/TableBody";
import TableCell from "@mui/material/TableCell";
import TableContainer from "@mui/material/TableContainer";
import TableHead from "@mui/material/TableHead";
import TableRow from "@mui/material/TableRow";
import Paper from "@mui/material/Paper";
import Layout from "./Layout";

function createData(
  name: string,
  calories: number,
  fat: number,
  carbs: number,
  protein: number
) {
  return { name, calories, fat, carbs, protein };
}

const rows = [
  createData("Frozen yoghurt", 159, 6.0, 24, 4.0),
  createData("Ice cream sandwich", 237, 9.0, 37, 4.3),
  createData("Eclair", 262, 16.0, 24, 6.0),
  createData("Cupcake", 305, 3.7, 67, 4.3),
  createData("Gingerbread", 356, 16.0, 49, 3.9),
];

export default function Orders() {
  return (
    <Layout>
      <TableContainer
        component={Paper}
        sx={{ maxWidth: "500px", m: "0 auto", mt: 5 }}
      >
        <Table aria-label="simple table">
          <TableHead>
            <TableRow>
              <TableCell>Dessert (100g serving)</TableCell>
              <TableCell align="right">Calories</TableCell>
              <TableCell align="right">Fat&nbsp;(g)</TableCell>
              <TableCell align="right">Carbs&nbsp;(g)</TableCell>
              <TableCell align="right">Protein&nbsp;(g)</TableCell>
            </TableRow>
          </TableHead>
          <TableBody>
            {rows.map((row) => (
              <TableRow
                key={row.name}
                sx={{ "&:last-child td, &:last-child th": { border: 0 } }}
              >
                <TableCell component="th" scope="row">
                  {row.name}
                </TableCell>
                <TableCell align="right">{row.calories}</TableCell>
                <TableCell align="right">{row.fat}</TableCell>
                <TableCell align="right">{row.carbs}</TableCell>
                <TableCell align="right">{row.protein}</TableCell>
              </TableRow>
            ))}
          </TableBody>
        </Table>
      </TableContainer>
    </Layout>
  );
}

```
- အဲ့ဒီ order အတွက်  Router component မှာ route တစ်ခုနဲ့  NavBar drawer မှာ List item တစ်ခု လုပ်ပေးပါမယ်။

```js
// NavBar.tsx -- sidebarMenuItems array

const sidebarMenuItems = [
  { id: 1, label: "Orders", icon: <LunchDiningIcon />, route: "/orders" },
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
  { id: 6, label: "Settings", icon: <SettingsIcon />, route: "/settings" },
];

```

```js
// NavBar.tsx -- drawerContent function

 const drawerContent = () => (
    <Box
      sx={{ width: 250 }}
      role="presentation"
      onClick={toggleDrawer(false)}
      onKeyDown={toggleDrawer(false)}
    >
      <List>
        {sidebarMenuItems.slice(0, 5).map((item) => (
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

```
```js
// Router.tsx

import { BrowserRouter, Route, Routes } from "react-router-dom";
import PrivateRoute from "./PrivateRoute";
import App from "../App";
import Menus from "../components/Menus";
import { MenuDetail } from "../components/MenuDetail";
import MenuCategories from "../components/MenuCategories";
import Addons from "../components/Addons";
import AddonCategories from "../components/AddonCategories";
import Setting from "../components/Setting";
import Login from "../components/Login";
import Register from "../components/Register";
import Logout from "../components/Logout";
import Orders from "../components/Orders";

const Router = () => {
  return (
    <BrowserRouter>
      <Routes>
        <Route element={<PrivateRoute />}>
          <Route path="/" Component={App} />
          <Route path="/orders" Component={Orders} />
          <Route path="/menus" Component={Menus} />
          <Route path="/menus/:menuId" Component={MenuDetail} />
          <Route path="/menu-categories" Component={MenuCategories} />
          <Route path="/addons" Component={Addons} />
          <Route path="/addon-categories" Component={AddonCategories} />
          <Route path="/settings" Component={Setting} />
        </Route>
        <Route path="/login" Component={Login} />
        <Route path="/logout" Component={Logout} />
        <Route path="register" Component={Register} />
      </Routes>
    </BrowserRouter>
  );
};

export default Router;

```
ခုဆို NavBar component က drawer မှာ Orders item တစ်ခု တိုးလာမှာဖြစ်ပြီး နှိပ်ကြည့်လိုက်ရင် Table တစ်ခုကို မြင်ရမှာ ဖြစ်ပါတယ်။

![enter image description here](https://github.com/Aungmyanmar32/msquare-m5-summaries/blob/main/Screenshot%202023-05-17%20111136.png?raw=true)

##
### ဆက်ပြီးတော့ NavBar မှာ ရွေးထားတဲ့ location နဲ့ လက်ရှိရောက်နေတဲ့ Page  ကို ပြပေးပါမယ်။

![enter image description here](https://github.com/Aungmyanmar32/msquare-m5-summaries/blob/main/Screenshot%202023-05-17%20114245.png?raw=true)
- context ထဲက location မှာ localstorage မှာ သိမ်းထားတဲ့ location id ရှိမရှိ ရှာလိုက်ပြီး တွေ့ခဲ့ရင် location name ကို ပြပေးလိုက်တာဖြစ်ပါတယ်။
- App မှာ အောက်က ပုံလိုလေး ပြပေးနေမှာဖြစ်ပါတယ်

![enter image description here](https://github.com/Aungmyanmar32/msquare-m5-summaries/blob/main/Screenshot%202023-05-17%20114318.png?raw=true)
