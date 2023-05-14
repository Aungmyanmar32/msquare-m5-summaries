## MSquare Programing Fullstack Course
### Episode-*64* 
### Summary For `Room(1)` intermediate Class
## Register and Login
### Login
- Login component တစ်ခုကို တည်ဆောက်ပြီး **/login** route နဲ့ ၀င်လာရင် Login component ကို ပြခိုင်းလိုက်ပါမယ်။
```js
// Add to index.tsx ( router array)
{
 path:  "/login",
 element:  <Login  />,
},
```
```js
//Login.tsx

import {
  Box,
  Button,
  IconButton,
  Snackbar,
  TextField,
  Typography,
} from "@mui/material";
import CloseIcon from "@mui/icons-material/Close";
import { useContext, useState } from "react";
import { config } from "../configs/config";
import { Link, useNavigate } from "react-router-dom";
import { AppContext } from "../contexts/AppContext";

const Login = () => {
  const navigate = useNavigate();
  const [open, setOpen] = useState(false);
  const [user, setUser] = useState({ email: "", password: "" });

  const SignIn = async () => {
    const isValid = user.email.length > 0 && user.password.length > 0;
    if (!isValid) return setOpen(true);
  };

  const handleClose = (
    event: React.SyntheticEvent | Event,
    reason?: string
  ) => {
    if (reason === "clickaway") {
      return;
    }
    setOpen(false);
  };

  const action = (
    <>
      <IconButton
        size="small"
        aria-label="close"
        color="inherit"
        onClick={handleClose}
      >
        <CloseIcon fontSize="small" />
      </IconButton>
    </>
  );

  return (
  <Layout>
    <Box
      sx={{
        display: "flex",
        justifyContent: "center",
      }}
    >
      <Snackbar
        open={open}
        autoHideDuration={3000}
        onClose={handleClose}
        message="Please enter email and password"
        action={action}
        anchorOrigin={{ vertical: "bottom", horizontal: "right" }}
      />
      <Box
        sx={{
          display: "flex",
          flexDirection: "column",
          maxWidth: 400,
          minWidth: 400,
          mt: 5,
        }}
      >
        <TextField
          label="Email"
          variant="outlined"
          sx={{ mb: 2 }}
          onChange={(evt) => setUser({ ...user, email: evt.target.value })}
        />
        <TextField
          label="Password"
          variant="outlined"
          type="password"
          sx={{ mb: 2 }}
          onChange={(evt) => setUser({ ...user, password: evt.target.value })}
        />
        <Box
          sx={{
            display: "flex",
            alignItems: "center",
            justifyContent: "center",
            flexDirection: "column",
            mt: 5,
          }}
        >
          <Button variant="contained" onClick={SignIn}>
            Log in
          </Button>
        </Box>
      </Box>
    </Box>
  );
  </Layout>
};

export default Login;

```
- email နဲ့ password အတွက် text input နှစ်ခု နဲ့ Login ခလုတ်တစ်ခုထည့်ထားပါတယ်။
- email နဲ့ password နှစ်ခုလုံး မထည့်ပဲ login လုပ်လိုက်ရင် mui Snackbar ကို သုံးပြီ: error message ကို ပြထားတာဖြစ်ပါတယ်။
- ပြီးရင် NavBar ထဲ က login ကို နှိပ်လိုက်ရင် /login route ဆီ ပို့ပေးလိုက်ပါမယ်

![enter image description here](https://github.com/Aungmyanmar32/msquare-m5-summaries/blob/main/Screenshot%202023-05-14%20125025.png?raw=true)
- Happy pos app က App bar ရှိ login ကို နှိပ်လိုက်ပါက /login ထဲရောက်သွားမှာဖြစ်ပါတယ်။
- login page မှာ email နဲ့ password မထည့်ပဲ login လုပ်ကြည့်ပါက error message ပြပေးတာကို မြင်ရမှာပါ

![enter image description here](https://github.com/Aungmyanmar32/msquare-m5-summaries/blob/main/Screenshot%202023-05-14%20124636.png?raw=true)

##
ဆက်ပြီး register button တစ်ခုကို login component မှာ ထည့်ပါမယ်။

![enter image description here](https://github.com/Aungmyanmar32/msquare-m5-summaries/blob/main/Screenshot%202023-05-14%20130013.png?raw=true)
- signup မလုပ်ထားတဲ့ user ဆိုရင် register လုပ်ခိုင်းဖို့အတွက် /register  ကို သွားလိုက်တာပါ။
- /register route မှာပြပေးဖို့ Register component တစ်ခုနဲ့ route တစ်ခုလုပ်ပါမယ်။
```js
// Add to index.tsx ( router array)
{
 path:  "/register",
 element:  <Register/>,
},
```
```js
//Register.tsx

import {
  Box,
  Button,
  IconButton,
  Snackbar,
  TextField,
  Typography,
} from "@mui/material";
import CloseIcon from "@mui/icons-material/Close";
import { useState } from "react";
import { config } from "../configs/config";
import { Link } from "react-router-dom";

const Register = () => {
  const [open, setOpen] = useState(false);
  const [user, setUser] = useState({ name: "", email: "", password: "" });

  const Register = async () => {
    const isValid =
      user.name.length > 0 && user.email.length > 0 && user.password.length > 0;
    if (!isValid) return setOpen(true);
   
  };

  const handleClose = (
    event: React.SyntheticEvent | Event,
    reason?: string
  ) => {
    if (reason === "clickaway") {
      return;
    }
    setOpen(false);
  };

  const action = (
    <>
      <IconButton
        size="small"
        aria-label="close"
        color="inherit"
        onClick={handleClose}
      >
        <CloseIcon fontSize="small" />
      </IconButton>
    </>
  );

  return (
    <Box
      sx={{
        display: "flex",
        justifyContent: "center",
      }}
    >
      <Snackbar
        open={open}
        autoHideDuration={3000}
        onClose={handleClose}
        message="Please enter email and password"
        action={action}
        anchorOrigin={{ vertical: "bottom", horizontal: "right" }}
      />
      <Box
        sx={{
          display: "flex",
          flexDirection: "column",
          maxWidth: 400,
          minWidth: 400,
          mt: 5,
        }}
      >
        <TextField
          label="Name"
          variant="outlined"
          sx={{ mb: 2 }}
          onChange={(evt) => setUser({ ...user, name: evt.target.value })}
        />
        <TextField
          label="Email"
          variant="outlined"
          sx={{ mb: 2 }}
          onChange={(evt) => setUser({ ...user, email: evt.target.value })}
        />
        <TextField
          label="Password"
          variant="outlined"
          type="password"
          sx={{ mb: 2 }}
          onChange={(evt) => setUser({ ...user, password: evt.target.value })}
        />
        <Box
          sx={{
            display: "flex",
            alignItems: "center",
            justifyContent: "center",
            flexDirection: "column",
            mt: 5,
          }}
        >
          <Button variant="contained" onClick={Register}>
            Register
          </Button>
          <Link to="/login">
            <Typography variant="body1" sx={{ mt: 2 }}>
              Login
            </Typography>
          </Link>
        </Box>
      </Box>
    </Box>
  );
};

export default Register;

```
## 
## Express server မှာ request တွေကို လက်ခံဖို့ အရင် ပြင်ဆင်ပါမယ်
```js
// Add to backend/server.ts

app.post("/auth/login", async (req: Request, res: Response) => {
  const { email, password } = req.body;
  const isValid = email && email.length > 0 && password && password.length > 0;
  if (!isValid) return res.sendStatus(400);
  const result= await db.query("select * from users where email=$1" and password = $2, [email,password]);
  
  res.send(result.rows);
});

app.post("/auth/register", async (req: Request, res: Response) => {
  const { name, email, password } = req.body;
  const isValid =
    name &&
    name.length > 0 &&
    email &&
    email.length > 0 &&
    password &&
    password.length > 0;
  if (!isValid) return res.send({ error: "Name and password are required." });
  const result = await db.query(
    "select * from users where email=$1 and password=$2",
    [email, password]
  );
  if (result.rows.length) res.send({ message: "User already exists." });



  const newUser = await db.query(
    "insert into users (name, email, password) values($1, $2, $3) RETURNING *",
    [name, email, password]
  );
  
  res.send(newUser);
});

```
### Database မှာလည်း users table တစ်ခု လုပ်ပါမယ်။
```sql
CREATE TABLE users (
id serial PRIMARY KEY,
name text NOT NULL,
eamil text UNIQUE NOT NULL,
password text NOT NULL
)
```
- ဆက်ပြီး Log in button ကို နှိပ်လိုက်တဲ့အခါ နဲ့ Log in button ကို နှိပ်လိုက်တဲ့အခါ ခေါ်ထားတဲ့  function တွေမှာserver ဆီ request လုပ်ပါမယ်။

![enter image description here](https://github.com/Aungmyanmar32/msquare-m5-summaries/blob/main/Screenshot%202023-05-14%20144445.png?raw=true)

![enter image description here](https://github.com/Aungmyanmar32/msquare-m5-summaries/blob/main/Screenshot%202023-05-14%20144559.png?raw=true)

- `(aung , aung@web.de , 1234 )`new user တစ်ယောက် register လုပ်ကြည့်ပါမယ်

![enter image description here](https://github.com/Aungmyanmar32/msquare-m5-summaries/blob/main/Screenshot%202023-05-14%20144902.png?raw=true)

- Register လုပ်လိုက်တဲ့အခါ server က response ပြန်လာတဲ့ log ကို မြင်ရမှာဖြစ်ပါတယ်
- အဲ့ဒီ User ကို login ၀င်ကြည့်ပါမယ်

![enter image description here](https://github.com/Aungmyanmar32/msquare-m5-summaries/blob/main/Screenshot%202023-05-14%20145005.png?raw=true)
 - email နဲ့ password ကို မှန်အောင် ရိုက်ထည့်ပြီး login ၀င်ကြည့်ပါက server က response ပြန်လာတဲ့ user info ကို log ထုတ်ပေးမှာဖြစ်ပါတယ်။
 - အချက်အလက် မမှန်ခဲ့ရင်တော့ 400 ( Bad requset) ကို network မှာ response ပြပေးမှာဖြစ်ပါတယ်။
 - ဆက်ပြီးတော့ login ၀င်တာမှန်ခဲ့ရင် react **useNavigate** hook ကိုသုံးပြီး settings page ဆီ Navigate လုပ်ပေးလိုက်ပါမယ်။

![enter image description here](https://github.com/Aungmyanmar32/msquare-m5-summaries/blob/main/Screenshot%202023-05-14%20150452.png?raw=true)
- ခု login ၀င်ကြည့်လိုက်တာနဲ့ settings page ကို ရောက်သွားတာမြင်ရမှာဖြစ်ပါတယ်။
