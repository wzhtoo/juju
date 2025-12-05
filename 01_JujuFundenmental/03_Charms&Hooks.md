# ၂။ Charms & Hooks (Application Logic)

Juju ရဲ့ အဓိက ခြားနားချက်က Application တွေကို ကိုယ်တိုင် Configuration တွေ လိုက်မရေးရဘဲ **Charm** လို့ခေါ်တဲ့ Package လေးတွေနဲ့ စီမံခန့်ခွဲတာပါပဲ။

#### Charm ဆိုတာ ဘာလဲ?

**Charm** ဆိုတာ Application တစ်ခုကို Cloud ပေါ်မှာ **ဘယ်လို စတင်အလုပ်လုပ်ရမယ်၊ ဘယ်လို ပြုပြင်ထိန်းသိမ်းရမယ်၊ ဘယ်လို ချိတ်ဆက်ရမယ်** ဆိုတဲ့ လူရဲ့ ဗဟုသုတ (Operator's Knowledge) ကို Code အဖြစ် ရေးသားထားတာပါ။

- **စံသတ်မှတ်ချက်:** ဥပမာ - WordPress Charm တစ်ခုဟာ WordPress ကို Install လုပ်ဖို့၊ Database နဲ့ ချိတ်ဖို့၊ Version Upgrade လုပ်ဖို့ စတဲ့ အဆင့်အားလုံးကို သူ့အတွင်းမှာ Code အဖြစ် ထည့်သွင်းထားပါတယ်။
- **The Nitty-Gritty Detail:** Charm တွေကို အရင်တုန်းက Shell Script တွေနဲ့ ရေးကြပေမယ့်၊ အခုအခါမှာ ပိုမိုလွယ်ကူပြီး စနစ်ကျတဲ့ **Python-based Operator Framework** ကို အသုံးပြုပြီး ရေးသားကြတာ များလာပါပြီ။

#### Hooks ဆိုတာ ဘာလဲ?

**Hooks** ဆိုတာကတော့ **Agent** (Worker) ကနေ Charm Code တွေကို ခေါ်သုံးတဲ့ **Event** (ဖြစ်ရပ်) တွေပဲ ဖြစ်ပါတယ်။ Agent ဟာ Controller ရဲ့ State (အခြေအနေ) ပြောင်းတာနဲ့ သက်ဆိုင်ရာ Hook ကို ချက်ချင်း ဝင်ပြီး Run ပါတော့တယ်။

ဒါက Juju Operator ရေးသားတဲ့အခါ အရေးအကြီးဆုံး အစိတ်အပိုင်းတွေပါ။ အဓိက Hooks တွေကတော့-

| Hook Name             | ဘယ်အချိန်မှာ အလုပ်လုပ်သလဲ                                                        | ဥပမာ                                                                       |
| :-------------------- | :------------------------------------------------------------------------------- | :------------------------------------------------------------------------- |
| **`install`**         | Application ကို ပထမဆုံးအကြိမ် စတင် Install လုပ်တဲ့အခါ                            | `apt update` လုပ်တာ၊ Software Files တွေ ဒေါင်းလုပ်ဆွဲတာ                    |
| **`config-changed`**  | User က Application ရဲ့ Configuration (ဥပမာ - Port Number) ကို ပြောင်းလိုက်တဲ့အခါ | Web Server ရဲ့ Config File ကို ပြန်လည်ပြင်ဆင်ပြီး Service ကို Restart ချတာ |
| **`upgrade-charm`**   | Charm Code Version အသစ်ကို Deploy လုပ်တဲ့အခါ                                     | Application ကို မထိခိုက်စေဘဲ Operator Logic ကို အဆင့်မြှင့်တင်တာ           |
| **`relation-joined`** | Application အချင်းချင်း စတင်ချိတ်ဆက်တဲ့အခါ (နောက်အပိုင်းအတွက်)                   | Database က Web Server ကို ချိတ်ဆက်ဖို့ လာပြောတဲ့အခါ                        |