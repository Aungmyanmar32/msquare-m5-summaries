## MSquare Programing Fullstack Course
### Episode-*64* 
### Summary For `Room(2)`
## Post booking info to server
- ဒီနေ့ သင်ခန်းစာမှာတော့ passport booking app ရဲ့ နောက်ဆုံး အဆင့်အနေနဲ့ booking info တွေ အကုန်လုံးကို server ဆီပို့ပြီး slot တွေ အတိုးအလျော့ လုပ်မှာဖြစ်ပါတယ်။
- passportStepper ထဲက step 3 မှာ ရှိတဲ့ comfirm ကို နှိပ်လိုက်တဲ့အခါ server ဆီ request လုပ်မှာ ဖြစ်ပါတယ်

![enter image description here](https://raw.githubusercontent.com/Aungmyanmar32/msquare-m5-summaries/main/Screenshot%202023-04-30%20130927.png)

- confirm ကိုနှိပ်လိုက်ချိန်မှာ createBooking function ကို ခေါ်ထားလိုက်ပါတယ်?
- createBooking function မှာ server ဆီ POST နဲ့ request လုပ်ထားပြီး data ကို body အနေနဲ့ ထည့်ပေးလိုက်တာဖြစ်ပါတယ်။
- အဲဒီ request ကို express ထဲက server.ts မှာ လက်ခံပါမယ်။

![enter image description here](https://raw.githubusercontent.com/Aungmyanmar32/msquare-m5-summaries/main/Screenshot%202023-04-30%20123343.png)
- request body ထဲက data တွေ ယူဖို့အတွက် `app.use(express.json());` ကို သုံးလိုက်ပါတယ်။
```
app.post("/createBooking", (req, res) => {

const { bookingDate, time } = req.body;
```
- createBooking route ကနေ request အနေနဲ့ ၀င်လာမယ့် data ထဲက bookingDate နဲ့ time ကို ခွဲထုတ်လိုက်တာဖြစ်ပါတယ်
```
const  requestedAvailability = availability.find(
(item) =>  item.date === bookingDate);

if (!requestedAvailability) {
return  res.send({ error:  "Date not found." });
}
```
- request ကနေ ပါလာတဲ့ date နဲ့ data.ts မှာ ရှိတဲ့ date တူမတူ စစ်လိုက်ပြီး မတူခဲ့ရင် Date not found error ပြန်ပို့ပေးလိုက်ပါတယ်
```
const requestedTime = requestedAvailability.slots.find(
    (slot) => slot.time === parseInt(time, 10)
  );

  if (!requestedTime) {
    return res.send({ error: "Time not found." });
  }
```
- တူတဲ့ date ရှိခဲ့တယ်ဆိုရင် အဲ့ဒီရလာမယ့် requestedAvailability ထဲက time နဲ့ request ၀င်လာတဲ့ time နဲ့ တူတဲ့ item ကို ဆက်ပြီး ရှာလိုက်တာဖြစ်ပါတယ်
- မတွေ့ခဲ့ရင်တော့ ခုနကလိုပဲ error တစ်ခု response လုပ်လိုက်မှာ ဖြစ်ပါတယ်

```
 if (!requestedTime.availableSlot)
    return res.send({ message: "No available slot." });

  requestedTime.bookedSlot = requestedTime.bookedSlot + 1;
  requestedTime.availableSlot = requestedTime.availableSlot - 1;

  res.send({ message: "Booked Successfully!" });
```
- ရက်လဲတွေ့၊ အချိန်လဲ တွေ့တယ်ဆိုပါက အဲ့ဒီအချိန်မှာ ရှိတဲ့ slot က 0 ဖြစ်မဖြစ် ထပ်စစ်လိုက်ပြီး
- 0 ထပ်ကြီးနေသေးတယ်ဆို ရင် book လုပ်လို့ ရတာမလို့ book လုပ်တဲ့အနေနဲ့
   - bookedSlot ကို တစ် တိုးလိုက်ပြီး
   - availableSlot ကို တစ်လျော့လိုက်တာ ဖြစ်ပါတယ်။
   - နောက်ဆုံးမှာတော့ booking တစ်ခု လုပ်ပြီးပြီး ဖြစ်ကြောင်း response ပြန်လိုက်တာဖြစ်ပါတယ်။
   - ခု ရက်တွေ အချိန်တွေ userinfo တွေ ထည့်ပြီး comfirm လုပ်ကြည့်ပါက booked ဆိုပြီး log ထုတ်ပေးတာကို မြင်ရမှာ ဖြစ်ပါတယ်

![enter image description here](https://raw.githubusercontent.com/Aungmyanmar32/msquare-m5-summaries/main/Screenshot%202023-04-30%20130946.png)
