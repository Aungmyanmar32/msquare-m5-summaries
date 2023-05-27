## MSquare Programing Fullstack Course
### Episode-*67* 
### Summary For `Room(1)` intermediate Class
## Refactor
- အရင် သင်ခန်းစာ တွေမှာ backend code တွေကို အစီစဥ်တကျ မဟုတ်ပဲ server.ts မှာပဲ စုပြုံပြီးရေးခဲ့ကြပါတယ်
- ခုသင်ခန်းစာမှာတော့ code တွေကို သူ့ folder နဲ့သူ သီးသန့်ခွဲရေးပြီး server.ts မှာ ပြန်ခေါ်သုံးကြပါမယ်
- src folder အောက်မှာ routers folder တစ်ခုလုပ်ပါမယ်
- အဲ့ဒီfolder ထဲမှ request ၀င်လာမယ့် route တွေကို ထားမှာဖြစ်ပါတယ်။
- အရင်ဆုံး menuRouter.ts ဖိုင်တစ်ခုလုပ်ပြီး server.ts မှာ menu route တွေ အကုန်လုံးကို cut လုပ်ပြီး menuRouter.ts ထဲ ပြောင်းထည့်လိုက်ပါမယ်
```js

//menuRouter.ts

import  Express,{Request,Response}  from "express";
import { db } from "../../db/db";
import { checkAuth } from "../auth/auth";

export const menuRouter = Express.Router()

menuRouter.post("/menu", checkAuth, async (req: Request, res: Response) => {
    const { name, price } = req.body;
    const text = "INSERT INTO menus(name, price) VALUES($1, $2) RETURNING *";
    const values = [name, price];
    const result = await db.query(text, values);
    res.send({ result });
  });
  

  
  menuRouter.delete("/menus/:menuId", checkAuth, async (req: Request, res: Response) => {
    const menuId = parseInt(req.params.menuId, 10);
    if (!menuId) return res.send("menu id is required");
    const text = "DELETE FROM menus WHERE id =($1) RETURNING *";
    const values = [menuId];
    const result = await db.query(text, values);
    res.send({ result });
  });
  
  menuRouter.put("/menus/:menuId", checkAuth, async (req: Request, res: Response) => {
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
- express router ကို သုံးပြီး app နေရာမှာ menuRouter နဲ့ အစားထိုးထားပြီး ကျန်တာကတော့ server.ts ကနေ ကူးထားတဲ့ အတိုင်းပဲ ဖြစ်ပါတယ်
- အဲ့ဒီ menuRouter ကို server.ts မှာ ခေါ်သုံးပါမယ်။

![enter image description here](https://github.com/Aungmyanmar32/msquare-m5-summaries/blob/main/Screenshot%202023-05-21%20025610.png?raw=true)
- use middleware နဲ့ menuRouter ကို ခေါ်သုံးလိုက်ပါတယ်
- **`app.use("/menus", menuRouter )`** ဆိုပြီး ရှေ့မှာ /menus route ပါ ထည့်ပေးလိုက်တာမလို့ menuRouter.ts က route တွေမှာ /menus ဆိုပြီး ထည့်ပေးစရာ မလိုတော့ပါဘူး

![enter image description here](https://github.com/Aungmyanmar32/msquare-m5-summaries/blob/main/Screenshot%202023-05-21%20030213.png?raw=true)
- ကျန်တဲ့ route တွေကိုလဲ အထက်ပါအတိုင်းပဲ ဆက်လုပ်သွားပါမယ်။
```console
/backend/src/routers
touch appRouter.ts authRouter.ts menuCategoriesRouter.ts
```

```js
//appRouter.ts

import Express, { Request, Response } from "express";
import { db } from "../../db/db";
import { checkAuth } from "../auth/auth";

export const appRouter = Express.Router();

appRouter.get("/data", checkAuth, async (req: Request, res: Response) => {
  const menus = await db.query("select * from menus");
  const menuCategories = await db.query("select * from menu_categories");
  const addons = await db.query("select * from addons");
  const addonCategories = await db.query("select * from addon_categories");
  const locations = await db.query("select * from locations");
  const menusLocations = await db.query("select * from menus_locations");

  res.send({
    menus: menus.rows,
    menuCategories: menuCategories.rows,
    addons: addons.rows,
    addonCategories: addonCategories.rows,
    locations: locations.rows,
    menusLocations: menusLocations.rows,
  });
});

```

```js
//authRouter.ts

import Express, { Request, Response } from "express";
import { db } from "../../db/db";

import jwt from "jsonwebtoken";
import bcrypt from "bcrypt";
import { config } from "../config";

export const authRouter = Express.Router();

authRouter.post("/auth/login", async (req, res) => {
  const { email, password } = req.body;
  console.log(req.body);

  if (!email || !password) return res.sendStatus(400);

  const userResult = await db.query("select * from users where email = $1", [
    email,
  ]);

  if (!userResult.rows.length) return res.sendStatus(401);

  const user = userResult.rows[0];
  const hashedPassword = user.password;

  const isCorrectPassword = await bcrypt.compare(password, hashedPassword);

  if (!isCorrectPassword)
    return res.sendStatus(401).send("Invalid Crendentials");

  const userToken = { id: user.id, name: user.name, email: user.email };
  const accessToken = jwt.sign(userToken, config.jwtSecret);

  res.send({ accessToken });
});

authRouter.post("/auth/register", async (req, res) => {
  const { name, email, password } = req.body;
  if (!name || !email || !password) return res.sendStatus(400);

  const hashedPassword = await bcrypt.hash(password, 10);

  const text =
    "INSERT INTO users(name, email, password) VALUES($1, $2, $3) RETURNING *";
  const values = [name, email, hashedPassword];

  try {
    const result = await db.query(text, values);
    const userInfo = result.rows[0];
    delete userInfo.password;
    res.send(userInfo);
  } catch (error) {
    console.log(error);
    res.send(500);
  }
});

```
```js
//menuCategoriesRouter.ts

import Express, { Request, Response } from "express";
import { db } from "../../db/db";
import { checkAuth } from "../auth/auth";

export const menuCategoriesRouter = Express.Router();

menuCategoriesRouter.post(
  "/",
  checkAuth,
  async (req: Request, res: Response) => {
    const { name } = req.body;

    if (!name) {
      return res
        .status(400)
        .send({ error: "Bad RequestPage. Plase provide menu categories name" });
    }

    const text = "INSERT INTO menu_categories(name) VALUES($1) RETURNING *";
    const values = [name];
    const result = await db.query(text, values);
    res.send({ menuCategories: result.rows });
  }
);

menuCategoriesRouter.delete(
  "/:menuCategoriesId",
  checkAuth,
  async (req: Request, res: Response) => {
    const menuCategoriesId = req.params.menuCategoriesId;

    if (!menuCategoriesId)
      return res.send({ error: "menuCategories Id required" });

    const text = "DELETE FROM menu_categories WHERE id=($1) RETURNING *";
    const values = [menuCategoriesId];
    const result = await db.query(text, values);
    res.send({ menuCategory: result.rows });
  }
);

```
```js
//server.ts

import dotenv from "dotenv";
dotenv.config();

import express, { Request, Response } from "express";
import cors from "cors";
import { menuRouter } from "./src/routers/menuRouters";
import { menuCategoriesRouter } from "./src/routers/menuCategoriesRouter";
import { authRouter } from "./src/routers/authRouter";
import { appRouter } from "./src/routers/appRouter";
const app = express();
const port = 5000;

app.use(express.json());
app.use(cors());

//Using Router
app.use("/menus", menuRouter);
app.use("/menu-categories", menuCategoriesRouter);
app.use("/auth", authRouter);
app.use("/", appRouter);

app.listen(port, () => console.log(`Example app listening on port ${port}!`));

```
##
### create default data for new user ( company,location,menu,...etc)
- user အသစ်တစ်ယောက် register လုပ်လိုက်တာနဲ့  ( company,location,menu,...etc) စတဲ့ နမူနာ data တွေကို create လုပ်ပြီး database မှာ သိမ်းထားလိုက်ပါမယ်။
- Login ပြန်၀င်လာတဲ့ အချိန်မှာတော့ ခနက သိမ်းထားတဲ့ data တွေကို ပြန်ပြပေးလိုက်ပြီး update လုပ်လို့ရအောင် လုပ်ပေးလိုက်မှာဖြစ်ပါတယ်။
-အရင်ဆုံး database ထဲက table တွေထဲမှာရှိတဲ့ data တွေ အကုန်ဖျက်ပစ်ပါမယ်
- join table ထဲက data တွေ အရင်ဖျက်ပြီးမှ ကျန်တဲ့ table ထဲက data တွေ ဖျက်ပေးရပါမယ်
```sql
DELETE FROM menus_locations;
DELETE FROM menus_menu_categories;
DELETE FROM menus_addon_categories;
DELETE FROM addons;
DELETE FROM addon_categories;
DELETE FROM locations;
DELETE FROM menus;
DELETE FROM menus_categories;
DELETE FROM users;

```
- ပြီးရင် companies table တစ်ခု create လုပ်ပါမယ်။
```sql
CREATE TABLE companies (
id serial PRIMARY KEY NOT NULL,
name text NOT NULL
)
```
- အဲ့ဒီ company နဲ့ user  table ကို ချိတ်ဆက်ပါမယ်
```sql
alter table users add column companies_id integer not null REFERENCES companies
```
- user register လုပ်တဲ့ အခါ server ဆီမှာ register ဆိုတဲ့ route နဲ့ လက်ခံပြီး database က user table မှာ သိမ်းထားခဲ့ပါတယ်
- အဲ့ဒီ user တွေကို company နဲ့ ချိတ်ဆက်ပေးမှာဖြစ်ပါတယ်
- အရင်ဆုံး database မှာ table အသစ်တွေ create လုပ်ပါမယ်
```sql
CREATE TABLE companies (
id serial PRIMARY KEY NOT NULL,
name text NOT NULL
)
```

### လိုအပ်တဲ့  table တွေ လုပ်ပြီးပြီး ဆိုတော့ register လုပ်တဲ့အခါ default data တွေ create လုပ်ပါမယ်

```js
// backend/server.ts --> /auth/register
qpp.post("/auth/register", async (req: Request, res: Response) => {
  const { name, email, password } = req.body;
  if (!name || !email || !password) return res.sendStatus(400);
  const hashedPassword = await bcrypt.hash(password, 10);

  try {
    const companiesResult = await db.query(
      "insert into companies(name) values($1)  RETURNING *",
      ["Defult company"]
    );
    console.log(companiesResult.rows);
    const companiesId = companiesResult.rows[0].id;

    const text =
      "INSERT INTO users(name, email, password,companies_id) VALUES($1, $2, $3,$4) RETURNING *";
    const values = [name, email, hashedPassword, companiesId];
    const userResult = await db.query(text, values);
    const user = userResult.rows[0];
    delete user.password;

    const locationResult = await db.query(
      "insert into locations(name,address,companies_id) values($1,$2,$3)  RETURNING *",
      ["Defult location", "Defult address", companiesId]
    );

    const locationId = locationResult.rows[0].id;

    const menusResult = await db.query(
      "insert into menus(name,price) select * from unnest($1::text[],$2::int[]) returning *",
      [
        ["mote-hinn-kharr", "shan-khout-swell"],
        [500, 1000],
      ]
    );

    const menus = menusResult.rows;
    const defaultMenuId1 = menus[0].id;
    const defaultMenuId2 = menus[1].id;
    await db.query(
      "insert into menus_locations(menus_id,locations_id) select * from unnest($1::int[],$2::int[]) returning *",
      [
        [defaultMenuId1, defaultMenuId2],
        [locationId, locationId],
      ]
    );

    const menuCategoriesResult = await db.query(
      "insert into menu_categories (name) values ('defaultMenuCategory1'),('defaultMenuCategory2') returning *"
    );
    const defaultMenuCategories = menuCategoriesResult.rows;
    const defaultMenuCategoryId1 = defaultMenuCategories[0].id;
    const defaultMenuCategoryId2 = defaultMenuCategories[1].id;

    await db.query(
      `insert into menus_menu_categories (menus_id, menu_categories_id) values(${defaultMenuId1}, ${defaultMenuCategoryId1}), (${defaultMenuId2}, ${defaultMenuCategoryId2})`
    );

    const defaultAddonCategoriesResult = await db.query(
      "insert into addon_categories (name,is_required) values ('Drinks',true), ('Sizes',true) returning *"
    );

    const addonCotegoriesIds = defaultAddonCategoriesResult.rows;
    const defaultAddonCategoryId1 = addonCotegoriesIds[0].id;
    const defaultAddonCategoryId2 = addonCotegoriesIds[1].id;

    await db.query(
      `insert into menus_addon_categories (menus_id, addon_categories_id) values (${defaultMenuId1}, ${defaultAddonCategoryId1}), (${defaultMenuId2}, ${defaultAddonCategoryId2})`
    );

    await db.query(`insert into addons (name, price, addon_categories_id) values ('Cola', 50, ${defaultAddonCategoryId1}), ('Pepsi', 50, ${defaultAddonCategoryId1}), 
      ('Large', 30, ${defaultAddonCategoryId2}), ('Normal', 0, ${defaultAddonCategoryId2})`);

    res.send(user);
  } catch (err) {
    console.log(err);

    res.sendStatus(500);
  }
});


```
> ရှင်းလင်းချက်

```js
const companiesResult = await db.query(
      "insert into companies(name) values($1)  RETURNING *",
      ["Defult company"]
    );
    console.log(companiesResult.rows);
    const companiesId = companiesResult.rows[0].id;
```
- companies table ထဲကို default company ဆိုတဲ့ name တစ်ခု ထည့်လိုက်တာပါ
- အဲ့ဒီ company ရဲ့ id ကို companiesId ဆိုပြီး သိမ်းထားပါတယ်
```js
 const text =
      "INSERT INTO users(name, email, password,companies_id) VALUES($1, $2, $3,$4) RETURNING *";
    const values = [name, email, hashedPassword, companiesId];
    const userResult = await db.query(text, values);
    const user = userResult.rows[0];
    delete user.password;
```
- request နဲ့အတူပါလာတဲ့ user info တွေကို users table ထဲမှာ သိမ်းလိုက်ပြီး companiesId  ကိုပါ ထည့်ပေးလိုက်ပါတယ်။
- ဒီ user  က ဒီ company နဲ့ relation (ချိတ်ထား) တာဖြစ်ပါတယ်
```js
const locationResult = await db.query(
      "insert into locations(name,address,companies_id) values($1,$2,$3)  RETURNING *",
      ["Defult location", "Defult address", companiesId]
    );

    const locationId = locationResult.rows[0].id;
```
- locations table မှာလည်း default locatios တစ်ခု ထည့်ပေးလိုက်ပြီး compaiesId နဲ့ ချိတ်ပေးထားပါတယ်
- အဲ့ဒီ locationရဲ့ id ကို locationId ဆိုပြီး သိမ်းထားပါတယ်

```js
  const menusResult = await db.query(
      "insert into menus(name,price) select * from unnest($1::text[],$2::int[]) returning *",
      [
        ["mote-hinn-kharr", "shan-khout-swell"],
        [500, 1000],
      ]
    );

    const menus = menusResult.rows;
    const defaultMenuId1 = menus[0].id;
    const defaultMenuId2 = menus[1].id;
```
- menus table မှာလည်း menu item နှစ်ခု create လုပ်လိုက်ပြီး 
- လုပ်ထားတဲ့ menu item နှစ်ခုရဲ့ id တွေကိုလဲ သိမ်းထားလိုက်ပါတယ်
```js
 await db.query(
      "insert into menus_locations(menus_id,locations_id) select * from unnest($1::int[],$2::int[]) returning *",
      [
        [defaultMenuId1, defaultMenuId2],
        [locationId, locationId],
      ]
    );
```
- အထက်က location နဲ့  menus တွေရဲ့ join table မှာလည်း id တွေ သွားထည့်ပြီး ချိတ်ဆက်လိုက်ပါတယ်
```js
   const menuCategoriesResult = await db.query(
      "insert into menu_categories (name) values ('defaultMenuCategory1'),('defaultMenuCategory2') returning *"
    );
    const defaultMenuCategories = menuCategoriesResult.rows;
    const defaultMenuCategoryId1 = defaultMenuCategories[0].id;
    const defaultMenuCategoryId2 = defaultMenuCategories[1].id;

```
- menu_categories table ထဲမှာလည်း defaultMenuCategories နှစ်ခု create လုပ်ထားလိုက်ပြီး id  တွေ ထုတ်ယူထားပါတယ်
```js
    await db.query(
      `insert into menus_menu_categories (menus_id, menu_categories_id) values(${defaultMenuId1}, ${defaultMenuCategoryId1}), (${defaultMenuId2}, ${defaultMenuCategoryId2})`
    );
```
- ရလာတဲ့ defaultMenuCategories id တွေကို menu နဲ့ join table ဖြစ်တဲ့ menus_menu_categories table ထဲ ချိတ်ဆက်ပေးထားလိုက်ပါတယ်။
```js
 const defaultAddonCategoriesResult = await db.query(
      "insert into addon_categories (name,is_required) values ('Drinks',true), ('Sizes',true) returning *"
    );

    const addonCotegoriesIds = defaultAddonCategoriesResult.rows;
    const defaultAddonCategoryId1 = addonCotegoriesIds[0].id;
    const defaultAddonCategoryId2 = addonCotegoriesIds[1].id;
```
- addon_categories table ထဲမှာ Drinks နဲ့ Sizes ဆိုတဲ့ addon နှစ်ခု ထည့်လိုက်ပြီး id တွေ ထုတ်သိမ်းထားပါတယ်
```js
await db.query(
      `insert into menus_addon_categories (menus_id, addon_categories_id) values (${defaultMenuId1}, ${defaultAddonCategoryId1}), (${defaultMenuId2}, ${defaultAddonCategoryId2})`
    );
```
- ရလာတဲ့ addon_categories id နဲ့ menu id တွေကို join table ထဲမှာ ချိတ်ဆက်ပေးထားပါတယ်
```js
await db.query(`insert into addons (name, price, addon_categories_id) values ('Cola', 50, ${defaultAddonCategoryId1}), ('Pepsi', 50, ${defaultAddonCategoryId1}), 
      ('Large', 30, ${defaultAddonCategoryId2}), ('Normal', 0, ${defaultAddonCategoryId2})`);

```
- Cola နဲ့ Pepsi addon နှစ်ခု လုပ်လိုက်ပြီး drinks categories နဲ့ ချိတ်ဆက်ထားပါတယ်
- နောက်ထပ် large နဲ့ Normal addon နှစ်ခု ထပ်ထည့်လိုက်ပြီး Sizes categories နဲ့ ချိတ်ဆက်ထားပါတယ်
```js
 res.send(user);
```
- နောက်ဆုံး password ဖယ်ထားတဲ့ user object ကို response ပြန်လိုက်ပါတယ်။
- ခု user အသစ်တစ်ယောက်ကို register လုပ်ကြည့်ပါက database table တွေမှာ အထက်မှာ ထည့်ထားတဲ့ data (rows) တွေ ၀င်လာတာကို မြင်ရမှာဖြစ်ပါတယ်
