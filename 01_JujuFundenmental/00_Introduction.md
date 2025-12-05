# Introduction

**Kubernetes** ကို နားလည်ပြီးသားဖြစ်လို့၊ [🔗 microk8s ကို ဒီ repo မှာ ကြည့်ပါ။](https://github.com/wzhtoo/microk8s) **Juju** ကို လေ့လာဖို့ဟာ **အလွန်လွယ်ကူပြီး အချိန်ကုန်သက်သာစေပါတယ်**။ Juju ဟာ **Canonical** ရဲ့ **Automation Core** ဖြစ်လို့ **အရေးအကြီးဆုံး Tool** ဖြစ်ပါတယ်။

### ၁။ Juju ဆိုတာဘာလဲ။

* **အဓိပ္ပာယ်:** **Juju** ဆိုတာဟာ **Infrastructure** (Kubernetes, AWS, Google Cloud, VMWare) တွေပေါ်မှာ **Applications** (ဥပမာ- Database, Monitoring Tools, OpenStack) တွေကို **Deploy** လုပ်ခြင်း၊ **ချိတ်ဆက်ခြင်း** နှင့် **စီမံခန့်ခွဲခြင်း** (Operate) ကို **Automate** လုပ်ပေးတဲ့ **Modeling Tool** ဖြစ်ပါတယ်။
* **ရည်ရွယ်ချက်:** **"Operators"** (သို့မဟုတ်) **"Charms"** လို့ခေါ်တဲ့ **Code Logic** ကိုသုံးပြီး **DevOps** အလုပ်တွေကို **လူသားဝင်ရောက်စွက်ဖက်မှုမရှိဘဲ** အလိုအလျောက် လုပ်ဆောင်ပေးဖို့ ဖြစ်ပါတယ် (Zero-Ops)။

### ၂။ Operator Pattern နှင့် Charm

**Operator Pattern** ဆိုတာ **Juju** ရဲ့ **Core Concept** ဖြစ်ပါတယ်။

| Concept | Kubernetes (K8s) ဖြင့် နှိုင်းယှဉ်ချက် | Juju (Canonical) အဓိပ္ပာယ် |
| :--- | :--- | :--- |
| **Operator** (Charm) | **Deployment/Pod** တွေလို **Application** တစ်ခုတည်းကို **Run** ရုံသာမကဘဲ၊ **Database Setup**၊ **Backup**၊ **Scaling** စတဲ့ **အုပ်ချုပ်ရေးဆိုင်ရာ Logic** ပါ ထည့်သွင်းထားသော Code Package။ | **Application** တစ်ခုကို **ဘယ်လို Install လုပ်ရမလဲ၊ ဘယ်လို Run ရမလဲ၊ ဘယ်လို Manage လုပ်ရမလဲ** ဆိုတာကို သိတဲ့ **"Software Robot"** တစ်ရုပ်နှင့်တူပါတယ်။ |
| **Model** | **Namespace** (သို့မဟုတ်) **Logical Environment** တစ်ခု။ | **Infrastructure** (Kubernetes/Cloud) ပေါ်မှာ **Application များ** ကို တည်ဆောက်မယ့် **Virtual Workspace**။ |
| **Cloud** | **Node** များ စုဝေးရာ။ | **Applications** တွေ Deploy လုပ်မယ့် **Physical Infrastructure** (ဥပမာ- MicroK8s, AWS, Azure)။ |

### ၃။ Juju ၏ အဓိက အားသာချက် (The Magic)

* **Relation (ချိတ်ဆက်မှု):** Juju ဟာ **Applications** (ဥပမာ- Web App နဲ့ Database) နှစ်ခုကို **Command တစ်ခုတည်း** နဲ့ **အလိုအလျောက် ချိတ်ဆက်ပေးနိုင်ပါတယ်**။
    * **ဥပမာ:** `juju relate my-web-app database-operator` ဆိုရင် **Web App** အတွက် လိုအပ်တဲ့ **Database Connection String** (Config) ကို Juju က **အလိုအလျောက် ပေးပို့** ပြီး **ချိတ်ဆက်ပေး** ပါတယ်။
* **Day-2 Operations:** **Upgrade, Backup, Failure Recovery** စတဲ့ **နေ့စဉ် စီမံခန့်ခွဲမှု** တွေကို **Operator** ကနေတစ်ဆင့် **Automate** လုပ်နိုင်ပါတယ်။
