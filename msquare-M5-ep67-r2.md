## MSquare Programing Fullstack Course
### Episode-*67* 
### Summary For `Room(2)`
## JOIN in SQL
![enter image description here](https://learnsql.com/blog/learn-and-practice-sql-joins/2.png)

 
## how to join tables
### Syntax
```sql
select * from table_1
join_method table_2 on table_2.FK = table_1.PK
```
### Usage
```sql
select users.name from users
inner join photos on photos.user_id = users.id
```
- အခု join တွေကို လက်တွေ့ စမ်းသပ်ကြည့်ပါမယ်။
### inner join
https://www.sqlteaching.com/#!joins

> Can you use an inner join to pair each character name  with the actor
> who plays them?   
> Select the columns:   
> **character**.*name*,  
> **character_actor**.*actor_name*


- inner join ကို သုံးပြီး **character** နာမည်နဲ့ **actor** နာမည်ကို select လုပ်ပြီး ပြခိုင်းတာဖြစ်ပါတယ်။
```sql
SELECT character.name,character_actor.actor_name 
FROM character
INNER JOIN character_actor 
ON character_actor.character_id = character.id
```

-
-  character table ထဲက id နဲ့ character_actor table ထဲက character_id တူတဲ့ အပေါ် မူတည်ပြီး inner join နဲ့ join လိုက်ပြီး
-  character table ထဲက name နဲ့ character_actor table ထဲက actor name ကို select လုပ်လိုက်တာဖြစ်ပါတယ်။

### multiple join
https://www.sqlteaching.com/#!multiple_joins
- အပေါ်က link ရဲ့ lesson မှာတော့
>Can you use two joins to pair each character name with the actor who plays them? Select the columns: **character**._name_, **actor**._name_
- join နှစ်ခါ လုပ်ပြီး actor name နဲ့ ဇာတ်ကောင် ကို တွဲပေးရမှာဖြစ်ပါတယ်။
```sql
SELECT character.name,actor.name
FROM character
INNER JOIN character_actor
ON 	character.id = character_actor.character_id
INNER JOIN actor 
ON character_actor.actor_id = actor.id
```

- character_actor ထဲက character_id နဲ့ character ထဲက id တူတဲ့ဟာကို အရင်စစ်ပြီး join လိုက်ပါတယ်
- ဆက်ပြီး actor ထဲက id နဲ့ character_actor ထဲ က actor_id တူတဲ့ဟာကို ထပ်စစ်ပြီး join ပါတယ်
- အဲ့ဒီ join တွေထဲက character.name နဲ့ actor.name ကို ရွေးထုတ်လိုက်တာဖြစ်ပါတယ်
##
### join multiple with where
https://www.sqlteaching.com/#!joins_with_where
>Can you return a list of characters and TV shows that are not named "Willow Rosenberg" and not in the show "How I Met Your Mother"?
- Willow Rosenberg ဆိုတဲ့ ဇာတ်ကောင်မပါတဲ့character  နဲ့ How I Met Your Mother ဆိုတဲ့ show မပါတဲ့  Show ကို ပြခိုင်းတာဖြစ်ပါတယ်။
```sql
SELECT character.name,tv_show.name
FROM character
INNER JOIN character_tv_show
ON character.id = character_tv_show.character_id
INNER JOIN tv_show 
ON character_tv_show.tv_show_id = tv_show.id
WHERE character.name != 'Willow Rosenberg' 
AND tv_show.name != 'How I Met Your Mother'
```
- character_tv_show ထဲက character_id နဲ့ characterထဲက id တူတဲ့ဟာကို အရင်join ပါတယ်
- ထပ်ပြီး character_tv_show ထဲက tv_show_id  နဲ့  tv_showထဲက id တူတဲ့ဟာကိုလည်း ထပ်join ပါတယ်။
- အဲ့ဒီ join လိုက်တဲ့ အထဲကမှ Willow Rosenberg ဆိုတဲ့ character name နဲ့ How I Met Your Mother ဆိုတဲ့ show မပါတဲ့ ဟာတွေကို ရွေးထုတ်လိုက်တာဖြစ်ပါတယ်
##
### Left join
https://www.sqlteaching.com/#!left_joins
>Can you use left joins to match **character names** with the **actors** that play them? 
- Left join ကို သုံးပြီး ဘယ် actor က ဘယ်လို ဇာတ်ကောင်နေရာ သရုပ်ဆောင်တယ်ဆိုတာကို join လုပ်ခိုင်းတာဖြစ်ပါတယ်။
```sql
SELECT character.name,actor.name
FROM character
LEFT JOIN character_actor 
ON character.id = character_actor.character_id
LEFT JOIN actor
ON  character_actor.actor_id = actor.id
```
- character_actor ထဲက character_id နဲ့ character ထဲက id တူတဲ့ဟာကို အရင်စစ်ပြီး join လိုက်ပါတယ်
- ဆက်ပြီး actor ထဲက id နဲ့ character_actor ထဲ က actor_id တူတဲ့ဟာကို ထပ်စစ်ပြီး join ပါတယ်
- အဲ့ဒီ join တွေထဲက character.name နဲ့ actor.name ကို ရွေးထုတ်လိုက်တာဖြစ်ပါတယ်
- ***left join နဲ့ inner join နဲ့ မတူတာက*** 
- **inner join** က table နှစ်ခု **join တဲ့ အချက်ပေါ် မူတည်ပြီး table နှစ်ခုထဲက မှ သက်ဆိုင်တဲ့ data တွေကိုပဲ** ပြပေးတာဖြစ်ပြီး
- **left join** ကတော့  **ပထမ table(left) ရှိ select လုပ်ထားတဲ့ column ထဲက data တွေကို အကုန်ပြပေးမှာဖြစ်ပြီး** join တဲ့ အချက်ပေါ် မူတည်ကာ ဒုတိယ table က data ကို ပြပေးမှာဖြစ်ပါတယ်။ ပ**ထမ table ရှိ data နဲ့ သက်ဆိုင်တဲ့ data တွေ  ဒုတိယ table မှာ မရှိပါက တန်ဖိုး null အဖြစ်ပြပေးမှာဖြစ်ပါတယ်**။
##
