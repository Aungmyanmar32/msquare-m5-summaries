## MSquare Programing Fullstack Course
### Episode-*79* 
### Summary For `Room(1)` 
## Location and Company 
### Setting component မှာ login ၀င်ထားတဲ့ user ရဲ့ company နဲ့ location ကို ပြပေးပြီး Update လုပ်လို့ရအောင်  လုပ်ကြပါမယ်။
- အရင်ဆုံး Company ကို ပြပေးလိုက်ပါမယ်
```js
import { useContext } from "react";
import Layout from "./Layout";
import { AppContext } from "../contexts/AppContext";
import {
  Box,
  TextField,
} from "@mui/material";
import { useState } from "react";
import { useEffect } from "react";

const Settings = () => {
  const { locations, fetchData, company } = useContext(AppContext);
 
  return (
    <Layout title="Settings">
      <Box sx={{ p: 3, width: "300px" }}>
        
        <TextField label="Company Name" defaultValue={company?.name} />
        
      </Box>
    </Layout>
  );
};

export default Settings;

```

- context ထဲက locations , company , fetchData ကို လှမ်းယူထားပြီး company ကို mui text field နဲ့ ပြပေးထားတာဖြစ်ပါတယ်။

![enter image description here](https://cdn.discordapp.com/attachments/1076154921562411008/1114457725766742036/image.png)

### ဆက်ပြီး အဲ့ဒီ company နဲ့ ချိတ်ဆက်ထားတဲ့ location တွေကို ရွေးလို့ရအောင် လုပ်ပါမယ်။

```js
import { useContext } from "react";
import Layout from "./Layout";
import { AppContext } from "../contexts/AppContext";
import {
  Box,
  FormControl,
  InputLabel,
  MenuItem,
  Select,
  SelectChangeEvent,
  TextField,
} from "@mui/material";
import { useState } from "react";
import { useEffect } from "react";

const Settings = () => {
  const { locations, fetchData, company } = useContext(AppContext);
  const [selectedLocationId, setSelectedLocationId] = useState<string>("");

  useEffect(() => {
    if (locations.length) {
      const locationIdFromLocalStorage =
        localStorage.getItem("selectedLocationId");
      if (locationIdFromLocalStorage) {
        setSelectedLocationId(locationIdFromLocalStorage);
      } else {
        const firstLocationId = String(locations[0].id);
        setSelectedLocationId(firstLocationId);
        localStorage.setItem("selectedLocationId", firstLocationId);
      }
    }
  }, [locations]);

  const handleChange = (event: SelectChangeEvent) => {
    const locationId = event.target.value;
    setSelectedLocationId(locationId);
    localStorage.setItem("selectedLocationId", event.target.value);
  };

  return (
    <Layout title="Settings">
      <Box sx={{ p: 3, width: "300px" }}>
        <TextField label="Company Name" defaultValue={company?.name} />
        <Box sx={{ mt: 3 }}>
          <FormControl fullWidth>
            <InputLabel>Locations</InputLabel>
            <Select
              value={selectedLocationId}
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
      </Box>
    </Layout>
  );
};

export default Settings;

```

- ရှင်းလင်းချက်
```js
  const [selectedLocationId, setSelectedLocationId] = useState<string>("");
```
- ရွေးထားတဲ့ location id ကို သိမ်းဖို့ state တစ်ခု လုပ်ထားလိုက်ပါတယ်
```js
useEffect(() => {
    if (locations.length) {
      const locationIdFromLocalStorage =
        localStorage.getItem("selectedLocationId");
      if (locationIdFromLocalStorage) {
        setSelectedLocationId(locationIdFromLocalStorage);
      } else {
        const firstLocationId = String(locations[0].id);
        setSelectedLocationId(firstLocationId);
        localStorage.setItem("selectedLocationId", firstLocationId);
      }
    }
  }, [locations]);
```
- Context ထဲက locations array မှာ item တွေ ရှိမရှိ စစ်လိုက်ပြီး
- ရှိခဲ့ရင် localStroage ထဲက locationId ကို လှမ်းယူလိုက်ပါတယ်
- locationId က ရှိခဲ့တယ်ဆိုရင် selectedLocationId ရဲ့ တန်ဖိုးအဖြစ် update လုပ်လိုက်တာဖြစ်ပြီး
- locationId က မရှိခဲ့ဘူးဆိုရင်တော့ locations array ထဲက ပထမ item ရဲ့ id ကို  selectedLocationId ရဲ့ တန်ဖိုးအဖြစ် update လုပ်လိုက်တာဖြစ်ပါတယ်
- အဲ့တာတွေကို useEffect ကိုသုံးပြီး location တွေ ပြောင်းတိုင်း render ပြန်လုပ်ထားလိုက်ပါတယ်
```js
const handleChange = (event: SelectChangeEvent) => {
    const locationId = event.target.value;
    setSelectedLocationId(locationId);
    localStorage.setItem("selectedLocationId", event.target.value);
  };
```
- MUI selector တစ်ခု ကို  အောက်မှာလုပ်ထားပြီး အဲ့ဒီမှာ တစ်ခုခု ပြောင်းလိုက်တိုင်း run မယ့် function ကို သတ်မှတ်ထားပါတယ်
- ရွေးလိုက်တဲ့ location id ကို ယူလိုက်ပြီး  selectedLocationId ရဲ့ တန်ဖိုးအဖြစ် update လုပ်လိုက်ကာ localStorage မှာလည်း သိမ်းပေးထားပါတယ်
```js
<FormControl fullWidth>
            <InputLabel>Locations</InputLabel>
            <Select
              value={selectedLocationId}
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
```
-  MUI Selector တစ်ခု ကို ပြပေးလိုက်တာဖြစ်ပါတယ်
- အဲ့ဒီအထဲမှာ location ထဲက name တွေကို select လုပ်လို့ရအောင် လုပ်ထားပြီး ပြောင်းလဲမှု ရှိတိုင်း အထက်ပါ function ကို run ခိုင်းထားတာဖြစ်ပါတယ်
- အခု Foodie App က setting မှာ ၀င်ကြည့်ပါက location တွေရွေးလို့ရမယ့်  selector ကို မြင်ရမှာဖြစ်ပါတယ်

![enter image description here](https://cdn.discordapp.com/attachments/1076154921562411008/1114771092020662362/image.png)
##
### Location
- ဆက်ပြီး context ထဲရှိ location arrray ကို သုံးပြီး location တွေကို ပြကြည့်ပါမယ်

```js
// Locations.tsx

import { useContext, useState } from "react";
import Layout from "./Layout";
import { AppContext } from "../contexts/AppContext";
import { Box, Button, TextField, Typography } from "@mui/material";
import { config } from "../config/config";

const Locations = () => {
  const { locations} = useContext(AppContext);


  return (
    <Layout title="Locations">
      <Box sx={{ ml: 3, mt: 5 }}>
        {locations.map((location, index) => {
          return (
            <Box sx={{ display: "flex", alignItems: "center", mb: 3 }}>
              <Typography variant="h5" sx={{ mr: 3 }}>
                {index + 1}.
              </Typography>
              <TextField defaultValue={location.name} sx={{ mr: 3 }} />
              <TextField defaultValue={location.address} sx={{ mr: 3 }} />
              <Button variant="contained">Update</Button>
            </Box>
          );
        })}
       
      </Box>
    </Layout>
  );
};

export default Locations;

```
- context ထဲက location array ကို လှမ်းယူလိုက်ပြီး loop လုပ်ကာ mui text field နဲ့ ပြပေးထားတာဖြစ်ပါတယ် ။ နောက်ပိုင်းကျရင် Update လုပ်လို့ရအောင်လဲ Update button လေး ကြိုထည့်ပေးထားပါတယ်

![enter image description here](https://cdn.discordapp.com/attachments/1076154921562411008/1112235436958568478/image.png)

- ဆက်ပြီး location အသစ်တွေ ထပ်လုပ်လို့ရအောင် လုပ်ပါမယ်

```js
// locations.tsx
import { useContext, useState } from "react";
import Layout from "./Layout";
import { AppContext } from "../contexts/AppContext";
import { Box, Button, TextField, Typography } from "@mui/material";
import { config } from "../config/config";

const Locations = () => {
  const { locations, fetchData, company } = useContext(AppContext);
  const [newLocation, setNewLocation] = useState({
    name: "",
    address: "",
    companyId: company?.id,
  });
  const accessToken = localStorage.getItem("accessToken");

  const createNewLocation = async () => {
    await fetch(`${config.apiBaseUrl}/locations`, {
      method: "POST",
      headers: {
        Authorization: `Bearer ${accessToken}`,
        "Content-Type": "application/json",
      },
      body: JSON.stringify(newLocation),
    });
    fetchData();
    setNewLocation({ name: "", address: "", companyId: company?.id });
  };

  return (
    <Layout title="Locations">
      <Box sx={{ ml: 3, mt: 5 }}>
        {locations.map((location, index) => {
          return (
            <Box sx={{ display: "flex", alignItems: "center", mb: 3 }}>
              <Typography variant="h5" sx={{ mr: 3 }}>
                {index + 1}.
              </Typography>
              <TextField defaultValue={location.name} sx={{ mr: 3 }} />
              <TextField defaultValue={location.address} sx={{ mr: 3 }} />
              <Button variant="contained">Update</Button>
            </Box>
          );
        })}
        <h1>Create New Location</h1>
        <Box sx={{ ml: 5, display: "flex", alignItems: "center" }}>
          <TextField
            value={newLocation.name}
            sx={{ mr: 3 }}
            onChange={(evt) =>
              setNewLocation({ ...newLocation, name: evt.target.value })
            }
          />
          <TextField
            value={newLocation.address}
            sx={{ mr: 3 }}
            onChange={(evt) =>
              setNewLocation({ ...newLocation, address: evt.target.value })
            }
          />
          <Button variant="contained" onClick={createNewLocation}>
            Create
          </Button>
        </Box>
      </Box>
    </Layout>
  );
};

export default Locations;

```
- ရှင်းလင်းချက်
```js
 const { locations, fetchData, company } = useContext(AppContext);
  const [newLocation, setNewLocation] = useState({
    name: "",
    address: "",
    companyId: company?.id,
  });
  const accessToken = localStorage.getItem("accessToken");
```
- context ထဲက fetchData နဲ့ company  ကို ထပ်ယူလိုက်ပါတယ်
- newLocation ဆိုတဲ့ state တစ်ခု သတ်မှတ်လိုက်ပြီး မူလတန်ဖိုးကို database မှာ ရှိတဲ့ locations table ထဲက ပုံစံအတိုင်း သတ်မှတ်ပေးထားလိုက်ပါတယ်
- accessToken ကိုလည်း localStorage ကနေ လှမ်းယူထားပါတယ်

```js
const createNewLocation = async () => {
    await fetch(`${config.apiBaseUrl}/locations`, {
      method: "POST",
      headers: {
        Authorization: `Bearer ${accessToken}`,
        "Content-Type": "application/json",
      },
      body: JSON.stringify(newLocation),
    });
    fetchData();
    setNewLocation({ name: "", address: "", companyId: company?.id });
  };
```
- create button ကို နှိပ်လိုက်တဲ့ အချိန်မှာ run မယ့် createNewLocation  function ပါ
- ဆာဗာဆီ locations route နဲ့ request လုပ်ထားပြီး newLocation object ( state) ကို request body အနေနဲ့ ထည့်ပေးလိုက်တာဖြစ်ပါတယ်
- ပြီးရင် context ထဲက fetchData function ကို သုံးပြီး sever ဆီက data တွေအားလုံးကို request ထပ်လုပ်လိုက်ထညးပါတယ်
- နောက်ဆုံးမှာတော့ newLocation state ရဲ့ တန်ဖိုးကို နဂို အနေအထားအတိုင်း ပြန်သတ်မှတ်ပေးလိုက်တာဖြစ်ပါတယ်
```js
<h1>Create New Location</h1>
        <Box sx={{ ml: 5, display: "flex", alignItems: "center" }}>
          <TextField
            value={newLocation.name}
            sx={{ mr: 3 }}
            onChange={(evt) =>
              setNewLocation({ ...newLocation, name: evt.target.value })
            }
          />
          <TextField
            value={newLocation.address}
            sx={{ mr: 3 }}
            onChange={(evt) =>
              setNewLocation({ ...newLocation, address: evt.target.value })
            }
          />
          <Button variant="contained" onClick={createNewLocation}>
            Create
          </Button>
        </Box>
```
- Mui text input နှစ်ခု နဲ့  Create Button တစ်ခု ထပ်ထည့်ထားပါတယ်
- TextField ထဲမှာ တစ်ခုခု ထည့်လိုက်တိုင်း newLocation state ကို update လုပ်ပေးထားပါတယ်
- Create Button ကို နှိပ်လိုက်ချိန်မှာတော့ အထက်မှာ သတ်မှတ်ထားတဲ့ CreateNewLocation function ကို အလုပ်လုပ်ခိုင်းလိုက်ပါတယ်
- backend serverမှာ /location route နဲ့ ၀င်လာမယ့် request ကို လက်ခံပြီး location အသစ်ကို database မှာ သိမ်းလိုက်မှာ ဖြစ်ပါတယ်။
- backend မှာ locationRouter.ts ဖိုင် အသစ်တစ်ခု ကို src/router folder ထဲမှာ လုပ်ပါမယ်

```js
// backend/src/router/locationRouter.ts

import express, { Request, Response } from "express";
import { checkAuth } from "../utils/auth";
import { db } from "../db/db";
const locationsRouter = express.Router();



locationsRouter.post("/", checkAuth, async (req: Request, res: Response) => {
  const { name, address, companyId } = req.body;
  const isValid = name && address && companyId;
  if (!isValid) return res.send(400);
  await db.query(
    "insert into locations (name, address, companies_id) values($1, $2, $3)",
    [name, address, companyId]
  );
  res.send(200);
});

export default locationsRouter;
```
- name / address / companyId ပါမပါ စစ်လိုက်ပြီး 
- မပါခဲ့ရင် 400 ( bad request) ကို response လုပ်လိုက်ကာ
- ပါလာတယ်ဆိုရင် database မှာ သိမ်းလိုက်တာဖြစ်ပါတယ်
- ခု locationRouter ကို server.ts မှာ ခေါ်သုံးပေးပါမယ်

```js
import dotenv from "dotenv";
dotenv.config();
import express, { Request, Response } from "express";
import cors from "cors";
import menuRouter from "./src/routers/menuRouter";
import authRouter from "./src/routers/authRouter";
import appRouter from "./src/routers/appRouter";
import locationRouter from "./src/routers/locationRouter";

const app = express();
app.use(cors());
app.use(express.json());

const port = 5000;

app.use("/menus", menuRouter);
app.use("/auth", authRouter);
app.use("/", appRouter);
app.use("/locations", locationRouter);

app.listen(port, () => {
  console.log("Server has started on port:", port);
});

```
- Foddie POS app မှာ location အသစ်တစ်ခု လုပ်ကြည့်ပါက Location ပြတဲ့နေရာမှာ ချက်ချင်းပေါ်လာမှာဖြစ်ပြီး database မှာလည်း သိမ်းထားပေးတာကို မြင်ရမှာဖြစ်ေပါတယ်

![enter image description here](https://cdn.discordapp.com/attachments/1076154921562411008/1112242245836480663/image.png)

##
### *UPDATE* and *DELETE* Location
- ရှိပြီးသား location တွေကို CURD လုပ် နိုင်ဖို့ ဆက်လုပ်ပါမယ်။
```js
// Locaions.tsx

import { useContext, useEffect, useState } from "react";
import Layout from "./Layout";
import { AppContext } from "../contexts/AppContext";
import { Box, Button, TextField, Typography } from "@mui/material";
import { Location } from "../typings/types";

const Locations = () => {
  const { locations, company, fetchData } = useContext(AppContext);
  const accessToken = localStorage.getItem("accessToken");
  const [newLocation, setNewLocation] = useState<Location>({
    name: "",
    address: "",
  });
  const [updatedLocations, setUpdateLocations] =
    useState<Location[]>(locations);

  useEffect(() => {
    setUpdateLocations(locations);
  }, [locations]);

  const updateLocation = async (location: Location) => {
    const locationId = location.id;
    const oldLocation = locations.find((loc) => loc.id === locationId);
    const newLocation = updatedLocations.find(
      (updateLocation) => updateLocation.id === locationId
    );
    if (
      oldLocation?.name !== newLocation?.name ||
      oldLocation?.address !== newLocation?.address
    ) {
      await fetch(`http://localhost:5000/locations/${location.id}`, {
        method: "PUT",
        headers: {
          "Content-Type": "application/json",
          Authorization: `Bearer ${accessToken}`,
        },
        body: JSON.stringify(location),
      });
      fetchData();
    }
  };

  const createLocation = async () => {
    const isValid = newLocation.name && newLocation.address;
    if (!isValid) return alert("Name and address are required.");
    newLocation.companyId = company?.id;
    const response = await fetch("http://localhost:5000/locations", {
      method: "POST",
      headers: {
        "Content-Type": "application/json",
        Authorization: `Bearer ${accessToken}`,
      },
      body: JSON.stringify(newLocation),
    });
    fetchData();
    setNewLocation({ name: "", address: "" });
  };

  const deleteLocation = async (location: Location) => {
    const response = await fetch(
      `http://localhost:5000/locations/${location.id}`,
      {
        method: "DELETE",
        headers: {
          Authorization: `Bearer ${accessToken}`,
        },
      }
    );
    if (response.ok) {
      return fetchData();
    }
    alert(
      "Cannot delete this location. Please delete menus associated with it first."
    );
  };

  return (
    <Layout title="Locations">
      <Box sx={{ px: 2, mt: 5 }}>
        {updatedLocations.map((location, index) => {
          return (
            <Box
              sx={{ display: "flex", alignItems: "center", mb: 3 }}
              key={location.id}
            >
              <Typography variant="h5" sx={{ mr: 1 }}>
                {index + 1}.
              </Typography>
              <TextField
                value={location.name}
                sx={{ mr: 3 }}
                onChange={(evt) => {
                  const newLocations = updatedLocations.map(
                    (updateLocation) => {
                      if (updateLocation.id === location.id) {
                        return { ...updateLocation, name: evt.target.value };
                      }
                      return updateLocation;
                    }
                  );
                  setUpdateLocations(newLocations);
                }}
              />
              <TextField
                value={location.address}
                sx={{ mr: 3, minWidth: 300 }}
                onChange={(evt) => {
                  const newLocations = updatedLocations.map(
                    (updateLocation) => {
                      if (updateLocation.id === location.id) {
                        return { ...updateLocation, address: evt.target.value };
                      }
                      return updateLocation;
                    }
                  );
                  setUpdateLocations(newLocations);
                }}
              />
              
              <Button
                variant="contained"
                onClick={() => updateLocation(location)}
              >
                Update
              </Button>
              
              <Button
                variant="contained"
                color="error"
                sx={{ mr: 2 }}
                onClick={() => deleteLocation(location)}
              >
                Delete
              </Button>
            </Box>
          );
        })}
      </Box>
      <Box sx={{ px: 5.5, display: "flex", alignItems: "center", mb: 3 }}>
        <TextField
          placeholder="Name"
          value={newLocation.name}
          onChange={(evt) =>
            setNewLocation({ ...newLocation, name: evt.target.value })
          }
          sx={{ mr: 3 }}
        />
        <TextField
          placeholder="Address"
          value={newLocation.address}
          onChange={(evt) =>
            setNewLocation({ ...newLocation, address: evt.target.value })
          }
          sx={{ mr: 3, minWidth: 300 }}
        />
        <Button variant="contained" onClick={createLocation}>
          Create
        </Button>
      </Box>
    </Layout>
  );
};

export default Locations;
```
> ရှင်းလင်းချက်

```js
  const [updatedLocations, setUpdateLocations] =
    useState<Location[]>(locations);
```
- updatedLocations State တစ်ခု လုပ်လိုက်ပြီး မူလ တန်ဖိုးအနေနဲ့ context ထဲ locations array ကိုပဲ ပေးထားလိုက်ပါတယ်

```js
  const updateLocation = async (location: Location) => {
    const locationId = location.id;
    const oldLocation = locations.find((loc) => loc.id === locationId);
    const newLocation = updatedLocations.find(
      (updateLocation) => updateLocation.id === locationId
    );
    if (
      oldLocation?.name !== newLocation?.name ||
      oldLocation?.address !== newLocation?.address
    ) {
      await fetch(`http://localhost:5000/locations/${location.id}`, {
        method: "PUT",
        headers: {
          "Content-Type": "application/json",
          Authorization: `Bearer ${accessToken}`,
        },
        body: JSON.stringify(location),
      });
      fetchData();
    }
  };

```
- location တစ်ခုခုက name နဲ့ address ကို ပြောင်းးပြီး update button ကို နှိပ်လိုက်ရင် run ပေးမယ့် function ဖြစ်ပါတယ်
- location ဆိုတဲ့ parameter တစ်ခု လက်ခံထားပါတယ်
```
 const locationId = location.id;
    const oldLocation = locations.find((loc) => loc.id === locationId);
    const newLocation = updatedLocations.find(
      (updateLocation) => updateLocation.id === locationId
    );
```
- location id ကို location parameter ကနေ ယူထားလိုက်ပြီး
- oldLocation နဲ့ newLocation  ကို  အထက်က id နဲ့ ရှာထားလိုက်ပါတယ်
```
   if (
      oldLocation?.name !== newLocation?.name ||
      oldLocation?.address !== newLocation?.address
    ) {
      await fetch(`http://localhost:5000/locations/${location.id}`, {
        method: "PUT",
        headers: {
          "Content-Type": "application/json",
          Authorization: `Bearer ${accessToken}`,
        },
        body: JSON.stringify(location),
      });
      fetchData();
    }
```
- တကယ်လို့ oldLocation ထဲက name နဲ့ address တစ်ခုခုက newLocation ထဲက ဟာတွေနဲ့ မတူခဲ့ရင် server ဆီ fetch လုပ်ပြီး database က locations table မှာ update လုပ်ပေးမှာဖြစ်ပါတယ်
- 

```js
return (
    <Layout title="Locations">
      <Box sx={{ px: 2, mt: 5 }}>
        {updatedLocations.map((location, index) => {
          return (
            <Box
              sx={{ display: "flex", alignItems: "center", mb: 3 }}
              key={location.id}
            >
              <Typography variant="h5" sx={{ mr: 1 }}>
                {index + 1}.
              </Typography>
              <TextField
                value={location.name}
                sx={{ mr: 3 }}
                onChange={(evt) => {
                  const newLocations = updatedLocations.map(
                    (updateLocation) => {
                      if (updateLocation.id === location.id) {
                        return { ...updateLocation, name: evt.target.value };
                      }
                      return updateLocation;
                    }
                  );
                  setUpdateLocations(newLocations);
                }}
              />
              <TextField
                value={location.address}
                sx={{ mr: 3, minWidth: 300 }}
                onChange={(evt) => {
                  const newLocations = updatedLocations.map(
                    (updateLocation) => {
                      if (updateLocation.id === location.id) {
                        return { ...updateLocation, address: evt.target.value };
                      }
                      return updateLocation;
                    }
                  );
                  setUpdateLocations(newLocations);
                }}
              />
              
              <Button
                variant="contained"
                onClick={() => updateLocation(location)}
              >
                Update
              </Button>
```
- return လုပ်တဲ့အခါ အစောပိုင်းကလို context ထဲက locations array ကို map မလုပ်တော့ပဲ updateLocations state ရဲ့ တန်ဖိုးကိုပဲ map လုပ်ပြီး ပြပေးထားပါတယ်

```
  <TextField
                value={location.name}
                sx={{ mr: 3 }}
                onChange={(evt) => {
                  const newLocations = updatedLocations.map(
                    (updateLocation) => {
                      if (updateLocation.id === location.id) {
                        return { ...updateLocation, name: evt.target.value };
                      }
                      return updateLocation;
                    }
                  );
                  setUpdateLocations(newLocations);
                }}
              />
```
- location name ကို ပြတဲ့နေရာမှာ တစ်ခုခု ပြင်လိုက်တာနဲ့ လက်ရှိloop လုပ်နေတဲ့ location id နဲ့ updateLocations ထဲ location id တူမတူ စစ်လိုက်ပြီး တူခဲ့ရင် ပြောင်းလိုက်တဲ့ တန်ဖိုးကိုလက်ရှိloop လုပ်နေတဲ့ location name အဖြစ် update လုပ်ကာ  ကျန်တဲ့ array တစ်ခုလုံးကို newLocations အဖြစ် သိမ်းလိုက်ပါတယ်
- updateLocation state ရဲ့ တန်ဖိုးအနေနဲ့ newLocations အဖြစ် update လုပ်ပေးလိုက်တာပါတယ်
- ခုဆို locations update လုပ်တာ ပြင်ဆင်ထားပါပြီး
- backend မှာ location id နဲ့ ၀င်လာမယ့် request ကို လက်ခံပါမယ်။

```js
//locationRouter.ts

import express, { Request, Response } from "express";
import { db } from "../../db/db";
import { checkAuth } from "../auth/auth";
export const locationsRouter = express.Router();

locationsRouter.put(
  "/:locationId",
  checkAuth,
  async (req: Request, res: Response) => {
    const locationId = req.params.locationId;
    const { name, address } = req.body;
    if (!locationId || !name || !address) return res.sendStatus(400);
    const locationResult = await db.query(
      "update locations set name = $1, address = $2 where id = $3",
      [name, address, locationId]
    );
    res.send(locationResult.rows[0]);
  }
);

locationsRouter.delete(
  "/:locationId",
  checkAuth,
  async (req: Request, res: Response) => {
    const locationId = req.params.locationId;
    if (!locationId) return res.sendStatus(400);
    try {
      await db.query("delete from locations where id = $1", [locationId]);
      res.sendStatus(200);
    } catch (err) {
      res.sendStatus(500);
    }
  }
);

locationsRouter.post("/", checkAuth, async (req: Request, res: Response) => {
  const { name, address, companyId } = req.body;
  const isValid = name && address && companyId;
  if (!isValid) return res.sendStatus(400);
  const locationResult = await db.query(
    "insert into locations (name,address,companies_id) values ($1, $2, $3)",
    [name, address, companyId]
  );
  res.send(locationResult.rows[0]);
});
```
> ရှင်းလင်းချက်
```js

locationsRouter.put(
  "/:locationId",
  checkAuth,
  async (req: Request, res: Response) => {
    const locationId = req.params.locationId;
    const { name, address } = req.body;
    if (!locationId || !name || !address) return res.sendStatus(400);
    const locationResult = await db.query(
      "update locations set name = $1, address = $2 where id = $3",
      [name, address, locationId]
    );
    res.send(locationResult.rows[0]);
  }
);
```
- ၀င်လာတဲ့ id နဲ စစ်ပြီး request နဲ့ အတူပါလာတဲ့ name နဲ့ address တွေကို database မှာ update လုပ်ပေးလိုက်တာဖြစ်ပါတယ်။

```js
locationsRouter.delete(
  "/:locationId",
  checkAuth,
  async (req: Request, res: Response) => {
    const locationId = req.params.locationId;
    if (!locationId) return res.sendStatus(400);
    try {
      await db.query("delete from locations where id = $1", [locationId]);
      res.sendStatus(200);
    } catch (err) {
      res.sendStatus(500);
    }
  }
);
```
- ဒါကတော့ delete middleware ဖြစ်ပြီး delete method နဲ့ request ၀င်လာခဲ့ရင် ပါလာတဲ့ ID နဲ့ တူတဲ့ location ကို database မှာ ဖျက်ပေးလိုက်တာဖြစ်ပါတယ်
##

