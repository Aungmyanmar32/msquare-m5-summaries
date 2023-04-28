## MSquare Programing Fullstack Course
### Episode-*61* 
### Summary For `Room(2)`
- အရင်သင်ခန်းစာမှာ date တစ်ခုခုရွေးလိုက်ရင် server ဆီ request လုပ်လိုက်ပါတယ်။
- ဒီနေ့သင်ခန်းစာမှာတော့ Date picker ကို ဖွင့်လိုက်တာနဲ့ server ဆီ request လုပ်နိုင်အောင် လုပ်ပါမယ်။
- အရင်ဆုံး server ဆီ request လုပ်တဲ့ function ကို **PassportApp.tsx** ကနေ **DateAndTime.tsx** ထဲ ပြောင်းထည့်ပါမယ်
```js
// PassportApp.tsx

import { useContext } from "react";
import { PassportAppContext } from "../contexts/PassportAppContext";
import PassportStepper from "../components/PassportStepper";

const PassportApp = () => {
  // use context from contexts
  const { updateData, ...data } = useContext(PassportAppContext);

  return <PassportStepper />;
};

export default PassportApp;

```

- ဆက်ပြီး fetchAvailability function ကို DateAndTime.tsx မှာ ထည့်ပြီး date picker ကို ဖွင့်လိုက်ချိန်နဲ့ month ပြောင်းတဲ့ အချိန်တွေမှာ server ဆီ request လုပ်နိုင်အောင် ရေးလိုက်ပါမယ်။

```js

// DateAndTime.tsx
import { Box } from "@mui/material";
import { DatePicker, LocalizationProvider } from "@mui/x-date-pickers";
import { DemoContainer } from "@mui/x-date-pickers/internals/demo";
import dayjs, { Dayjs } from "dayjs";
import { useContext, useEffect } from "react";
import { PassportAppContext } from "../contexts/PassportAppContext";
import { AdapterDayjs } from "@mui/x-date-pickers/AdapterDayjs";

const DateAndTime = () => {
  const { updateData, ...data } = useContext(PassportAppContext);
  // console.log(data);

  const fetchAvailability = async (month: number) => {
    const response = await fetch(
      `http://localhost:5000/availability?month=${month}`
    );
    const data = await response.json();
    console.log(data);
  };

  return (
    <LocalizationProvider dateAdapter={AdapterDayjs}>
      <Box sx={{ maxWidth: 200, margin: "0 auto", minHeight: 200 }}>
        <DemoContainer components={["DatePicker"]}>
          <DatePicker
            label="Basic date picker"
            disablePast
            format="DD-MM-YYYY"
            onOpen={() => fetchAvailability(dayjs().month())}
            onMonthChange={(date: Dayjs) => fetchAvailability(date.month())}
            onChange={(value) => {
              const dayjsObj = value as Dayjs;
              updateData({
                ...data,
                bookingDate: dayjsObj.toDate(),
              });
              //setSelectedMonth(month);
            }}
          />
        </DemoContainer>
      </Box>
    </LocalizationProvider>
  );
};

export default DateAndTime;
```

> ရှင်းလင်းချက်

```js
  const fetchAvailability = async (month: number) => {
    const response = await fetch(
      `http://localhost:5000/availability?month=${month}`
    );
    const data = await response.json();
    console.log(data);
  };
```
- fetchAvailability function ကို  သတ်မှတ်ထားပြီး month ဆိုတဲ့ parameter  တစ်ခု လက်ခံထားပါတယ်
- အဲ့ဒီ parameter ကို server ဆီ  request  လုပ်တဲ့အခါ query အနေနဲ့ ထည့်ပေးထားပါတယ်။

```js
<DatePicker
            label="Basic date picker"
            disablePast
            format="DD-MM-YYYY"
            onOpen={() => fetchAvailability(dayjs().month())}
            onMonthChange={(date: Dayjs) => fetchAvailability(date.month())}
            onChange={(value) => {
              const dayjsObj = value as Dayjs;
              updateData({
                ...data,
                bookingDate: dayjsObj.toDate(),
              });
              //setSelectedMonth(month);
            }}
          />
```
- onOpen နဲ့ onMonthChange event တွေမှာ fetchAvailability function ကို ခေါ်ပြီး server ဆီ  request လုပ်ထားတာဖြစ်ပါတယ်။
`onOpen={() => fetchAvailability(dayjs().month())}`
- Date picker ကို ဖွင့်လိုက်တာနဲ့  fetchAvailability function ကို ခေါ်လိုက်တာဖြစ်ပါတယ်။
- parameter အနေနဲ့ လက်ရှိdate ထဲက month ကို ထည့်ပေးလိုက်တာဖြစ်ပါတယ်။
`onMonthChange={(date: Dayjs) => fetchAvailability(date.month())}`
- month ပြောင်းလိုက်တဲ့အချိန်မှာ  fetchAvailability function ကို ခေါ်လိုက်တာဖြစ်ပါတယ်။
- - parameter အနေနဲ့ ပြောင်းလိုက်တဲ့ month ကို ထည့်ပေးလိုက်တာဖြစ်ပါတယ်။
- ခု react app ထဲ၀င်ပြီး date picker ကို ဖွင့်လိုက်တာနဲ့ request လုပ်ပေးသွားတာကို မြင်ရမှာဖြစ်ပါတယ်။
-  month တစ်ခုချင်းစီးကို ပြောင်းကြည့်ပါကလည်း server ဆီ request လုပ်ပေးတာကို မြင်ရမှာဖြစ်ပါတယ်။
##
### backend မှာ express server တည်ဆောက်ခြင်း ကို ပြန်လေ့လာကြည့်ပါမယ်။
- အရင်က လုပ်ထားတာတွေကိုပဲ ပြန်လေ့လာမှာဖြစ်ပါတယ်။

```js
// backend /server.ts

import express from "express";
import cors from "cors";
import { availability } from "./data";
const app = express();
const port = 5000;
app.use(cors());

// app.get("/users/:id", (req, res) => {
//   const query = req.query;
//   const param = req.params;

//   console.log("query", query);
//   console.log("param", param);

//   res.send({ query, param });
// });

app.get("/availability", (req, res) => {
  //@ts-ignore
  const choosenMonth = parseInt(req.query.month, 10);
  console.log(choosenMonth);

  const foundedAvailability = availability.filter(
    (ele) => ele.month === choosenMonth
  );
  console.log(foundedAvailability);

  res.send(foundedAvailability);
});

app.listen(port, () => {
  console.log(`Example app listening on port ${port}!`);
});

```
>ရှင်းလင်းချက်

```
app.get("/availability", (req, res) => {
  //@ts-ignore
  const choosenMonth = parseInt(req.query.month, 10);
  console.log(choosenMonth);

  const foundedAvailability = availability.filter(
    (ele) => ele.month === choosenMonth
  );
  console.log(foundedAvailability);

  res.send(foundedAvailability);
});
```
- query အနေနဲ့ ပါလာမယ့် month ကို number type အရင်ပြောင်းလိုက်ပါတယ်
- availability array ထဲ က month နဲ့ choosenMonth  ရဲ့ တန်ဖိုး တူမတူ တိုက်စစ်လိုက်ပြီး တူတဲ့ elementတွေကို filter လုပ်ပြီး foundedAvailability  arrayအနေနဲ့ သိမ်းလိုက်တာဖြစ်ပါတယ်။
- နောက်ဆုံးအနေနဲ့ foundedAvailability  array ကို response လုပ်ပေးလိုက်ပါတယ်။
- ၀င်လာမယ့် request အတွက် ဆာဗာမှာ ပြင်ဆင်လို့ပြီးပါပြီး။
##
