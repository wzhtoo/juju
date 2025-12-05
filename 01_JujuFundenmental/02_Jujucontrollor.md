# Juju Architecture

### ၁။ Juju Controller (The Brain )

Juju ကို စသုံးပြီဆိုတာနဲ့ ပထမဆုံး လိုအပ်တာက **Controller** ပါ။ သူက Juju ရဲ့ "ဦးနှောက်" သို့မဟုတ် "မန်နေဂျာ" လို့ ပြောလို့ရပါတယ်။

- **သူ့ရဲ့ အလုပ်:** Cloud (AWS, Azure, etc.) ပေါ်မှာ Server တွေ ဖန်တီးတာ၊ ဖျက်တာ၊ ဘယ် Server မှာ ဘာ run နေလဲဆိုတာကို မှတ်သားထားတာပါ။
- **The Nitty-Gritty:** Controller ဟာ အထဲမှာ **MongoDB** Database ကို အသုံးပြုပြီး အရာအားလုံးရဲ့ အခြေအနေ (State) ကို သိမ်းဆည်းထားပါတယ်။ ဥပမာ - Web Server ရဲ့ IP က ဘာလဲ၊ Database နဲ့ ချိတ်ထားလား ဆိုတာမျိုးပေါ့။

### ၂။ Juju Agent (The Worker)

Controller က မန်နေဂျာဆိုရင် **Agent** ကတော့ လုပ်သား (Worker) ပါ။ Juju ကနေ Server အသစ်တစ်လုံး (Machine) ဆောက်လိုက်တိုင်း အဲဒီ Server ထဲမှာ `jujud` ဆိုတဲ့ Agent လေးတစ်ခုကို အရင်ဆုံး သွားထည့်ပါတယ်။

- **သူ့ရဲ့ အလုပ်:** Controller ဆီက အမိန့်ကို စောင့်နေပါတယ်။ Controller က "Web Server တင်လိုက်" လို့ ခိုင်းရင် Agent က သူ့စက်ထဲမှာ Web Server ကို တင်ပေးပါတယ်။

---
