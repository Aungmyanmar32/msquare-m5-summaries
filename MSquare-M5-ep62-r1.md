## MSquare Programing Fullstack Course
### Episode-*61* 
### Summary For `Room(1)` intermediate Class
##
- ခု သင်ခန်းစာမှာတော့ ပြထားတဲ့ menu ကို နှိပ်လိုက်တဲ့အခါ အဲ့ဒီ menu item နဲ့ ပတ်သတ်တဲ့ datail တွေကို ပြပေးဖို့ လုပ်ကြပါမယ်။
- component folder ထဲမှာ MenuDetail.tsx ဖိုင်တစ်ခု လုပ်ပါမယ်။
```js
//MenuDetail.tsx
import { useContext, useEffect, useState } from "react";
import { AppContext } from "../contexts/AppContext";
import { useParams } from "react-router-dom";
import { Box, Button, TextField } from "@mui/material";
import { Menu } from "../types/types";
import { config } from "../configs/config";
import Layout from "./Layout";

export const MenuDetail = () => {
  const { menus } = useContext(AppContext);
  const { menuId } = useParams();

  let menu: Menu | undefined;

  if (menuId) {
    menu = menus.find((menu) => menu.id === parseInt(menuId, 10));
  }

  return (
    <Layout>
      <Box
        sx={{
          display: "flex",
          flexDirection: "column",
          maxWidth: 200,
          margin: "0 auto",
          mt: 5,
        }}
      >
        {menu ? (
          <Box>
            <TextField
              id="outlined-basic"
              label="Name"
              variant="outlined"
              defaultValue={menu.name}
              sx={{ mb: 2 }}
              
            />
            <TextField
              id="outlined-basic"
              label="Price"
              variant="outlined"
              type="number"
              defaultValue={menu.price}
              sx={{ mb: 2 }}
              
            />
            
            <Button variant="contained">Update</Button>
          </Box>
        ) : (
          <h1>Menu not found</h1>
        )}
      </Box>
    </Layout>
  );
};

```
- ပြီးရင် အဲ့ဒီ component ကို render လုပ်ဖို့ route တစ်ခု ကို index.tsx မှာ သတ်မှတ်ပါမယ်။
```js
{
 path:  "/menus/:menuId",
 element:  <MenuDetail  />,
}
```
- /menus နောက်မှာ params အနေနဲ့  id ကို ထည့်ပေးလိုက်တာဖြစ်ပါတယ်
- ခု app  ကို ဖွင့်ပြိး id တစ်ခုခုကို param အနေနဲ့ /menu/ နောက်မှာာထည့်စမ်းကြည့်ပါက menu list ထဲ တူတဲ့ id ရှိခဲ့ရင် menu item ကို ပြပေးမှာဖြစ်ပြီး မရှိခဲ့ရင် **Menu not found** ဆိုပြီး ပြပေးမှာ ဖြစ်ပါတယ်။
![enter image description here](https://github.com/Aungmyanmar32/msquare-m5-summaries/blob/main/ep62r1%20%281%29.png?raw=true)
##
### update menu items
- ဆက်ပြီး menu itemsကို input မှာ ထည့်ပြီး update နှိပ်လိုက်ချိန်မှာ update လို့ ရအောင် လုပ်ပါမယ်။
```js
// MenuDetail.tsx;
import { useContext, useEffect, useState } from "react";
import { AppContext } from "../contexts/AppContext";
import { useParams } from "react-router-dom";
import { Box, Button, TextField } from "@mui/material";
import { Menu } from "../types/types";
import { config } from "../configs/config";
import Layout from "./Layout";

export const MenuDetail = () => {
  const { menus } = useContext(AppContext);
  const { menuId } = useParams();

  let menu: Menu | undefined;

  if (menuId) {
    menu = menus.find((menu) => menu.id === parseInt(menuId, 10));
  }
  const [newMenu, setNewMenu] = useState({
    name: "",
    price: 0,
  });

  useEffect(() => {
    if (menu) {
      setNewMenu({
        name: menu.name,
        price: menu.price,
      });
    }
  }, [menu]);

  const updateMenu = async () => {
    const res = await fetch(`${config.apiBaseUrl}/menus/${menu?.id}`, {
      method: "PUT",
      headers: {
        "Content-Type": "application/json",
      },
      body: JSON.stringify(newMenu),
    });
    const data = await res.json();
    alert(data.message);
  };
  return (
    <Layout>
      <Box
        sx={{
          display: "flex",
          flexDirection: "column",
          maxWidth: 200,
          margin: "0 auto",
          mt: 5,
        }}
      >
        {menu ? (
          <Box>
            <TextField
              id="outlined-basic"
              label="Name"
              variant="outlined"
              defaultValue={menu.name}
              sx={{ mb: 2 }}
              onChange={(evt) =>
                setNewMenu({ ...newMenu, name: evt.target.value })
              }
            />
            <TextField
              id="outlined-basic"
              label="Price"
              variant="outlined"
              type="number"
              defaultValue={menu.price}
              sx={{ mb: 2 }}
              onChange={(evt) =>
                setNewMenu({
                  ...newMenu,
                  price: parseInt(evt.target.value, 10),
                })
              }
            />

            <Button variant="contained" onClick={updateMenu}>
              Update
            </Button>
          </Box>
        ) : (
          <h1>Menu not found</h1>
        )}
      </Box>
    </Layout>
  );
};

```
- Update button ကို click လုပ်လိုက်တဲ့အခါ updateMenu function ကို ခေါ်ထားလိုက်ပါတယ်
- updateMenu function မှာတော့ server ဆီ put  နဲ့ request လုပ်ပြီး update လုပ်လိုက်မှာဖြစ်ပါတယ်။
- ခု request ကို server မှာလက်ခံပါမယ်။
```js
//server.ts
app.put("/menus/:menuId", async (req: Request, res: Response) => {
  const menuId = req.params.menuId;
  if (!menuId) return res.send("Menu id is required.");
  const payload = req.body;
  const { name, price } = payload;
  if (!name || !price) return res.send("Please provide at least name or price");
  const text = "UPDATE menus SET name=$1, price=$2  WHERE id =($3) RETURNING *";
  const values = [name, price, menuId];
  await db.query(text, values);
  res.send({ message: "successfully updated" });
});
```
- request နဲ့ ပါလာတဲ့ param ကို id အဖြစ်ယူပြီး menus table ထဲက id နဲ့ တိုက်စစ်ပြီး database မှာ Update လုပ်လိုက်တာဖြစ်ပါတယ်။၏
- ခု app မှာ စမ်းသပ်ကြည့်ပါမယ်

![enter image description here](https://github.com/Aungmyanmar32/msquare-m5-summaries/blob/main/ep62r1%20%285%29.png?raw=true)
- menu list ထဲမှာ items နှစ်ခုရှိပါတယ်
- pal pyote nan pyar ကို shwe yin aye အဖြစ်ပြောင်းကြည့်ပါမယ်။
![enter image description here](https://github.com/Aungmyanmar32/msquare-m5-summaries/blob/main/ep62r1%20%282%29.png?raw=true)

![enter image description here](https://github.com/Aungmyanmar32/msquare-m5-summaries/blob/main/ep62r1%20%283%29.png?raw=true)
- update နှိပ်လိုက်တဲ့ အချိန်မှာ alert တစ်ခု ပြပေးမှာဖြစ်ပါတယ်။

![enter image description here](https://github.com/Aungmyanmar32/msquare-m5-summaries/blob/main/ep62r1%20%284%29.png?raw=true)
- ခု menus page ကို ပြန်သွားပြီး refresh လုပ်ကြည့်ပါက   menu list မှာ ပြောင်းသွားတာကို မြင်ရမှာဖြစ်ပါတယ်

![enter image description here](https://github.com/Aungmyanmar32/msquare-m5-summaries/blob/main/ep62r1%20%286%29.png?raw=true)

## 
### Menu list ထဲက itemsတစ်ခုခုကို နှိပ်လိုက်တာနဲ့ MenuDetail page ထဲ ၀င်သွားပြီး detail တွေ ပြင်နိူင် ပြနိုင်အောင် ဆက်လုပ်ပါမယ်
- Menus.tsx မှာ Menu List တွေပြတဲ့နေရာမှာ react router dom က Link ကို သုံးပြီး MenuDetail page ထဲ ပို့ပေးလိုက်တာဖြစ်ပါတယ်
```js
 <h1> Menu List</h1>
        <Stack direction="column" spacing={1}>
          {menus.map((menu) => (
            <Link to={`/menus/${menu.id}`} key={menu.id}>
              <Chip
                sx={{
                  cursor: "pointer",
                  ":hover": { backgroundColor: "lightblue" },
                }}
                label={menu.name}
                variant="outlined"
                onDelete={() => handleDelete(menu.id)}
              />
            </Link>
          ))}
        </Stack>
```
- ခုဆိုရင် MENU LIST ထဲက menu item တစ်ခုခုကို နှိပ်လိုက်တာနဲ့ menu detail  page ကို ရောက်သွားမှာဖြစ်ပါတယ်။
