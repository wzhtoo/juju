# ၃။ Relations & Integration (The Wiring)

Relations ဆိုတာက Juju ရဲ့ တကယ်ကို "Nitty-Gritty" အကျဆုံးနဲ့ အစွမ်းထက်ဆုံး အစိတ်အပိုင်းပါပဲ။ သူ့ရဲ့ အလုပ်လုပ်ပုံက ဒီလိုပါ။

#### ၁။ ချိတ်ဆက်ရန် အမိန့်ပေးခြင်း

User က `juju relate wordpress mysql` လို့ အမိန့်ပေးလိုက်တဲ့အခါ၊ Controller (Manager) က ဒီ Application နှစ်ခုကို ချိတ်ဆက်ဖို့ လိုပြီဆိုတာကို သိရှိသွားပါတယ်။

#### ၂။ Hook များကို Trigger လုပ်ခြင်း

Controller က ဒီအချက်အလက်ကို Database ထဲမှာ မှတ်တမ်းတင်လိုက်ပြီး WordPress Agent နဲ့ MySQL Agent နှစ်ခုလုံးကို သက်ဆိုင်ရာ **Hook** တွေ Run ဖို့ အမိန့်ပေးပါတယ်။

- MySQL Charm Agent က **`relation-joined`** hook ကို run ပြီး သူ့ရဲ့ Username/Password ကို Controller ရဲ့ Database ထဲကို **ရေးသား** လိုက်ပါတယ်။
- WordPress Charm Agent ကလည်း **`relation-changed`** hook ကို run ပြီး Controller ရဲ့ Database ထဲက MySQL က ရေးထားတဲ့ Username/Password ကို **ပြန်လည်ဖတ်ယူ** လိုက်ပါတယ်။

#### ၃။ အလိုအလျောက် ပြီးမြောက်ခြင်း

WordPress Charm Code ထဲမှာပါတဲ့ Logic က ဒီရလာတဲ့ Username/Password ကိုယူပြီး သူ့ရဲ့ WordPress Config File ထဲမှာ အလိုအလျောက် သွားပြင်ပေးလိုက်တာပဲ ဖြစ်ပါတယ်။

- **ရလဒ်:** လူသားက ဘာ Configuration မှ လိုက်လုပ်စရာ မလိုဘဲ၊ Juju ရဲ့ Charms နဲ့ Relations စနစ်က Database ကို ရှာဖွေတာ၊ Password ယူတာ၊ Web Server Config ကို ပြင်တာ စတဲ့ အဆင့်အားလုံးကို အလိုအလျောက် ပြီးမြောက်စေပါတယ်။
