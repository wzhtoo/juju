# Juju Installation

**Juju** ကို  **WSL Ubuntu** ပေါ်မှာ စတင်လေ့လာဖို့အတွက် **Install** လုပ်ရပါမယ်။

| Step | Command | မှတ်ချက် |
| :--- | :--- | :--- |
| **1. Install Juju** | `sudo snap install juju --classic` | **Juju CLI** (Command Line Interface) ကို **Snap** ဖြင့် သွင်းခြင်း။ |
| **2. Add MicroK8s as a Cloud** | `juju add-k8s microk8s-cloud --client` | **MicroK8s** ကို **Juju Deploy Target** (Cloud) အဖြစ် သတ်မှတ်ပေးခြင်း။ |

## Installation Error

## Juju Connection Error ရှင်းလင်းခြင်း

```markdown
uyanpi@WinZawHtoo:~$ juju add-k8s microk8s-cloud --client

Since Juju 3 is being run for the first time, it has downloaded the latest public cloud information.

ERROR making juju admin credentials in cluster: invalid configuration: no configuration has been provided, try setting KUBERNETES_MASTER environment variable

uyanpi@WinZawHtoo:~$
```

**Error:** `ERROR making juju admin credentials in cluster: invalid configuration: no configuration has been provided`

**အဓိပ္ပာယ်:**

* **Juju** ဟာ **MicroK8s Cluster** ကို **Control** လုပ်ဖို့အတွက် **`kubectl`** ရဲ့ **Configuration File** (kubeconfig) ကို ရှာမတွေ့တာ ဒါမှမဟုတ် **Access** မလုပ်နိုင်တာ ဖြစ်ပါတယ်။
* **MicroK8s** ကို **Snap** နဲ့ သွင်းထားရင် **၎င်းရဲ့ Config** ကို **Default Kubernetes Location** မှာ မထားပါဘူး။

## ဖြေရှင်းနည်း (Solution)

**Juju** ကို **MicroK8s** ရဲ့ **Configuration** ကို **ဘယ်မှာရှာရမလဲ** ဆိုတာကို **ပထမဆုံး** ပြောပြပေးဖို့ လိုအပ်ပါတယ်။

### အဆင့် ၁: MicroK8s Kubeconfig File ကို ရယူခြင်း

**Juju** သုံးမယ့် **Kubeconfig File** ကို **Temporary File** အနေနဲ့ ထုတ်ပေးဖို့ လိုပါတယ်။

| Command | မှတ်ချက် |
| :--- | :--- |
| `microk8s config > ~/.kube/config` | **MicroK8s** ရဲ့ **Access Config File** ကို **Standard Kubernetes Location** (သင့်ရဲ့ Home Directory အောက်ရှိ `.kube/config`) ကို **Copy** လုပ်ပေးလိုက်ခြင်း။ |

```markdown
uyanpi@WinZawHtoo:~$ microk8s config > ~/.kube/config
uyanpi@WinZawHtoo:~$ juju add-k8s microk8s-cloud --client

ERROR   No recommended storage configuration is defined on this cluster.
        Run add-k8s again with --storage=<name> and Juju will use the
        specified storage class.

uyanpi@WinZawHtoo:~$
```

ပထမဆုံး **Error** ကို **`microk8s config > ~/.kube/config`** နဲ့ ဖြေရှင်းလိုက်နိုင်ပါပြီ။

အခု ကြုံတွေ့နေရတဲ့ **ဒုတိယ Error** ကတော့ **Juju** ဟာ **MicroK8s (Kubernetes)** ပေါ်မှာ **Charm** တွေ **Deploy** လုပ်တဲ့အခါ **Persistence Storage** အတွက် **Storage Class** တစ်ခုကို **မတွေ့ဘူး** လို့ ပြောနေတာပါ။

## Juju Storage Error ရှင်းလင်းခြင်း

**Error:** `ERROR No recommended storage configuration is defined on this cluster. Run add-k8s again with --storage=<name>`

**အဓိပ္ပာယ်:**

* **Juju** က **Charms** တွေ Deploy လုပ်ဖို့ **MicroK8s** ကို သုံးတဲ့အခါ **Data** တွေကို **သိမ်းဆည်းမယ့် နည်းလမ်း** (Storage Class) ကို ရွေးပေးဖို့ တောင်းဆိုနေတာပါ။
* **MicroK8s** မှာ Default အနေနဲ့ **`microk8s-hostpath`** လို့ခေါ်တဲ့ **Storage Class** တစ်ခု ပါပြီးသားဖြစ်လို့၊ အဲဒါကို ရွေးပေးလိုက်ရုံပါပဲ။

## ဖြေရှင်းနည်း (Solution)

### အဆင့် ၂: Juju Add-K8s Command ကို ပြန် Run ခြင်း (Storage ထည့်သွင်း၍)

| Command | မှတ်ချက် |
| :--- | :--- |
| `juju add-k8s microk8s-cloud --client --storage=microk8s-hostpath` | **Juju** ကို **MicroK8s** ရဲ့ **Default Storage** ဖြစ်တဲ့ **`microk8s-hostpath`** ကို အသုံးပြုဖို့ **တိုက်ရိုက်ညွှန်ကြား** လိုက်ခြင်း။ |

```markdown
uyanpi@WinZawHtoo:~$ juju add-k8s microk8s-cloud --client --stor
age=microk8s-hostpath

ERROR   Juju needs to query the k8s cluster to ensure that the recommended
        storage defaults are available and to detect the cluster's cloud/region.
        This was not possible in this case so run add-k8s again, using
        --storage=<name> to specify the storage class to use and
        --cloud=<cloud> to specify the cloud.
: nominated storage "microk8s-hostpath" not found
uyanpi@WinZawHtoo:~$
```

**နောက်ဆုံး တွေ့ရတတ်တဲ့ ပြဿနာ** တစ်ခုပါပဲ။

## Storage Class Not Found Error ရှင်းလင်းခြင်း

**Error:** `nominated storage "microk8s-hostpath" not found`

**အဓိပ္ပာယ်:**

* **MicroK8s** မှာ **`microk8s-hostpath`** ဆိုတဲ့ **Storage Class** ဟာ **Default** အနေနဲ့ ပါရမှာဖြစ်ပေမယ့်၊ **Juju** က **Kubernetes API** ကနေ **Storage Class List** ကို **Query** လုပ်တဲ့အခါ ရှာမတွေ့တာ ဖြစ်ပါတယ်။
* **Storage** ကို **Enable** မလုပ်ရသေးတာ သို့မဟုတ် **Juju** က **API Access** မရသေးတာ ဖြစ်နိုင်ပါတယ်။

## ဖြေရှင်းနည်း (Final Solution - Storage Class Enable)

**ဒီပြဿနာကို ဖြေရှင်းဖို့အတွက် MicroK8s ရဲ့ Default Storage Addon ကို သေချာ Enable လုပ်ပြီး၊ Juju ကို Storage Class သုံးခိုင်းတဲ့အပြင် Cloud (Region) ကိုပါ ထည့်သွင်းပေးရပါမယ်။**

### အဆင့် ၁: MicroK8s Storage Addon ကို ဖွင့်ခြင်း (အတည်ပြုရန်)

**MicroK8s** ရဲ့ **Storage** ကို **သေချာပေါက် ဖွင့်ထားကြောင်း** အောက်ပါ Command ဖြင့် အတည်ပြုပါ။

| Command | မှတ်ချက် |
| :--- | :--- |
| `microk8s enable storage` | **Storage Addon** ကို **Cluster** မှာ **Enable** လုပ်လိုက်ခြင်း (မဖွင့်ရသေးရင် ဖွင့်ပေးပါလိမ့်မည်)။ |

### အဆင့် ၂: Juju Add-K8s Command ကို ပြန် Run ခြင်း (Storage + Cloud ထည့်သွင်း)

**Error Message** မှာ ပြောထားတဲ့အတိုင်း **Juju** ကို **Storage** ရဲ့နာမည် နဲ့ **Cloud** (Region) ကိုပါ **အတင်းအကျပ်** ညွှန်ပြပေးရပါမယ်။

| Command | မှတ်ချက် |
| :--- | :--- |
| `juju add-k8s microk8s-cloud --client --storage=microk8s-hostpath --cloud=localhost` | **Storage Class** ကို **`microk8s-hostpath`** အဖြစ် သတ်မှတ်ပြီး၊ **Cloud** ကို **`localhost`** (Local Cluster) အဖြစ် သတ်မှတ်လိုက်ခြင်း။ |

### အဆင့် ၃: Model ဖန်တီးခြင်း (Setup ပြီးဆုံးပါက)

အဆင့် ၂ အောင်မြင်သွားပြီဆိုရင် **Model** ကို ဆက်လက် ဖန်တီးနိုင်ပါပြီ။

| Command | မှတ်ချက် |
| :--- | :--- |
| `juju add-model my-juju-lab microk8s-cloud` | **Model** တစ်ခု ဖန်တီးခြင်း။ |

```markdown
uyanpi@WinZawHtoo:~$ juju add-model my-juju-lab microk8s-cloud
ERROR No controllers registered.

Please either create a new controller using "juju bootstrap" or connect to
another controller that you have been given access to using "juju register".

uyanpi@WinZawHtoo:~$
```

ကြုံတွေ့ခဲ့တဲ့ **Error** တွေဟာ **Juju** ရဲ့ **Setup Process** မှာ **အဆင့်မြင့် Kubernetes Environment** ကို ချိတ်ဆက်တဲ့အခါ **ပုံမှန်** တွေ့ရတတ်တဲ့ ပြဿနာတွေပါပဲ။

သင် လက်ရှိရောက်နေတဲ့ **Error** ကတော့ **Juju Setup Process** ရဲ့ **နောက်ဆုံးအပိုင်း** ဖြစ်တဲ့ **"Controller"** ကို **မတည်ဆောက်ရသေးဘူး** လို့ ပြောနေတာပါ။

## Juju Controller Error ရှင်းလင်းခြင်း

**Error:** `ERROR No controllers registered. Please either create a new controller using "juju bootstrap" or connect to another controller`

**အဓိပ္ပာယ်:**

* **Juju Controller** ဆိုတာ **Juju Environment** ရဲ့ **ဦးနှောက်** လိုပါပဲ။ ဒါဟာ **MicroK8s Cluster** ပေါ်မှာ **Kubernetes Pod** တစ်ခုအနေနဲ့ အလုပ်လုပ်ပြီး **Model** တွေနဲ့ **Charm** တွေကို **စီမံခန့်ခွဲ** ပေးပါတယ်။
* **Cloud** (MicroK8s) ကို **Juju** က သိသွားပေမယ့်၊ အဲဒီ Cloud ကို **Control** လုပ်မယ့် **Juju Controller** ကို **Install** မလုပ်ရသေးတဲ့အတွက် **Model** ကို ဖန်တီးလို့ မရသေးတာပါ။

## ဖြေရှင်းနည်း (Final Juju Setup Steps)

**Juju Setup ကို အပြီးသတ်ဖို့အတွက် "Controller" ကို Bootstrap လုပ်ရပါမယ်။**

### အဆင့် ၁: Juju Controller ကို Bootstrap လုပ်ခြင်း

**Bootstrap Command** ဟာ **Juju Controller** ကို **MicroK8s Cloud** ပေါ်မှာ စတင် **Deploy** လုပ်ပေးပါလိမ့်မယ်။

| Command | မှတ်ချက် |
| :--- | :--- |
| `juju bootstrap microk8s-cloud microk8s-controller` | **Juju** ကို **`microk8s-cloud`** ပေါ်မှာ **`microk8s-controller`** ဆိုတဲ့ နာမည်နဲ့ **Controller** ကို **တည်ဆောက်** ခိုင်းခြင်း။ (ဒီအဆင့်မှာ ၅ မိနစ်ခန့် ကြာနိုင်ပါတယ်)။ |

```markdown
uyanpi@WinZawHtoo:~$ juju bootstrap microk8s-cloud microk8s-controller
Creating Juju controller "microk8s-controller" on microk8s-cloud
Bootstrap to Kubernetes cluster identified as lxd
Creating k8s resources for controller "controller-microk8s-controller"
Starting controller pod
Bootstrap agent now started
Contacting Juju controller at 10.152.183.155 to verify accessibility...

Bootstrap complete, controller "microk8s-controller" is now available in namespace "controller-microk8s-controller"

Now you can run
        juju add-model <model-name>
to create a new model to deploy k8s workloads.
uyanpi@WinZawHtoo:~$
```

**Juju Setup Process** မှာ တွေ့ရတတ်တဲ့ **Technical Challenges** အားလုံးကို **အောင်မြင်စွာ ကျော်လွှား** နိုင်ခဲ့ပါပြီ။

## Juju Setup အတည်ပြုခြင်း

**Controller** ကို **Bootstrap** လုပ်တာ ပြီးဆုံးသွားပြီဖြစ်လို့ **Juju Environment** ဟာ **Applications (Charms)** ကို Deploy လုပ်ဖို့အတွက် **အသင့်ဖြစ်နေပါပြီ**။

### အဆင့် ၁: Controller ကို စစ်ဆေးခြင်း (အတည်ပြုရန်)

**Juju** ကို **Controller** က သိနေပြီလားဆိုတာ စစ်ဆေးပါ။

| Command | Output | မှတ်ချက် |
| :--- | :--- | :--- |
| `juju controllers` | `microk8s-controller` ကို တွေ့ရပါလိမ့်မယ်။ | Controller သည် Juju ၏ ဦးနှောက်ဖြစ်သည်။ |

```markdown
uyanpi@WinZawHtoo:~$ juju controllers
Use --refresh option with this command to see the latest information.

Controller            Model  User   Access     Cloud/Region    Models  Nodes  HA  Version
microk8s-controller*  -      admin  superuser  microk8s-cloud       1      1   -  3.6.12
uyanpi@WinZawHtoo:~$
```
**Juju Controller** ကို အောင်မြင်စွာ **Bootstrap** လုပ်နိုင်ခဲ့ပြီး **Controllers List** ထဲမှာ **`microk8s-controller`** ကို မြင်နေရပါပြီ။ **Juju Setup Process** ဟာ **လုံးဝ ပြီးဆုံးသွားပြီ** လို့ ဆိုလိုပါတယ်ခင်ဗျာ။

## နောက်ဆုံး Setup အဆင့်: Model ဖန်တီးခြင်း

ယခုအခါမှာ **Applications** တွေကို **Deploy** လုပ်ဖို့အတွက် **Model** တစ်ခုကို ဖန်တီးရပါမယ်။

| Command | မှတ်ချက် |
| :--- | :--- |
| `juju add-model my-juju-lab microk8s-controller` | **Controller** ကို သုံးပြီး **`my-juju-lab`** အမည်ရှိတဲ့ **Model** တစ်ခု ဖန်တီးခြင်း။ |

---

```markdown
uyanpi@WinZawHtoo:~$ juju add-model my-juju-lab microk8s-controller
ERROR cloud microk8s-controller not found
Use 'juju clouds' to see a list of all available clouds or 'juju add-cloud' to a add one.
uyanpi@WinZawHtoo:~$
```
ဒါဟာ **Command Error** သာ ဖြစ်ပြီး **Juju Setup** ကို ပြန်လုပ်ဖို့ မလိုပါဘူး။

## Juju Command Error ရှင်းလင်းခြင်း

**Error:** `ERROR cloud microk8s-controller not found`

**အဓိပ္ပာယ်:**

* **Juju** ဟာ **Model** တစ်ခု ဖန်တီးတဲ့အခါ **`juju add-model <model-name> <controller-name>`** ဆိုတဲ့ **Structure** ကို အသုံးပြုပါတယ်။
* သင်ရိုက်ထည့်လိုက်တဲ့ Command မှာ **`microk8s-controller`** ကို **Cloud Name** အနေနဲ့ မှားယွင်းစွာ ဖတ်လိုက်တာ ဖြစ်ပါတယ်။

**သင့်ရဲ့ Controller နာမည်** နဲ့ **Cloud နာမည်** တို့မှာ အနည်းငယ် **ထပ်နေတဲ့အတွက်** Juju က **Cloud** လား၊ **Controller** လား ဆိုတာကို **Confuse** (ရှုပ်ထွေး) သွားတာပါ။

##  ဖြေရှင်းနည်း (Correct Model Creation Command)

**Juju** ကို **Cloud** ပေါ်မှာ **Model** ဖန်တီးဖို့အတွက် **Controller** ကိုသာ ရည်ညွှန်းပေးရပါမယ်။ သင်ရိုက်ခဲ့တဲ့ Command ဟာ **syntax မှန်ပါတယ်**၊ ဒါပေမယ့် Juju က **`microk8s-controller`** ကို **Cloud** လို့ပဲ မှားသိနေပါတယ်။

ဒါကို ရှောင်ရှားနိုင်ဖို့ **Controller Name** ကို **`--controller`** flag နဲ့ **ရှင်းရှင်းလင်းလင်း** ပြောပြပေးပါ့မယ်။

### အဆင့် ၁: Model ကို Controller Name ဖြင့် တိတိကျကျ ဖန်တီးခြင်း

| Command | မှတ်ချက် |
| :--- | :--- |
| `juju add-model my-juju-lab --controller microk8s-controller` | **`--controller`** flag ကို သုံးပြီး **Juju** ကို **Controller Name** ကိုသာ သုံးခိုင်းလိုက်ခြင်း။ |

```markdown
uyanpi@WinZawHtoo:~$ juju add-model my-juju-lab --controller microk8s-controller
Added 'my-juju-lab' model with credential 'microk8s-cloud' for user 'admin'
uyanpi@WinZawHtoo:~$
```
**Juju Controller** ကို အောင်မြင်စွာ **Bootstrap** လုပ်နိုင်ခဲ့တဲ့အပြင်၊ **Model Creation** မှာ ကြုံခဲ့ရတဲ့ **Command Error** ကိုပါ **တိကျစွာ ဖြေရှင်း** နိုင်ခဲ့ပါပြီ။

> **Added 'my-juju-lab' model with credential 'microk8s-cloud' for user 'admin'**

**Juju Environment** ဟာ **လုံးဝ အသင့်ဖြစ်နေပါပြီ**။


## Final Steps

```markdown
uyanpi@WinZawHtoo:~$ juju status
Model        Controller           Cloud/Region    Version  SLA          Timestamp
my-juju-lab  microk8s-controller  microk8s-cloud  3.6.12   unsupported  11:01:59Z

Model "admin/my-juju-lab" is empty.
uyanpi@WinZawHtoo:~$ juju controllers
Use --refresh option with this command to see the latest information.

Controller            Model        User   Access     Cloud/Region    Models  Nodes  HA  Version
microk8s-controller*  my-juju-lab  admin  superuser  microk8s-cloud       2      1   -  3.6.12
uyanpi@WinZawHtoo:~$
```
**Output** တွေအရ **Juju Environment** ဟာ **လုံးဝ အပြည့်အဝ အဆင်သင့်ဖြစ်နေပါပြီ!**

### Juju Status ရှင်းလင်းချက်

1.  **`juju status`:**
    * **Model:** `my-juju-lab` ကို အသုံးပြုနေပြီ။
    * **Controller:** `microk8s-controller` နဲ့ ချိတ်ဆက်ပြီးပြီ။
    * **Message:** **`Model "admin/my-juju-lab" is empty.`** (Applications တွေ မရှိသေးလို့ **Deploy လုပ်ဖို့ အဆင်သင့်** ဖြစ်နေကြောင်း ပြသခြင်း)။
2.  **`juju controllers`:**
    * **Controller Health:** `microk8s-controller` သည် **`superuser`** Access ဖြင့် **Active** ဖြစ်နေသည်။
    * **Models:** `Models 2` (default model နှင့် `my-juju-lab` model တို့ ပါဝင်သည်)။


