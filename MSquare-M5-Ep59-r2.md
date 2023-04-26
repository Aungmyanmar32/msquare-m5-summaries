## MSquare Programing Fullstack Course
### Episode-*59* 
### Summary For `Room(2)`
##
### Fetch data to server
- ဒီနေ့ သင်ခန်းစာမှာတော့ date picker မှာ ရက်တစ်ရက် ရွေးလိုက်တဲ့အခါ server ဆီ request လုပ်တာကို လေ့လာကြပါမယ်။
- backend ထဲက data.ts မှာ date နည်းနည်းပြောင်းလိုက်ပါမယ်။
```js
// backend/data.ts

export interface Slot {
  time: number;
  totoalSlot: number;
  bookedSlot: number;
  availableSlot: number;
}

export interface Availability {
  date: string;
  month: number;
  slots: Slot[];
}

export const availability: Availability[] = [
  {
    date: "27-04-2023",
    month: 3,
    slots: [
      { time: 9, totoalSlot: 100, bookedSlot: 50, availableSlot: 50 },
      { time: 10, totoalSlot: 100, bookedSlot: 70, availableSlot: 30 },
      { time: 11, totoalSlot: 100, bookedSlot: 20, availableSlot: 80 },
      { time: 12, totoalSlot: 100, bookedSlot: 100, availableSlot: 0 },
      { time: 13, totoalSlot: 100, bookedSlot: 60, availableSlot: 40 },
    ],
  },
  {
    date: "28-04-2023",
    month: 3,
    slots: [
      { time: 9, totoalSlot: 100, bookedSlot: 50, availableSlot: 50 },
      { time: 10, totoalSlot: 100, bookedSlot: 70, availableSlot: 30 },
      { time: 11, totoalSlot: 100, bookedSlot: 20, availableSlot: 80 },
      { time: 12, totoalSlot: 100, bookedSlot: 100, availableSlot: 0 },
      { time: 13, totoalSlot: 100, bookedSlot: 60, availableSlot: 40 },
    ],
  },
  {
    date: "30-04-2023",
    month: 3,
    slots: [
      { time: 9, totoalSlot: 100, bookedSlot: 50, availableSlot: 50 },
      { time: 10, totoalSlot: 100, bookedSlot: 70, availableSlot: 30 },
      { time: 11, totoalSlot: 100, bookedSlot: 20, availableSlot: 80 },
      { time: 12, totoalSlot: 100, bookedSlot: 100, availableSlot: 0 },
      { time: 13, totoalSlot: 100, bookedSlot: 60, availableSlot: 40 },
    ],
  },
  {
    date: "01-05-2023",
    month: 4,
    slots: [
      { time: 9, totoalSlot: 100, bookedSlot: 50, availableSlot: 50 },
      { time: 10, totoalSlot: 100, bookedSlot: 70, availableSlot: 30 },
      { time: 11, totoalSlot: 100, bookedSlot: 20, availableSlot: 80 },
      { time: 12, totoalSlot: 100, bookedSlot: 100, availableSlot: 0 },
      { time: 13, totoalSlot: 100, bookedSlot: 60, availableSlot: 40 },
    ],
  },
];

```
- server.ts မှာလည်း အရင်သင်ခန်းစာကအတိုင်းပဲ ဆက်ထားပါမယ်။
```js
// backend/server.ts

import express from "express";
import cors from "cors";
import { availability } from "./data";
const app = express();
const port = 5000;

app.use(cors());


app.get("/availability", (req, res) => {
  const month = req.query.month as string;
  const choosenMonth = parseInt(month, 10);
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
- အထက်ပါ code တွေကို အရင်သင်ခန်းစာတွေမှာ ရှင်းပြခဲ့ပြီးပြီမလို့ ထပ်ပြီး မရှင်းတော့ပါဘူး
- request ကနေ ၀င်လာမယ့် query.month ကို သုံးပြီး data ထဲရှိ month နဲ့ တူ မတူ တိုက်စစ်ပြီး ရလာတဲ့ array အသစ်ကို response လုပ်လိုက်တာပဲ ဖြစ်ပါတယ်။
- ဆက်ပြီး react apps ထဲက passport app မှာ server ဆီ request လုပ်မယ့် code တွေ ပြန်ရေးထည့်ပါမယ်။
```js
import { Box, Button } from "@mui/material";
import { DatePicker, LocalizationProvider } from "@mui/x-date-pickers";
import { AdapterDayjs } from "@mui/x-date-pickers/AdapterDayjs";
import { log } from "console";
import dayjs, { Dayjs } from "dayjs";
import React, { useContext, useEffect, useState } from "react";
import { PassportAppContext } from "../contexts/PassportAppContext";
import PassportStepper from "../components/PassportStepper";

const PassportApp = () => {

  // use context from contexts
  const { updateData, ...data } = useContext(PassportAppContext);

  const selectedMonth = dayjs(data.bookingDate).month();

  useEffect(() => {
    fetchAvailability();
  }, [data]);

  // fetch to server with query param
  const fetchAvailability = async () => {
    const url = `http://localhost:5000/availability?month=${selectedMonth}`;
    const response = await fetch(url);
    const data = await response.json();
    console.log("data from server", data);
  };
  
  return <PassportStepper />;
};

export default PassportApp;

```
```
  const { updateData, ...data } = useContext(PassportAppContext);
```
- PassportAppContext ကို အသုံးပြုထားပါတယ်
```
const selectedMonth = dayjs(data.bookingDate).month();
```
- PassportAppContext ရှိ data ထဲက bookingDate ကို သုံးပြီး ရွေးလိုက်တဲ့ ရက်ရဲ့  လကို လှမ်းယူလိုက်တာဖြစ်ပါတယ်။
```
 useEffect(() => {
    fetchAvailability();
  }, [data]);
```
- PassportAppContext ရှိ data တစ်ခုခု update ဖြစ်တိုင်း useEffect ကို သုံးပြီး server ဆီ request လုပ်ခိုင်းထားတာဖြစ်ပါတယ်။
```
const fetchAvailability = async () => {
    const url = `http://localhost:5000/availability?month=${selectedMonth}`;
    const response = await fetch(url);
    const data = await response.json();
    console.log("data from server", data);
  };
```
- server ဆီ request လုပ်တဲ့ function ဖြစ်ပြီး selectedMonth ကို query အနေနဲ့ ထည့်ပြီး request လုပ်ထားတာဖြစ်ပါတယ်။
- ဆက်ပြီးတော့ DateAndTime component မှာ bookingDate ကို update လုပ်တဲ့အခါ ထည့်ပေးလိုက်မယ့် date ကို အနည်းငယ် ပြင်လိုက်ပါမယ်။

![enter image description here](https://raw.githubusercontent.com/Aungmyanmar32/msquare-m5-summaries/main/Screenshot%202023-04-26%20122640.png)
- bookingDate ကို update လုပ်တဲ့အခါ `dayjsObj.format("DD/MM/YYYY")` အစား **`dayjsObj.toDate()`** လို့ ပြောင်းလိုက်တာဖြစ်ပါတယ်။
- ခု ရက်တစ်ရက်ကို ရွေးကြည့်လိုက်တဲ့ အခါ  ရွေးလိုက်တဲ့ ရက်ပါတဲ့ month တစ်လလုံးရှိ data တွေ ပြပေးမှာဖြစ်ပါတယ်

![enter image description here](https://raw.githubusercontent.com/Aungmyanmar32/msquare-m5-summaries/main/Screenshot%202023-04-26%20122902.png)
##
### Comfirm and review
- ဆက်ပြီး ရွေးလိုက်တဲ့ ရက် ရယ် , ထည့်လိုက်တဲ့ user info တွေရယ် ကို နောက်ဆုံး step မှာ ပြပေးကြည့်ပါမယ်။
```js
// ConfirmAndReview.tsx
import { Box } from "@mui/material";
import { useContext } from "react";
import { PassportAppContext } from "../contexts/PassportAppContext";
import dayjs from "dayjs";

const ConfirmAndReivew = () => {
  const { bookingDate, time, user, updateData } = useContext(PassportAppContext);
  return (
    <Box
      sx={{
        display: "flex",
        flexDirection: "column",
        justifyContent: "center",
        alignItems: "center",
        textAlign: "left",
      }}
    >
      <h1>Confirm and review</h1>
      <h2>{dayjs(bookingDate).format("DD-MM-YYYY")}</h2>
      <h2>{user?.name}</h2>
      <h2>{user?.nrcNumber}</h2>
      <h2>{user?.email}</h2>
    </Box>
  );
};

export default ConfirmAndReivew;

```
```
 const { bookingDate, time, user, updateData } = useContext(PassportAppContext);
```
- PassportAppContext ကို ခေါ်သုံးထားပြီး context ရဲ့ value ကို  **`{ bookingDate, time, user, updateData }`** ဆိုပြီး destructure လုပ်လိုက်တာဖြစ်ပါတယ်။
```
return (
    <Box
      sx={{
        display: "flex",
        flexDirection: "column",
        justifyContent: "center",
        alignItems: "center",
        textAlign: "left",
      }}
    >
      <h1>Confirm and review</h1>
      <h2>{dayjs(bookingDate).format("DD-MM-YYYY")}</h2>
      <h2>{user?.name}</h2>
      <h2>{user?.nrcNumber}</h2>
      <h2>{user?.email}</h2>
    </Box>
  );
```
- mui Box တစ်ခု return လုပ်ထားပြီး ခုနက destructure လုပ်ထားတဲ့ data တွေကို h2 tag အနေနဲ့ ပြပေးထားတာဖြစ်ပါတယ်
- react app ကို စမ်ကြည့်လိုက်ပါက ရွေးလိုက်တဲ့ ရက် နဲ့ ရိုက်ထည့်လိုက်တဲ့ user info တွေကို comfirm and review step မှာ ပြပေးတာကို မြင်ရမှာဖြစ်ပါတယ်။

![enter image description here](https://raw.githubusercontent.com/Aungmyanmar32/msquare-m5-summaries/main/Screenshot%202023-04-26%20124836.png)
