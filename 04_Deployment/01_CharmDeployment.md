## Charm Deployment (Applications)

**Juju** ရဲ့ **Core Automation Logic** ဖြစ်တဲ့ **Charm Deployment** နဲ့ **Relation** ကို လက်တွေ့ စတင်လေ့လာကြပါစို့။

### အဆင့် ၁: Applications (Charms) များကို Deploy လုပ်ခြင်း

| Command | မှတ်ချက် |
| :--- | :--- |
| **Grafana Deploy ရန်** | `juju deploy grafana-k8s` | **Grafana Operator** ကို Deploy လုပ်ခြင်း။ |
| **PostgreSQL Deploy ရန်** | `juju deploy postgresql-k8s --channel 14/stable --trust` | **PostgreSQL Operator** ကို Deploy လုပ်ခြင်း။ |
| **Deploy Status စစ်ဆေးရန်** | `juju status` | **Applications** နှစ်ခုလုံး **"Active/Idle"** ဖြစ်လာဖို့ စောင့်ဆိုင်းပါ။ |

---

## Juju Grafana ဆိုတာ ဘာလဲ။

**Grafana** ဆိုတာက **Monitoring (စောင့်ကြည့်စစ်ဆေးခြင်း)** နဲ့ **Data Visualization (အချက်အလက်များကို ပုံဖော်ပြသခြင်း)** အတွက် အလွန်ကျော်ကြားတဲ့ Open Source Tool တစ်ခု ဖြစ်ပါတယ်။

Juju ရဲ့ အခြေအနေမှာ Grafana ကို ဘာအတွက် အသုံးပြုလဲဆိုရင်-

1.  **Deployment (ဖြန့်ချီခြင်း):**
    Juju ရဲ့ အားသာချက်ဖြစ်တဲ့ **Charm** (Operator) ကို အသုံးပြုပြီး Grafana ကို Kubernetes (MicroK8s) ဒါမှမဟုတ် Cloud ပေါ်မှာ **အလွယ်တကူ ထည့်သွင်း (deploy) နိုင်ပါတယ်**။ ဒီလို ထည့်သွင်းတဲ့ operator ကို **`grafana-k8s` charm** လို့ ခေါ်ပါတယ်။

    ```bash
    juju deploy grafana-k8s
    ```

2.  **Integration (ပေါင်းစပ်ခြင်း):**
    Grafana ဟာ သူ့ချည်းသက်သက်ဆို အသုံးဝင်မှာ မဟုတ်ပါဘူး။ သူ့ရဲ့ အဓိက အခန်းကဏ္ဍက အခြားသော Application တွေ၊ Service တွေက လာတဲ့ **Metrics (စွမ်းဆောင်ရည် အချက်အလက်)** တွေကို ဖတ်ပြီး၊ **Dashboard (အချက်အလက်ပြ ကဏ္ဍ)** တွေနဲ့ ပုံဖော်ပြသဖို့ ဖြစ်ပါတယ်။

      * Juju ရဲ့ **Relation** စနစ်က ဒီအလုပ်ကို လုပ်ဆောင်ပေးပါတယ်။ ဥပမာ-
          * **Prometheus** (Metrics တွေကို စုဆောင်းသိမ်းဆည်းတဲ့ tool) ကို deploy လုပ်ပြီး၊
          * **Grafana** နဲ့ **`juju relate`** command ကို သုံးပြီး ချိတ်ဆက်လိုက်ရင်၊
          * Grafana က Prometheus ကနေ metrics တွေကို အလိုအလျောက် ရယူပြီး စောင့်ကြည့်စစ်ဆေးနိုင်ပါတယ်။

3.  **Observability Stack (စောင့်ကြည့်မှု အစုအဝေး):**
    Grafana ဟာ Canonical ရဲ့ **Canonical Observability Stack (COS Lite)** လို့ခေါ်တဲ့ စောင့်ကြည့်မှု အစုအဝေးရဲ့ မရှိမဖြစ် အစိတ်အပိုင်းတစ်ခု ဖြစ်ပါတယ်။ ဒီ Stack မှာ Grafana အပြင် Prometheus (metrics) နဲ့ Loki (logs) တို့ ပါဝင်ပြီး စနစ်တစ်ခုလုံးကို အပြည့်အဝ စောင့်ကြည့်နိုင်အောင် Juju ရဲ့ Operator တွေက စီမံခန့်ခွဲပေးပါတယ်။

### အဓိက အချက်

**Juju က Grafana ကို** သူ့ရဲ့ **Charmed Operator** စနစ်ကို သုံးပြီး **အလိုအလျောက် ထည့်သွင်းခြင်း၊ ချိတ်ဆက်ခြင်းနဲ့ စီမံခန့်ခွဲခြင်း** တို့ကို လုပ်ဆောင်ပေးနိုင်တဲ့ **Application** တစ်ခုလို့ မှတ်ယူနိုင်ပါတယ်။

## PostgreSQL

# `juju deploy postgresql` command ရဲ့ အဓိပ္ပာယ်နဲ့ လုပ်ဆောင်ပုံ

## `juju deploy postgresql` ဆိုတာ ဘာလဲ။

ဒီ command က **PostgreSQL** (ကမ္ဘာကျော် open-source Database တစ်ခု) ကို သင်ရဲ့ Juju Model (MicroK8s သို့မဟုတ် Cloud) ပေါ်မှာ **အလိုအလျောက် ထည့်သွင်းပြီး စီမံခန့်ခွဲဖို့** အမိန့်ပေးလိုက်တာပဲ ဖြစ်ပါတယ်။

ဒီ command မှာ အဓိက အစိတ်အပိုင်း (၂) ခု ပါဝင်ပါတယ်-

| အမိန့် | ရှင်းလင်းချက် |
| :--- | :--- |
| `juju deploy` | Juju ကို **Application (Charm)** တစ်ခုကို ယူပြီး သင်ရွေးချယ်ထားတဲ့ Infrastructure (ဥပမာ- MicroK8s cluster) ပေါ်မှာ **ဖြန့်ချီ (Deploy)** လုပ်ခိုင်းတဲ့ အမိန့်ဖြစ်ပါတယ်။ |
| `postgresql` | ဖြန့်ချီဖို့ လိုအပ်တဲ့ Application ရဲ့ နာမည်ပါ။ ဒီနေရာမှာ **Charmed PostgreSQL Operator** (ခေါ်) **Charm** ကို ရည်ညွှန်းပါတယ်။ |

### Charmed PostgreSQL Operator (Charm)

  * **Charm** ဆိုတာ PostgreSQL ကို **ဘယ်လို ထည့်သွင်းရမယ်၊ စီမံခန့်ခွဲရမယ်၊ Scale လုပ်ရမယ်၊ Upgrade လုပ်ရမယ်** စတဲ့ လုပ်ဆောင်မှုဆိုင်ရာ အသိဉာဏ် (Operational Logic) တွေ အားလုံးကို ထည့်သွင်းထားတဲ့ Software Package တစ်ခုဖြစ်ပါတယ်။
  * သင်က `juju deploy postgresql` လို့ ရိုက်လိုက်တဲ့အခါ Juju က **Charmhub** (Canonical ရဲ့ Operator Store) ကနေ ဒီ PostgreSQL Charm ကို သွားရောက် ရှာဖွေပြီး ယူလာပါတယ်။

### Command တစ်ကြောင်းရဲ့ အကျိုးဆက်များ

သင် `juju deploy postgresql` လို့ ရိုက်လိုက်တာနဲ့ Juju Controller က အောက်ပါ အလုပ်တွေကို **အလိုအလျောက်** စတင် လုပ်ဆောင်ပါတယ်-

1.  **Infrastructure Provisioning:**
      * PostgreSQL ကို လည်ပတ်ဖို့အတွက် လိုအပ်တဲ့ **Container/Pod** (MicroK8s ပေါ်မှာဆိုရင်) သို့မဟုတ် **Virtual Machine** (Cloud ပေါ်မှာဆိုရင်) ကို ဖန်တီးဖို့ Underlying Infrastructure (MicroK8s/Cloud) ကို ပြောကြားပါတယ်။
2.  **Application Installation:**
      * အဲဒီ Container/VM ထဲမှာ PostgreSQL Software ကို **install** လုပ်ပါတယ်။
3.  **Configuration & Best Practices:**
      * PostgreSQL ကို Production Environment အတွက် သင့်တော်အောင် **Configuration** (ဖွဲ့စည်းပုံ) တွေကို အကောင်းဆုံး လုပ်ဆောင်ပေးပါတယ်။ (ဥပမာ- Database Cluster စနစ်၊ Replication စနစ်တွေကို အလိုအလျောက် စီစဉ်ပေးပါတယ်။)
4.  **Operator Running:**
      * PostgreSQL ရဲ့ Life Cycle Management (သက်တမ်း စီမံခန့်ခွဲမှု) ကို လုပ်ဆောင်ပေးမယ့် **Operator (Charm)** က စတင် လည်ပတ်နေပြီး Database ရဲ့ အခြေအနေကို စောင့်ကြည့်ပါတယ်။

### ဥပမာ (MicroK8s အတွက်)

MicroK8s ပေါ်မှာဆိုရင်၊ အများအားဖြင့် **Kubernetes Version** ကို ရည်ညွှန်းတဲ့ Charm ကို ရွေးချယ်ဖို့အတွက် အောက်ပါအတိုင်း Channel (ဗားရှင်းလမ်းကြောင်း) ကို ထည့်သုံးဖို့ လိုအပ်တတ်ပါတယ်-

```bash
juju deploy postgresql-k8s --channel 14/stable --trust
```

  * `postgresql-k8s`: Kubernetes ပေါ်မှာ အသုံးပြုတဲ့ Charm ကို အတိအကျ ဖော်ပြတာပါ။
  * `--channel 14/stable`: PostgreSQL ဗားရှင်း 14 ရဲ့ Stable (တည်ငြိမ်) ဗားရှင်းကို ရွေးချယ်တာပါ။
  * `--trust`: ဒီ Operator က Kubernetes Cluster ရဲ့ အရင်းအမြစ်အချို့ (Resources) ကို စီမံခန့်ခွဲခွင့် ပေးလိုက်တာပါ။

-----

# PostSQL Deployment

**WSL (Windows Subsystem for Linux)** ကို သုံးပါတယ်ဆိုရင်၊ အဲဒါက တော်တော်လေး အရေးကြီးတဲ့ အချက်ပါ။

WSL ပေါ်မှာ Juju ကို သုံးတဲ့အခါ၊ သင်ဟာ Linux Environment ထဲမှာ ရောက်နေတာ ဖြစ်ပေမယ့်၊ သင်ရဲ့ **Juju Model** (Application တွေ ထားရှိမယ့်နေရာ) ကို ဘယ်မှာ ချိတ်ဆက်ထားလဲဆိုတဲ့အပေါ်မှာ မူတည်ပြီး Command ကို ရွေးချယ်ရပါလိမ့်မယ်။

### WSL ပေါ်မှာ Juju ကို အသုံးပြုပုံ

WSL ထဲမှာ Juju ကို စမ်းသပ်လေ့လာသူအများစုက များသောအားဖြင့် **MicroK8s** ကိုပဲ Model အနေနဲ့ အသုံးပြုကြပါတယ်ခင်ဗျာ။

| Model Type | Command | ရည်ညွှန်းသော Charm |
| :--- | :--- | :--- |
| **MicroK8s Cluster** (WSL ထဲမှာ install လုပ်ထားသော MicroK8s) | `juju deploy postgresql-k8s --channel 14/stable --trust` | **Kubernetes Charm** (`-k8s`) |
| **LXD Container** (WSL ထဲမှာ install လုပ်ထားသော LXD) | `juju deploy postgresql` | **VM Charm** |
| **Cloud (AWS, Azure, etc.)** | `juju deploy postgresql` | **VM Charm** |

### အများဆုံးသုံးရမယ့် Command

**MicroK8s** ကို ရည်ညွှန်းခဲ့တာကြောင့်၊ သင်ရဲ့ Juju Controller ဟာ WSL ထဲမှာရှိတဲ့ MicroK8s Cluster ကို ချိတ်ဆက်ထားတဲ့ **Model** တစ်ခုကို သုံးနေပါတယ်။

ဒါကြောင့်၊ သင် deploy လုပ်ရမယ့် command က **Kubernetes Charm** ကို သုံးတဲ့ အောက်ပါအတိုင်းပဲ ဖြစ်ပါတယ်-

```bash
juju deploy postgresql-k8s --channel 14/stable --trust
```

-----

### စစ်ဆေးရန် အဆင့်များ

deploy မလုပ်ခင်၊ သင်ရဲ့ Juju Model က ဘာဖြစ်လဲဆိုတာကို အောက်ပါ command နဲ့ အမြန်ဆုံး စစ်ဆေးနိုင်ပါတယ်-

1.  **လက်ရှိ Juju Model ကို စစ်ဆေးခြင်း:**
    ```bash
    juju status
    ```
      * **Output ထဲမှာ `Controller` အောက်မှာ `k8s` သို့မဟုတ် `microk8s` လို့ တွေ့ရင်၊** သင်ဟာ Kubernetes Model ကို သုံးနေတာ ဖြစ်ပါတယ်။ (Kubernetes Charm ကို သုံးရပါမယ်။)
      * **Output ထဲမှာ `Controller` အောက်မှာ `lxd` သို့မဟုတ် Cloud Provider နာမည် တွေ့ရင်၊** သင်ဟာ VM Model ကို သုံးနေတာ ဖြစ်ပါတယ်။ (VM Charm ကို သုံးရပါမယ်။)

အများစုက WSL မှာ MicroK8s ကိုသုံးတာကြောင့်၊ ပထမ Command ကိုသာ သုံးပါ။


**အချုပ်အားဖြင့်-**

`juju deploy postgresql-k8s --channel 14/stable --trust` ဆိုတာဟာ **PostgreSQL Database ကို Production Grade စနစ်နဲ့ အလိုအလျောက် ထည့်သွင်းပြီး စီမံခန့်ခွဲပေးမယ့် Operator (Charm)** ကို သင်ရဲ့ Infrastructure ပေါ်မှာ ချက်ချင်း လွှတ်တင်ပေးလိုက်တဲ့ အမိန့်ဖြစ်ပါတယ်။


# juju status explaination

```markdown
uyanpi@WinZawHtoo:~$ juju status
Model        Controller           Cloud/Region    Version  SLA          Timestamp
my-juju-lab  microk8s-controller  microk8s-cloud  3.6.12   unsupported  07:59:18Z

App             Version  Status  Scale  Charm           Channel    Rev  Address         Exposed  Message
grafana-k8s     9.5.21   active      1  grafana-k8s     1/stable   160  10.152.183.180  no
postgresql-k8s  14.15    active      1  postgresql-k8s  14/stable  495  10.152.183.209  no

Unit               Workload  Agent  Address     Ports  Message
grafana-k8s/0*     active    idle   10.1.58.20
postgresql-k8s/0*  active    idle   10.1.58.19         Primary
uyanpi@WinZawHtoo:~$
```

**`juju status`** ရဲ့ Output ဟာ **Juju Environment** ရဲ့ **Health** နဲ့ **Deployment Details** ကို **အသေးစိတ်** ပြသနေတာ ဖြစ်ပါတယ်။

## Juju Status Output အသေးစိတ် ရှင်းလင်းချက်

### ၁။ Model အချက်အလက် (Overall Environment)

ဒီအပိုင်းဟာ သင်လက်ရှိအသုံးပြုနေတဲ့ **Working Space** ရဲ့ အခြေအနေကို ပြသပါတယ်။

| Field | Value | အဓိပ္ပာယ် |
| :--- | :--- | :--- |
| **Model** | `my-juju-lab` | သင်ဖန်တီးထားသော လက်ရှိ **Working Space** နာမည်။ |
| **Controller** | `microk8s-controller` | ဒီ Model ကို **ထိန်းချုပ်** နေတဲ့ **Controller** နာမည်။ |
| **Cloud/Region** | `microk8s-cloud` | Deploy လုပ်ထားသော **Cloud Provider** (သင်၏ Local **MicroK8s Kubernetes Cluster** ကို ရည်ညွှန်းသည်)။ |
| **Version** | `3.6.12` | အသုံးပြုနေသော **Juju Version**။ |
| **Timestamp** | `07:59:18Z` | **Status Update** လုပ်ခဲ့သော **အချိန်**။ |

---

### ၂။ Application အချက်အလက် (Charms)

ဒီအပိုင်းဟာ **Deploy** လုပ်ထားတဲ့ **Applications** နှစ်ခုရဲ့ **Overview** ကို ပြသပါတယ်။

ဤဇယားသည် Juju Model ပေါ်တွင် ဖြန့်ချီထားသော **Grafana** နှင့် **PostgreSQL** Application (Charm) နှစ်ခု၏ အခြေအနေနှင့် အသေးစိတ်အချက်အလက်များကို ပြသထားပါသည်။

| အသေးစိတ်အချက်အလက် | Grafana-k8s | PostgreSQL-k8s | အဓိပ္ပာယ် ဖွင့်ဆိုချက် |
| :--- | :--- | :--- | :--- |
| **Software Version** | 9.5.21 | 14.15 | Operator မှ စီမံခန့်ခွဲပေးနေသော Software (Grafana/PostgreSQL) ၏ ဗားရှင်းနံပါတ်။ |
| **Deployment Status** | **Active** | **Active** | Application နှစ်ခုလုံးသည် အောင်မြင်စွာ ထည့်သွင်းပြီးစီး၍ **စနစ်တကျ အလုပ်လုပ်နေပြီ** ဖြစ်သည်။ (ကျေနပ်ဖွယ် အကောင်းဆုံး အခြေအနေ) |
| **Scale (Units)** | 1 | 1 | Cluster ပေါ်တွင် လည်ပတ်နေသော Application ၏ **Instance အရေအတွက်** (Unit)။ (တစ်လုံးစီသာ ဖြန့်ချီထားခြင်း) |
| **Charm (Operator)** | `grafana-k8s` | `postgresql-k8s` | 해당 Application ကို ထည့်သွင်း၊ စီမံခန့်ခွဲ၊ ထိန်းသိမ်းပေးနေသော **Juju Operator** (Charm) ၏ နာမည်။ |
| **Internal Cluster IP** | $10.152.183.180$ | $10.152.183.209$ | **Kubernetes Cluster** အတွင်း Application များ အချင်းချင်း ဆက်သွယ်ရန်အတွက် သတ်မှတ်ထားသော **Internal Service IP Address** များ။ |

---

### ၃။ Unit အချက်အလက် (Instances)

ဤဇယားသည် Deploy လုပ်ထားသော **Application များ၏ Instance (Unit) တစ်ခုချင်းစီ** ၏ လက်ရှိ ကျန်းမာရေး အခြေအနေ (Health Status) ကို အသေးစိတ် ပြသထားပါသည်။

| အသေးစိတ်အချက်အလက် | Unit: `grafana-k8s/0*` | Unit: `postgresql-k8s/0*` | အဓိပ္ပာယ် ဖွင့်ဆိုချက် |
| :--- | :--- | :--- | :--- |
| **Unit Identifier** | `grafana-k8s/0*` | `postgresql-k8s/0*` | **Application Name / Unit Index** ကို ဖော်ပြသည်။ `*` သည် လက်ရှိ Console မှ ရွေးချယ်ထားသော Unit ကို ဆိုလိုသည်။ |
| **Workload Status** | **`active`** | **`active`** | Unit အတွင်းရှိ **Core Software** (Grafana/PostgreSQL) သည် လိုအပ်သော Configuration များနှင့်အတူ **ကောင်းမွန်စွာ လည်ပတ်နေခြင်း**။ |
| **Agent Status** | **`idle`** | **`idle`** | Unit ကို စီမံခန့်ခွဲနေသော **Juju Agent** သည် လက်ရှိတွင် အလုပ်လုပ်စရာမရှိဘဲ **Command အသစ်များကို စောင့်ဆိုင်းနေခြင်း**။ (Agent Ready ဖြစ်နေခြင်းကို ဖော်ပြသည်) |
| **Message** | *[မရှိ/No Message]* | **`Primary`** | **Unit ၏ အခြေအနေ သတင်းစကား**။ PostgreSQL တွင် ဤ Unit သည် **Master Node** သို့မဟုတ် **Primary Replica** အဖြစ် ဆောင်ရွက်နေကြောင်း ဖော်ပြသည်။ |



