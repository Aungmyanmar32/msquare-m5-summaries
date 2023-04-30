## MSquare Programing Fullstack Course
### Episode-*63* 
### Summary For `Room(2)`
## Show available *TIME* for booking
- ဒီနေ့သင်ခန်းစာမှာတော့ ရွေးလိုက်တဲ့ ရက်မှာ ရှိတဲ့ အချိန်တွေကို ပြပေးပြီး တစ်ခုကို ရွေးလို့ရအောင် ဆက်လုပ်သွားကြပါမယ်။
- အရင်ဆုံး DateAndTime component မှာ ရွေးလိုက်တဲ့ ရက်ပေါ် မူတည်ပြီး available slot ရှိမရှိ စစ်ပါမယ်

![enter image description here](https://raw.githubusercontent.com/Aungmyanmar32/msquare-m5-summaries/main/Screenshot%202023-04-30%20043742.png)
```js
  const slotToBook = availability
    .find((item) => item.date === dayjs(bookingDate).format("DD-MM-YYYY"))
    ?.slots.filter((slot) => slot.availableSlot !== 0);

  console.log(slotToBook);
```
- availability ထဲမှာ ရွေးလိုက်တဲ့ ရက် ရှိမရှိ ရှာလိုက်ပါတယ်။
- ရှာတွေ့ခဲ့ရင် အဲ့ဒီရက်ထဲက available slot 0 မဖြစ်နေတဲ့ slot တွေကို filter လုပ်လိုက်ပြီး slotToBook ဆိုတဲ့ variable မှာ သိမ်းလိူက်တာ ဖြစ်ပါတယ်။
- ခု ဆိုရင် book လုပ်လို့ရမယ့် slot တွေ ရှာပြီးပြီး ဆိုတော့ time တွေ ကို ပြပေးပြီး user တွေ ရွေးလို့ရအောင် ဆက်လုပ်သွားကြပါမယ်
- mui component တွေ ထဲက [radio component](https://mui.com/material-ui/react-radio-button/) ကို အသုံးပြုပါမယ်
```js
    <FormControl>
      <FormLabel id="demo-radio-buttons-group-label">Select Time</FormLabel>
      <RadioGroup
        aria-labelledby="demo-radio-buttons-group-label"
        name="radio-buttons-group"
      >
        <FormControlLabel value="test" control={<Radio />} label="test" />
   
      </RadioGroup>
    </FormControl>
```
- FormLabel က ရွေးလို့ရမယ့် အကြောင်းအရာ ကို ပြပေးမှာဖြစ်ပါတယ်။ **Select Time** ဆိုပြီး ထည့်ပေးထားပါတယ်
- FormControlLabel မှာတော့
   - value က ရွေးလိုက်ရင် ဖြစ်လာမယ့် တန်ဖိုးဖြစ်ပြီး
   - lable ကတော့ ရွေးလို့ရမယ့် တန်ဖိုးကို ပြပေးထားကာ
   - control မှာတော့ ထိန်းချုပ်လို့ရမယ့် function တွေ event တွေ အသုံးပြုလို့ရမှာ ဖြစ်ပါတယ်။

##
- အထက်မှာ နမူနာ ပြထားတဲ့ mui radio component ကို သုံးပြီး slotToBook ထဲက အချိန်တွေကို loop လုပ်ပြီး ပြပေးမှာ ဖြစ်ပါတယ်။

![enter image description here](https://raw.githubusercontent.com/Aungmyanmar32/msquare-m5-summaries/main/Screenshot%202023-04-30%20050454.png)

```js
 <FormControl
          sx={{
            mx: "0 auto",
            mt: 2,
          }}
        >
          <FormLabel id="demo-radio-buttons-group-label">Select Time</FormLabel>
          <RadioGroup
            aria-labelledby="demo-radio-buttons-group-label"
            name="radio-buttons-group"
          >
            {slotToBook &&
              slotToBook.map((slot) => (
                <FormControlLabel
                  value={slot.time}
                  control={<Radio />}
                  label={slot.time}
                  key={slot.time}
                />
              ))}
          </RadioGroup>
        </FormControl>
```
- mui radio component ကို သုံးပြီး slotToBook array ကို loop ပတ်ထားပါတယ်
- FormControlLabel က value နဲ့ label မှာ slotslotToBook array ရှိ item တစ်ခုချင်းမှာ ပါတဲ့ time တွေကို ထည့်ပေးထားပါတယ်။
- အခု react app မှာ date picker ထဲက available ဖြစ်တဲ့ ရက်တစ်ရက် ရွေးလိုက်ပါက time တွေ ရွေးလို့ရမယ့် radio group တစ်ခု ပေါ်လာမှာဖြစ်ပါတယ်။
- ဆက်ပြီးရွေးလိုက်တဲ့ time ကို context data မှာ update လုပ်ပြီး သိမ်းမှာ ဖြစ်ပါတယ်။

![enter image description here](https://raw.githubusercontent.com/Aungmyanmar32/msquare-m5-summaries/main/Screenshot%202023-04-30%20111216.png)
```js
control={<Radio  onChange={(e)=>updateData({...contextData,time:  e.target.value})}/>}
```
- ရွေးလိုက်တဲ့ အချိန်ကို contextData ထဲက time ရဲ့ တန်ဖိုးအဖြစ် သတ်မှတ်လိုက်တာဖြစ်ပါတယ်။
- ဆက်ပြီး context ထဲက အချက်အလက်တွေ ပြတဲ့အခါ အချိန်ကိုပါ ထည့်ပြပေးပါမယ်
```js
// ConfirmAndReview.tsx
import { Box } from "@mui/material";
import { useContext } from "react";
import { PassportAppContext } from "../contexts/PassportAppContext";
import dayjs from "dayjs";

const ConfirmAndReivew = () => {
  const { bookingDate, time, user, updateData } =
    useContext(PassportAppContext);
  return (
    <Box
      sx={{
        display: "flex",
        flexDirection: "column",
        textAlign: "left",
      }}
    >
      <h1>Confirm and review</h1>
      <h2>Date : {dayjs(bookingDate).format("DD-MM-YYYY")}</h2>
      <h2>Time : {time}:00</h2>
      <h2>Name : {user?.name}</h2>
      <h2>Nrc Nmuber : {user?.nrcNumber}</h2>
      <h2>Email : {user?.email}</h2>
    </Box>
  );
};

export default ConfirmAndReivew;

```
- ခု ဆို ရက် နဲ့ အချိန် ကို ရွေးပြီး user info တွေ ထည့်ကာ next လုပ်လိုက်ပါက အောက်ကပုံလို ပြပေးမှာဖြစ်ပါတယ်။

![enter image description here](https://raw.githubusercontent.com/Aungmyanmar32/msquare-m5-summaries/main/Screenshot%202023-04-30%20112140.png)
