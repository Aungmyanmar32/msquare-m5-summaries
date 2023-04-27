## MSquare Programing Fullstack Course
### Episode-*60* 
### Summary For `Room(2)`
- ပြီးခဲ့တဲ့ သင်ခန်းစာမှာ ရွေးလိုက်တဲ့ ရက် နဲ့ user info တွေကို review and comfirm ဆိုတဲ့ step မှာ ပြခဲ့ကြပါတယ်။
![enter image description here](https://raw.githubusercontent.com/Aungmyanmar32/msquare-m5-summaries/main/Screenshot%202023-04-26%20124836.png)

- အောက်နားက finish ဆိုတဲ့ ခလုတ်ကို comfirm လို့ ပြောင်းပြီး နှိပ်လိုက်တဲ့အခါ context ထဲက data တွေ log ထုတ်ကြည့်ပါမယ်။
### PassportStepper.tsx
![enter image description here](https://raw.githubusercontent.com/Aungmyanmar32/msquare-m5-summaries/main/Screenshot%202023-04-26%20132319.png)
- PassportAppContext  value ကို  data object ထဲမှာ သိမ်းလိုက်တာဖြစ်ပါတယ်။

![enter image description here](https://raw.githubusercontent.com/Aungmyanmar32/msquare-m5-summaries/main/Screenshot%202023-04-26%20132334.png)
- comfirm ကို နှိပ်လိုက်ချိန်မှာ  data object  ကို log ထုတ်လိုက်တာပဲဖြစ်ပါတယ်

![enter image description here](https://raw.githubusercontent.com/Aungmyanmar32/msquare-m5-summaries/main/Screenshot%202023-04-26%20132517.png)
##
### ဆက်ပြီးတော့ ပြီးခဲ့တဲ့ သင်ခန်းစာက လေ့လာခဲ့တဲ့ server ဆီ fetch လုပ်တာကို ပြန်လေ့လာကြည့်ကြပါမယ်။

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
  }, [selectedMonth ]);

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
  }, [selectedMonth ]);
```
- PassportAppContext ရှိ selectedMonth  update ဖြစ်တိုင်း useEffect ကို သုံးပြီး server ဆီ request လုပ်ခိုင်းထားတာဖြစ်ပါတယ်။
```
const fetchAvailability = async () => {
    const url = `http://localhost:5000/availability?month=${selectedMonth}`;
    const response = await fetch(url);
    const data = await response.json();
    console.log("data from server", data);
  };
```
- server ဆီ request လုပ်တဲ့ function ဖြစ်ပြီး selectedMonth ကို query အနေနဲ့ ထည့်ပြီး request လုပ်ထားတာဖြစ်ပါတယ်။
