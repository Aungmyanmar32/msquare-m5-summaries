## MSquare Programing Fullstack Course
### Episode-*68* 
### Summary For `Room(1)` intermediate Class
## Abstraction
- backend ထဲရှိ code တွေကို router သီးသန့် ခွဲထုတ်ပေးထားပါပြီး
- ဒါပေမယ့် router တွေထဲမှာ SQL query တွေပါ ရောရေးထားတာမလို့ အနည်းငယ် ရှုပ်ထွေးနေပါတယ်
- ခု သင်ခန်းစာမှာတော့ route  တွေ ခွဲပေးလိုက်သလိုပဲ SQL query တွေကို သီးသန့်ခွဲရေးပြီး လိုတဲ့နေရာမှာ ပြန်ခေါ်သုံးလိုက်မှာဖြစ်ပါတယ်
- ခု menuRouter ထဲရှိ SQL query တွေကုိ သီးသန့်ခွဲရေးလိုက်ပါမယ်။
- /backend/src/ အောက်မှာ query folder တစ်ခုလုပ်ပါမယ်
- အဲ့ဒီ query folder ထဲမှာ menuQuery.ts ဖိုင်တစ်ခုလုပ်ပါမယ်
```js
import { db } from "../db/db";
import { CreateMenuParams, Menu } from "../types/menu";

interface MenuQuries {
  createMenu: (createMenuParams: CreateMenuParams) => Promise<Menu>;

}

export const menuQuries: MenuQuries = {
  createMenu: async (createMenuParams: CreateMenuParams) => {
    const { name, description, price, locationIds, assetUrl } =
      createMenuParams;
    const text =
      "INSERT INTO menus(name, price, description, asset_url) VALUES($1, $2, $3, $4) RETURNING *";
    const values = [name, price, description, assetUrl];
    const result = await db.query(text, values);
    const menu = result.rows[0] as Menu;
    const menuId = menu.id;
    await db.query(
      `insert into menus_locations (menus_id, locations_id) select * from unnest ($1::int[], $2::int[])`,
      [Array(locationIds.length).fill(menuId), locationIds]
    );
    return { id: menuId, name, price, locationIds };
  },
 
};

```
- menu table ထဲမှာ menu တစ်ခု insert လုပ်လိုက်ပြီး အဲဒီ menu ရဲ့ id နဲ့ locations id ကို menus_locations table ထဲမှာ join လုပ်လိုက်တာဖြစ်ပါတယ်
- type folder တစ်ခု ထပ်လုပ်ပြီး types တွေ သီးသန့်ခွဲပြီး သိမ်းကာ import လုပ်သုံးမှာဖြစ်ပါတယ်။
- menu အတွက် သီးသန့် type file တစ်ခု လုပ်ပါမယ်။
```js
// backend/src/typing/menu.ts
export interface Menu {
  id?: string;
  name: string;
  price: number;
  assetUrl?: string;
  locationIds: string[];
  menuCategoryIds?: string[];
  addonCategoryIds?: string[];
}

export interface CreateMenuParams {
  name: string;
  price: number;
  locationIds: string[];
  description?: string;
  assetUrl?: string;
}
```
- ခု သတ်မှတ်ထားတဲ့ menuQuries ကို menuRouter ထဲမှာ သုံးပါမယ်
```js
// backend/src/router/menuRouter --> 

menusRouter.post("/", async (req: Request, res: Response) => {
  const { name, price, description, locationIds, assetUrl } = req.body;
  const isValid = name && locationIds && locationIds.length;
  if (!isValid) return res.sendStatus(400);
  const menu = await menuQuries.createMenu({
    name,
    price,
    locationIds,
    description,
    assetUrl,
  });
  res.send(menu);
});

```
- Post method နေရာမှာအရင် code တွေ ဖျက်ပြီး  menuQuries ထဲက createMenu ကို ခေါ်သုံးလိုက်ပြီး parameter အနေနဲ့ request body ထဲက data တွေ ထည့်ပေးလိုက်တာဖြစ်ပါတယ်
- ဆက်ပြီး Get method အတွက် query တွေ ကို menuQuries မှာ သတ်မှတ်လိုက်ပါမယ်။

```js
import { db } from "../db/db";
import { CreateMenuParams, Menu } from "../types/menu";

interface MenuQuries {
  createMenu: (createMenuParams: CreateMenuParams) => Promise<Menu>;
  getMenu: (menuId: string) => Promise<Menu | undefined>;
  /* getMenusByLocationId: (locationId: string) => Promise<Menu[] | []>;
  deleteMenu: (menuId: string) => Promise<void>;
  updateMenu: (
    menuId: string,
    updateMenuParams: Partial<Menu>
  ) => Promise<Menu>; */
}

export const menuQuries: MenuQuries = {
  createMenu: async (createMenuParams: CreateMenuParams) => {
    .......................................
    ................................
  },
  
  getMenu: async (menuId: string) => {
    const menuResult = await db.query("select * from menus where id = $1", [
      menuId,
    ]);
    const hasMenu = menuResult.rows.length > 0;
    if (hasMenu) {
      const menu = menuResult.rows[0] as Menu;
      const menusLocationsResult = await db.query(
        "select locations_id from menus_locations where menus_id = $1",
        [menuId]
      );
      const locationIds = menusLocationsResult.rows.map(
        (row) => row.locations_id
      );
      const menusMenuCategoriesResult = await db.query(
        "select menu_categories_id from menus_menu_categories where menus_id = $1",
        [menuId]
      );
      const menuCategoryIds = menusMenuCategoriesResult.rows.map(
        (row) => row.menu_categories_id
      );
      const menusAddonCategoriesResult = await db.query(
        "select addon_categories_id from menus_addon_categories where menus_id = $1",
        [menuId]
      );
      const addonCategoryIds = menusAddonCategoriesResult.rows[0];
      return {
        id: menuId,
        name: menu.name,
        price: menu.price,
        locationIds,
        menuCategoryIds,
        addonCategoryIds,
      };
    }
  },
};
```
- request ၀င်လာတဲ့ menu id နဲ့ menu table ထဲ က အချက်အလက်တွေ အပြင် menu နဲ့ join ထားတဲ့ table တွေထဲက  locationIds, menuCategoryIds, addonCategoryIds,
တွေကို ပါ လှမ်းယူပြီး return လုပ်ပေးထားလိုက်ပါတယ်

- ခု ရေးထားတဲ့ getMenu query ကို menuRouter မှာ သုံးလိုက်ပါမယ်။

```
menusRouter.get("/:menuId", async (req: Request, res: Response) => {
  const menuId = req.params.menuId;
  const menu = await menuQuries.getMenu(menuId);
  res.send(menu);
});

```
- ခုဆိုရင် menuRouter ထဲက queries တွေကို သီးသန့်ခွဲထုတ်ထားပြီး ရှင်းရှင်းလင်းလင်းနဲ့ လိုတဲ့နေရာမှာ ပြန်သုံးလို့ရပြီဖြစ်ပါတယ် 

### Show menu
- menu page ထဲ ရောက်တဲ့အခါ context ထဲမှာ သိမ်းထားတဲ့ menu item တွေကို mui card ကို သုံးပြီး ပြပေးပါမယ်

```js
//Menus.tsx

import { useContext } from "react";
import Layout from "./Layout";
import { AppContext } from "../contexts/AppContext";
import {
  Box,
  Card,
  CardActionArea,
  CardContent,
  CardMedia,
  Typography,
} from "@mui/material";

const Menus = () => {
  const { menus } = useContext(AppContext);
  const sampleMenuImageUrl =
    "https://msquarefdc.sgp1.cdn.digitaloceanspaces.com/Spicy%20seasoned%20seafood%20noodles.png";
  return (
    <Layout title="Menus">
      <Box sx={{ ml: 3, mt: 5, display: "flex" }}>
        {menus.map((menu) => {
          return (
            <Card sx={{ maxWidth: 345, mr: 5 }}>
              <CardActionArea>
                <CardMedia
                  component="img"
                  height="140"
                  image={sampleMenuImageUrl}
                  alt="green iguana"
                />
                <CardContent>
                  <Typography gutterBottom variant="h5" component="div">
                    {menu.name}
                  </Typography>
                  <Typography variant="body2" color="text.secondary">
                    Lorem Ipsum is simply dummy text of the printing and
                    typesetting industry. Lorem Ipsum has been the industry's
                    standard dummy text ever since the 1500s
                  </Typography>
                </CardContent>
              </CardActionArea>
            </Card>
          );
        })}
      </Box>
    </Layout>
  );
};

export default Menus;
```
```
 const sampleMenuImageUrl =
    "https://msquarefdc.sgp1.cdn.digitaloceanspaces.com/Spicy%20seasoned%20seafood%20noodles.png";
    
<Card sx={{ maxWidth: 345, mr: 5 }}>
              <CardActionArea>
                <CardMedia
                  component="img"
                  height="140"
                  image={sampleMenuImageUrl}
                  alt="green iguana"
                />
```
-- ပုံကိုတော့ နမူနာအနေနဲ့ ပဲ ထည့်ပေးထားတာဖြစ်ပါတယ်
```js
 <CardContent>
                  <Typography gutterBottom variant="h5" component="div">
                    {menu.name}
                  </Typography>
                  <Typography variant="body2" color="text.secondary">
                    Lorem Ipsum is simply dummy text of the printing and
                    typesetting industry. Lorem Ipsum has been the industry's
                    standard dummy text ever since the 1500s
                  </Typography>
                </CardContent>
```
- context ထဲက meuns array ကို loop လုပ်ပြီး mui cardContent ထဲမှာ name တွေ ပြပေးလိုကတာဖြစ်ပါတယ်

![enter image description here](https://cdn.discordapp.com/attachments/1076154921562411008/1112231593453572157/image.png)

##
