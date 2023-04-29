## MSquare Programing Fullstack Course
### Episode-*61* 
### Summary For `Room(2)`
- ဒီနေ့သင်ခန်းစာမှာတော့ ရွေးလိုက်တဲ့ ရက်နဲ့ user information တွေကို back ပြန်လုပ်တဲ့အခါ ရေးထားပြီးသားဖြစ်အောင် လုပ်ကြပါမယ်။
- server ဆီ fetch လုပ်တဲ့ function နဲ့ ရလာတဲ့ data တွေကို context မှာ ပြောင်းပြီး သိမ်းလိုက်ပါမယ်။
```js
import { createContext, useState } from "react";

//class code
interface Slot {
  time: number;
  totoalSlot: number;
  bookedSlot: number;
  availableSlot: number;
}

interface Availability {
  date: string;
  month: number;
  slots: Slot[];
}
//end of calss

interface User {
  name: string;
  nrcNumber: string;
  dateOfBirth: string;
  phoneNumber: string;
  email: string;
}

export interface BookingInfo {
  bookingDate: string | null;
  time: number | null;
  user: User | null;
  availability: Availability[];
  fetchAvailability: (value: any) => void;
  updateData: (value: any) => void;
}

const defaultContext = {
  bookingDate: null,
  time: null,
  user: null,
  availability: [],
  fetchAvailability: () => {},
  updateData: () => {},
};

export const PassportAppContext = createContext<BookingInfo>(defaultContext);

//prop = { children : <PassportApp />}

const PassportProvider = (prop: any) => {
  const [bookingData, setBookingData] = useState(defaultContext);

  const fetchAvailability = async (month: number) => {
    const response = await fetch(
      `http://localhost:5000/availability?month=${month}`
    );
    const data = await response.json();
    setBookingData({ ...bookingData, availability: data });
  };

  return (
    <PassportAppContext.Provider
      value={{ ...bookingData, updateData: setBookingData, fetchAvailability }}
    >
      {prop.children}
    </PassportAppContext.Provider>
  );
};

export default PassportProvider;

```
##
```
const defaultContext = {
  bookingDate: null,
  time: null,
  user: null,
  availability: [],
  fetchAvailability: () => {},
  updateData: () => {},
};
```
- context ရဲ့ default value မှာ availability ကို empty array , fetchAvailability ကို  function အဖြစ် ထပ်ပေါင်းထည့်ထားပါတယ်။
```
  const fetchAvailability = async (month: number) => {
    const response = await fetch(
      `http://localhost:5000/availability?month=${month}`
    );
    const data = await response.json();
    setBookingData({ ...bookingData, availability: data });
  };
```
- provider component ထဲမှာ fetchAvailability FUNCTIONကို သတ်မှတ် လိုက်ပါတယ်။
- response ပြန်လာတဲ့ data ကို availability ရဲ့ တန်ဖိုးအဖြစ် သတ်မှတ်ပေးထားပါတယ်
- ဆက်ပြီး DateAndTime component ထဲက date picker မှာ value propတစ်ခု ထပ်ပေါင်းထည့်လိုက်ပါမယ်

![enter image description here](https://raw.githubusercontent.com/Aungmyanmar32/msquare-m5-summaries/main/Screenshot%202023-04-29%20135443.png)
- context value ထဲက updateData, fetchAvailability, availability, bookingDate တွေကို ခွဲထုတ်ပြီး သိမ်းလိုက်ပါတယ်။
 `value={bookingDate ? dayjs(bookingDate) : null}`
 - context value ထဲက bookingDate မှာ တန်ဖိုးတစ်ခုခု ရှိရင် date picker ရဲ့ value အနေနဲ့ ပြခိုင်းထားတာဖြစ်ပါတယ်။
 - ခု react app မှာ ရက်တစ်ရက် ရွေးပြီး next ကို နှိပ်ကြည့်လိုက်ပါ။ ပြီးရင် back နဲ့ ပြန်လာကြည့်ပါက ခုနက ရွေးထားတဲ့ ရက်ကို date picker မှာ ပြပေးနေမှာ ဖြစ်ပါတယ်။
 - အလားတူပဲ userInfo component  ထဲမှာ ရိုက်ထည့်လိုက်တဲ့ data တွေကိုလည်း ပြန်ပြပေးနိုင်အောင် လုပ်ကြည့်ကြပါမယ်။
 ```js
// UserInfo.tsx
import { Box, TextField } from "@mui/material";
import { useContext } from "react";
import { PassportAppContext } from "../contexts/PassportAppContext";

const UserInfo = () => {
  const { updateData, ...data } = useContext(PassportAppContext);

  return (
    <Box
      sx={{
        display: "flex",
        flexDirection: "column",
      }}
    >
      <TextField
        required
        id="name"
        sx={{ maxWidth: "300px", margin: "0 auto", mb: 2 }}
        placeholder="Name"
        value={data.user?.name ? data.user?.name : null}
        onChange={(evt) =>
          updateData({
            ...data,
            user: { ...data.user, name: evt.target.value },
          })
        }
      />
      <TextField
        required
        id="nrc_number"
        sx={{ maxWidth: "300px", margin: "0 auto", mb: 2 }}
        placeholder="NRC number"
        value={data.user?.nrcNumber ? data.user?.nrcNumber : null}
        onChange={(evt) =>
          updateData({
            ...data,
            user: { ...data.user, nrcNumber: evt.target.value },
          })
        }
      />
      <TextField
        required
        id="email"
        sx={{ maxWidth: "300px", margin: "0 auto", mb: 2 }}
        placeholder="Email"
        value={data.user?.email ? data.user?.email : null}
        onChange={(evt) =>
          updateData({
            ...data,
            user: { ...data.user, email: evt.target.value },
          })
        }
      />
    </Box>
  );
};

export default UserInfo;

```
- ခု react app မှာ စမ်းကြည့်ပါက back ခလုတ် ကို နှိပ်လိုက်ချိန်မှာ ထည့်ပြီးသားdata တွေ ပြပေးနေတာကို မြင်ရမှာပါ။
##
### shouldDisableDate
- mui date picker မှာ ရက်တွေ ရွေးလို့မရအောင်ပိတ်ထားချင်ရင် **shouldDisableDate** ကို အသုံးပြုရပါတယ်။
### syntax
```js
<DatePicker shouldDisableDate={callback-function}/>
```
-shouldDisableDate က ထည့်ပေးလိုက်တဲ့ callback - function ဟာ ရောက်နေတဲ့ လ အထဲမှာ ရှိတဲ့ ရက်အလိုက် loop လုပ်ပြီး ခေါ်ပေးမှာဖြစ်ပါတယ်။
- ဥပမာ april လထဲမှာရက် သုံးဆယ် ရှိပါက callback - function ဟာ အခါ သုံးဆယ် run မှာဖြစ်ပါတယ်
- shouldDisableDate မှာ ထည့်ပေးလိုက်တဲ့ callback - function ကနေ return true ဖြစ်တဲ့ ရက်မှန်သမျှ ကို date picker မှာ ရွေးလို့ မရအောင် ပိတ်ပေးလိုက်မှာ ဖြစ်ပါတယ်။
##
### ခု ကျနော်တို့ data မှာ ရှိတဲ့  ရက်ကိုပဲ ရွေးလို့ရအောင် date picker မှာ လုပ်ပါမယ်။
-
![enter image description here](https://raw.githubusercontent.com/Aungmyanmar32/msquare-m5-summaries/main/Screenshot%202023-04-29%20142355.png)
- shouldDisableDate prop ကို date picker ထဲမှာ ခေါ်လိုက်ပြီး callback အနေနဲ့ dateToDisable function  ကို ခေါ်လိုက်တာဖြစ်ပါတယ်။
```js
const dateToDisable = (date: Dayjs) => {
    const canBookDate = availability.find(
      (item) => item.date === date.format("DD-MM-YYYY")
    );

    if (!canBookDate) {
      return true;
    }

    const isAvailable = canBookDate.slots.some(
      (slot) => slot.availableSlot > 0
    );

    return isAvailable ? false : true;
  };
```
- ရွေးလိုက်တဲ့ date ကို server က ပြန်လာတဲ့ data ထဲရှိ date နဲ့  တူမတူ တိုက်စစ်လိုက်ပါတယ်။
- မတွေ့ခဲ့ရင်တော့ return true လုပ်ထားတာမလို့ အဲ့ဒီ ရက်ကို ပိတ်ပေးမှာဖြစ်ပါတယ်။
- တွေ့ခဲရင်တော့ availableSlot တွေ ရှိမရှိ ထပ်စစ်ပါတယ်။
- **array.some( )** method က **return true တစ်ခါဖြစ်တာ**နဲ့ array ထဲရှိ ကျန်တဲ့ item တွေ ဘာဖြစ်ဖြစ် **true ကိုပဲ return ပြန်ပေး**မှာဖြစ်ပါတယ်။
- availableSlot တွေ ရှိခဲ့ရင် return false လုပ်ပြီး ရက်ကို ဖွင့်ထားပေးမှာဖြစ်ပြီး
- availableSlot တွေ မရှိခဲ့ရင် return true လုပ်ပြီး ရက်ကို ပိတ်ထားပေးမှာဖြစ်ပါတယ်။
### all code in DateAndTime.tsx
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
  const contextData = useContext(PassportAppContext);

  const { updateData, fetchAvailability, availability, bookingDate } =
    contextData;

  const dateToDisable = (date: Dayjs) => {
    const canBookDate = availability.find(
      (item) => item.date === date.format("DD-MM-YYYY")
    );

    if (!canBookDate) {
      return true;
    }

    const isAvailable = canBookDate.slots.some(
      (slot) => slot.availableSlot > 0
    );

    return isAvailable ? false : true;
  };

  return (
    <LocalizationProvider dateAdapter={AdapterDayjs}>
      <Box sx={{ maxWidth: 200, margin: "0 auto", minHeight: 200 }}>
        <DemoContainer components={["DatePicker"]}>
          <DatePicker
            label="Basic date picker"
            disablePast
            format="DD-MM-YYYY"
            value={bookingDate ? dayjs(bookingDate) : null}
            onOpen={() => fetchAvailability(dayjs().month())}
            onMonthChange={(date: Dayjs) => fetchAvailability(date.month())}
            shouldDisableDate={dateToDisable}
            onChange={(value) => {
              const dayjsObj = value as Dayjs;
              updateData({
                ...contextData,
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
