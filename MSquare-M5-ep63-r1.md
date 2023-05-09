## MSquare Programing Fullstack Course
### Episode-*63* 
### Summary For `Room(1)` intermediate Class
## Location and Menu items
- database မှာ menus table နဲ့ locations table ထဲက data တွေ ချိတ်ဆက်ပါမယ်

![enter image description here](https://github.com/Aungmyanmar32/msquare-m5-summaries/blob/main/ep63r1.png?raw=true)
- server ဆီ request လုပ်တဲ့အခါမှာ location table နဲ့ menus_location table ထဲက  data တွေကိုလဲ လှမ်းယူထားလိုက်ပါမယ်။
### Server.ts
![enter image description here](https://github.com/Aungmyanmar32/msquare-m5-summaries/blob/main/ep63r11%20%281%29.png?raw=true)

- ရလာတဲ့ data ကို context မှာ သိမ်းမှာမလို့ context နဲ့ typing မှာ locations အတွက် ထည့်ပေးပါမယ်။
```js
//types.ts

interface BaseTypes {
  name: string;
  id?: number;
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

export interface Location extends BaseTypes {
  address?: string;
}

export interface MenusLocation {
  id: number;
  menu_id: number;
  location_id: number;
  is_available: boolean;
}

```

```js
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
  menusLocations : MenusLocation[]
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

    const { menus, menuCategories, addons, addonCategories, locations } =
      responseJson;

    updateData({
      ...data,
      menus,
      menuCategories,
      addons,
      addonCategories,
      locations,
      menusLocations
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
##

### Use location id from url query
- URl ထဲ ကနေ ၀င်လာမယ့် query ကို သုံးပြီး location ID ကိုရယူပြီး menu location table ထဲက data တွေနဲ့ တိုက်စစ်ပီး location ရှိမရှိ အရင်စစ်ပါမယ်

![enter image description here](https://github.com/Aungmyanmar32/msquare-m5-summaries/blob/main/ep63r11%20%285%29.png?raw=true)
- ဆက်ပြီး location ရှိခဲ့ရင် သက်ဆိုင်တဲ့ menu ကိုပဲ ပြထားလို့ရအောင် လုပ်ပါမယ်


![enter image description here](https://github.com/Aungmyanmar32/msquare-m5-summaries/blob/main/ep63r11%20%286%29.png?raw=true)
 - location က စမ်းချောင်းဖြစ်ခဲ့ရင်  စမ်းချောင်းမှာပဲ မှာလို့ရမယ့် menu item တွေကို ရွေးပြချင်တာ ဖြစ်ပါတယ်။
![enter image description here](https://github.com/Aungmyanmar32/msquare-m5-summaries/blob/main/ep63r11%20%287%29.png?raw=true)
- app မှာ /meuns?locationId=1 ဆိုပြီး ၀င်စမ်းကြည့်ပါက menu item နှစ်ခုရှိသော်လည်း location_id 1 မှာပဲ ရှိတဲ့ mont him khar တစ်ခုပဲ ပြပေးတာကိုတွေ့ရမှာပါ

![enter image description here](https://github.com/Aungmyanmar32/msquare-m5-summaries/blob/main/ep63r11%20%288%29.png?raw=true)

##
### Default location ( set local storage)
- setting component တစ်ခုလုပ်ပြီး router ထဲမှာလဲ link ချိတ်ထားလိုက်ပါမယ်
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
import { MenuDetail } from "./components/MenuDetail";
import Setting from "./components/setting";

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
    path: "/menus/:menuId",
    element: <MenuDetail />,
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
  {
    path: "/setting",
    element: <Setting/>,
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
```
- setting component  မှာ mui ရဲ့ selector component ကို သုံးပြီး context ထဲရှိ location အကုန်လုံးကို ရွေးလို့ရအောင် လုပ်ပါမယ်။
- local storage ကို သုံးပြီး default location တစ်ခုလဲ ထည့်ပေးထားပါမယ်။
```js
//setting.tsx

import React, { useContext, useEffect, useState } from "react";
import Layout from "./Layout";
import {
  SelectChangeEvent,
  FormControl,
  InputLabel,
  Select,
  MenuItem,
  Box,
} from "@mui/material";
import { AppContext } from "../contexts/AppContext";
import { Location } from "../types/types";

const Setting = () => {
  const contextData = useContext(AppContext);
  const { locations } = contextData;
  const [selectedLocation, setSelectedLocation] = useState<
    Location | undefined
  >();

  useEffect(() => {
    if (locations.length) {
      const selectedLocation = localStorage.getItem("selectedLocation");
      if (!selectedLocation) {
        localStorage.setItem("selectedLocation", String(locations[0].id));
        setSelectedLocation(locations[0]);
      }
    }
  }, [locations]);

  const handleChange = (event: SelectChangeEvent<number>) => {};
  return (
    <Layout>
      <Box sx={{ maxWidth: "400px", m: "0 auto", mt: 2 }}>
        <FormControl fullWidth>
          <InputLabel id="demo-simple-select-label">Location</InputLabel>
          <Select
            labelId="demo-simple-select-label"
            id="demo-simple-select"
            value={selectedLocation ? selectedLocation.id : ""}
            label="Locations"
            onChange={handleChange}
          >
            {locations.map((location) => {
              return (
                <MenuItem value={location.id} key={location.id}>
                  {location.name}
                </MenuItem>
              );
            })}
          </Select>
        </FormControl>
      </Box>
    </Layout>
  );
};

export default Setting;

```
![enter image description here](https://github.com/Aungmyanmar32/msquare-m5-summaries/blob/main/Screenshot%202023-05-09%20153859.png?raw=true)
- ဆက်ပြီး location တစ်ခုခု ရွေးပေးလိုက်တဲ့အခါ handleOnChange function တစ်ခု define လုပ်ပြီး ခေါ်သုံးလိုက်ပါမယ်။

```js

import React, { useContext, useEffect, useState } from "react";
import Layout from "./Layout";
import {
  SelectChangeEvent,
  FormControl,
  InputLabel,
  Select,
  MenuItem,
  Box,
} from "@mui/material";
import { AppContext } from "../contexts/AppContext";
import { Location } from "../types/types";

const Setting = () => {
  const contextData = useContext(AppContext);
  const { locations } = contextData;
  const [selectedLocation, setSelectedLocation] = useState<
    Location | undefined
  >();

  useEffect(() => {
    if (locations.length) {
      const selectedLocationID = localStorage.getItem("selectedLocation");
      if (!selectedLocationID) {
        localStorage.setItem("selectedLocation", String(locations[0].id));
        setSelectedLocation(locations[0]);
      } else {
        const selectLocation = locations.find(
          (location) => String(location.id) === selectedLocationID
        );
        setSelectedLocation(selectLocation);
      }
    }
  }, [locations]);

//update selected location 
  const handleChange = (evt: SelectChangeEvent<number>) => {
    const selectedLocation =
      locations &&
      locations.find((location) => location.id === evt.target.value);
    localStorage.setItem("selectedLocation", String(selectedLocation?.id));
    setSelectedLocation(selectedLocation);
  };
  return (
    <Layout>
      <Box sx={{ maxWidth: "400px", m: "0 auto", mt: 2 }}>
        <FormControl fullWidth>
          <InputLabel id="demo-simple-select-label">Location</InputLabel>
          <Select
            labelId="demo-simple-select-label"
            id="demo-simple-select"
            value={selectedLocation ? selectedLocation.id : ""}
            label="Locations"
            onChange={handleChange}
          >
            {locations.map((location) => {
              return (
                <MenuItem value={location.id} key={location.id}>
                  {location.name}
                </MenuItem>
              );
            })}
          </Select>
        </FormControl>
      </Box>
    </Layout>
  );
};

export default Setting;

```
- ခု app ကို location တွေ ရွေးစမ်းကြည့်တဲ့အခါ update ဖြစ်တဲ့ location ကို local storage မှာ သိမ်းပြီး ပြပေးတာကို မြင်ရမှာဖြစ်ပါတယ်။

![enter image description here](https://github.com/Aungmyanmar32/msquare-m5-summaries/blob/main/Screenshot%202023-05-09%20154118.png?raw=true)


