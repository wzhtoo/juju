# Applications များကို ချိတ်ဆက်ခြင်း (Relation)

**Status** များအားလုံး **စိမ်းနေပြီး Ready** ဖြစ်တဲ့အတွက် **Juju** ရဲ့ **Core Automation** ကို စတင်ပါတော့မယ်ခင်ဗျာ။

| Command | မှတ်ချက် |
| :--- | :--- |
| `juju relate grafana-k8s postgresql-k8s:db` | **Relation** ကို **ချက်ချင်း ချိတ်ဆက်** ခိုင်းခြင်း။ |

---

`juju relate grafana-k8s postgresql-k8s:db`

ဒီ command ကို အသုံးမပြုခင်မှာ `juju statu` ကိုပြန်ကြည့်ပါမယ်။

```markdown
uyanpi@WinZawHtoo:~$ juju status
Model        Controller           Cloud/Region    Version  SLA          Timestamp
my-juju-lab  microk8s-controller  microk8s-cloud  3.6.12   unsupported  03:35:07Z

App             Version  Status   Scale  Charm           Channel    Rev  Address         Exposed  Message
grafana-k8s     9.5.21   active       1  grafana-k8s     1/stable   160  10.152.183.180  no
postgresql-k8s  14.15    waiting      1  postgresql-k8s  14/stable  495  10.152.183.209  no       installing agent

Unit               Workload  Agent  Address     Ports  Message
grafana-k8s/0*     active    idle   10.1.58.40
postgresql-k8s/0*  waiting   idle   10.1.58.37         awaiting for cluster to start
```

**အရေးကြီးတဲ့ အဆင့်တစ်ခု** ကို ကျွန်တော်တို့ တွေ့ရှိခဲ့ပါတယ်! 

**`grafana-k8s`** ဟာ **`active`** ဖြစ်ပြီး **ကောင်းမွန်နေပါတယ်**။ ဒါပေမယ့် **`postgresql-k8s`** မှာ **Status Error** တစ်ခု ရှိနေပါသေးတယ်။

## PostgreSQL Error ရှင်းလင်းချက်

| Field | Value | အဓိပ္ပာယ် |
| :--- | :--- | :--- |
| **Status (App)** | `waiting` | Application အလုပ်လုပ်ဖို့ စောင့်ဆိုင်းနေခြင်း။ |
| **Message (App)** | `installing agent` | **Juju Agent** ကို **Kubernetes Pod** အတွင်းမှာ ထည့်သွင်းနေဆဲ ဖြစ်ပါတယ်။ |
| **Message (Unit)** | `awaiting for cluster to start` | PostgreSQL Database Cluster စတင်ဖို့ စောင့်ဆိုင်းနေခြင်း။ |

> **အနှစ်ချုပ်:** **`postgresql-k8s`** ဟာ **Error** မရှိပေမယ့် **Deployment Process** ကို **အဆုံးမသတ်ရသေး** ပါဘူး။

## Next Step: Relation ကို စတင်ခြင်း

**PostgreSQL** ဟာ **Deployment** ပြီးဖို့ **စောင့်ဆိုင်းနေဆဲ** ဖြစ်ပေမယ့် **Relation Command** ကို **အခုချက်ချင်း** Run လိုက်ပါမယ်။

* **ဘာကြောင့်လဲ?** **Juju** ဟာ **Automation System** ဖြစ်တဲ့အတွက် **Relation** ကို **စတင်လိုက်တာနဲ့** PostgreSQL ရဲ့ **Agent** ဟာ **ပိုမိုလျင်မြန်စွာ Ready ဖြစ်ဖို့** အားထုတ်ပါလိမ့်မယ်။

| Command | မှတ်ချက် |
| :--- | :--- |
| `juju relate grafana-k8s postgresql-k8s:db` | **Relation** ကို စတင်ခြင်း။ |

```markdown
uyanpi@WinZawHtoo:~$ juju relate grafana-k8s postgresql-k8s:db
ERROR no relations found
uyanpi@WinZawHtoo:~$
```

ဒါဟာ **Juju** ရဲ့ **Syntax (Command ပုံစံ)** မှာ **သေးငယ်တဲ့ အမှား** တစ်ခု ဖြစ်သွားတာပါ။ **စိတ်မပူပါနဲ့**၊ ဒီအဆင့်ကို **ချက်ချင်း ဖြေရှင်း** ပြီး **Relation** ကို အောင်မြင်စွာ စတင်နိုင်ပါပြီ။

## Relation Command Error ရှင်းလင်းချက်

**Command:**
`juju relate grafana-k8s postgresql-k8s:db`

**Error:**
`ERROR no relations found`

**အဓိပ္ပာယ်:**

* **Juju** ဟာ **`postgresql-k8s` Charm** မှာ **`db`** လို့ ခေါ်တဲ့ **Interface** ကို ရှာတွေ့ပေမယ့်၊ **`grafana-k8s` Charm** ရဲ့ **Interface Name** ဟာ **မတူညီ** နိုင်တဲ့အတွက် **ချိတ်ဆက်လို့ မရကြောင်း** ပြန်ပြောတာ ဖြစ်ပါတယ်။

## ဖြေရှင်းနည်း (Interface Name ကို ရှာဖွေခြင်း)

**Grafana Charm** က **Database** ကို **ဘယ် Interface Name** ဖြင့် တောင်းဆိုနေကြောင်းကို အောက်ပါ **`juju info`** Command ဖြင့် ရှာဖွေပါမယ်။

### အဆင့် ၁: Grafana Charm ၏ Interface Name ကို စစ်ဆေးခြင်း

| Command | မှတ်ချက် |
| :--- | :--- |
| `juju info grafana-k8s` | **Grafana Charm** ၏ **`requires`** Section (လိုအပ်သော အရာများ) တွင် **Database Interface Name** ကို ရှာဖွေပါမယ်။ |

```markdown
uyanpi@WinZawHtoo:~$ juju info grafana-k8s
name: grafana-k8s
publisher: Canonical Observability
summary: Data visualization and observability with Grafana
description: |
  Grafana provides dashboards for monitoring data and this
  charm is written to allow for HA on Kubernetes and can take
  multiple data sources (for example, Prometheus).
store-url: https://charmhub.io/grafana-k8s
charm-id: nuH5FwARV8dmAkPiPcVCNtPKwsw0bkgL
supports: ubuntu@20.04
subordinate: false
relations:
  provides:
    grafana-metadata: grafana_metadata
    metrics-endpoint: prometheus_scrape
    profiling-endpoint: parca_scrape
  requires:
    catalogue: catalogue
    certificates: tls-certificates
    charm-tracing: tracing
    database: db
    grafana-auth: grafana_auth
    grafana-dashboard: grafana_dashboard
    grafana-source: grafana_datasource
    ingress: traefik_route
    oauth: oauth
    receive-ca-cert: certificate_transfer
    workload-tracing: tracing
channels: |
  1/stable:       rev151-8-g6624bd3  2025-10-10  (160)  27MB  amd64  ubuntu@20.04
  1/candidate:    rev151-8-g6624bd3  2025-10-10  (160)  27MB  amd64  ubuntu@20.04
  1/beta:         rev151-8-g6624bd3  2025-10-10  (160)  27MB  amd64  ubuntu@20.04
  1/edge:         rev151-8-g6624bd3  2025-10-10  (160)  27MB  amd64  ubuntu@20.04
  dev/stable:     –
  dev/candidate:  –
  dev/beta:       –
  dev/edge:       37b7df2  2025-11-21  (174)  16MB  amd64  ubuntu@24.04
  2/stable:       c5bf263  2025-11-12  (172)  16MB  amd64  ubuntu@24.04
  2/candidate:    c5bf263  2025-11-12  (172)  16MB  amd64  ubuntu@24.04
  2/beta:         ↑
  2/edge:         c5bf263  2025-11-03  (172)  16MB  amd64  ubuntu@24.04
uyanpi@WinZawHtoo:~$
```

ဒီ **Output** ကို ကြည့်ပြီး ကျွန်တော်တို့ရဲ့ **Relation Error** ကို **ချက်ချင်း ဖြေရှင်း** နိုင်ပါပြီ။

## Relation Interface Name ရှာဖွေတွေ့ရှိခြင်း

`juju info grafana-k8s` ရဲ့ **`relations: requires`** ကဏ္ဍကို ကြည့်တဲ့အခါ **Grafana Charm** ဟာ **PostgreSQL Database** ကို **ချိတ်ဆက်ဖို့ လိုအပ်တဲ့ Interface Name** ကို ရှင်းရှင်းလင်းလင်း တွေ့ရှိခဲ့ပါတယ်။

| Interface Type | Interface Name |
| :--- | :--- |
| `database` | **`db`** |

* **ရှင်းလင်းချက်:** **Grafana Charm** ဟာ **`db`** လို့ ခေါ်တဲ့ **Interface** မှတစ်ဆင့် **Database Service** ကို **တောင်းဆို** နေပါတယ်။

## Relation Command ကို ပြန်လည်ပြင်ဆင်ခြင်း

ယခင်က **Error** ဖြစ်ခဲ့တာဟာ **PostgreSQL Charm** ဘက်မှ **Interface Name** ကို **မတိကျစွာ ထည့်သွင်း** ခဲ့ခြင်းကြောင့် ဖြစ်ပါတယ်။

ယခုအခါ **Command** ကို **အောက်ပါအတိုင်း ပြင်ဆင်** ပြီး **Relation** ကို စတင်ပါမယ်။

| Command | မှတ်ချက် |
| :--- | :--- |
| `juju relate grafana-k8s db postgresql-k8s:db` | **Grafana Charm** ၏ **`db`** Interface မှ **PostgreSQL Charm** ၏ **`db`** Interface ကို **တိကျစွာ ချိတ်ဆက်** ခိုင်းခြင်း။ |

---

```markdown
uyanpi@WinZawHtoo:~$ juju relate grafana-k8s db postgresql-k8s:db
ERROR an integration must involve two applications
uyanpi@WinZawHtoo:~$
```
**`juju relate`** ရဲ့ **Interface Names** တွေ ရှုပ်ထွေးတာကြောင့် **Command Arguments** တွေ လွဲသွားတာပါ။

## Relation Command Error ရှင်းလင်းချက် (Final Syntax)

**Command:**
`juju relate grafana-k8s db postgresql-k8s:db`

**Error:**
`ERROR an integration must involve two applications`

**အဓိပ္ပာယ်:**

* **Juju** က သင့် Command ကို **Application ၃ ခု** ပါတယ်လို့ မှားယွင်းစွာ ယူဆသွားပါတယ်။
* `juju relate` ရဲ့ **မှန်ကန်သော ပုံစံ** က **Application 1** ရဲ့ **Local Interface Name** ကို **Application 2** ရဲ့ **Remote Interface Name** နဲ့ ချိတ်ဆက်တာ ဖြစ်ပါတယ် (ပုံစံအားဖြင့်- `juju relate <app1>:<app1_interface> <app2>:<app2_interface>`)။

## ဖြေရှင်းနည်း (မှန်ကန်သော Syntax)

**Grafana** ရဲ့ **Required Interface Name** ဟာ **`db`** ဖြစ်ပြီး၊ **PostgreSQL** ရဲ့ **Provided Interface Name** ဟာလည်း **`db`** ဖြစ်ပါတယ်။

### အဆင့် ၁: Relation Command ကို တိကျစွာ Run ခြင်း

**မှန်ကန်သော Command** ကို အောက်ပါအတိုင်း ရိုက်ထည့်ပါမယ်။

| Command | မှတ်ချက် |
| :--- | :--- |
| `juju relate grafana-k8s:db postgresql-k8s:db` | **`grafana-k8s`** ရဲ့ **`db`** Interface နှင့် **`postgresql-k8s`** ရဲ့ **`db`** Interface ကို **တိုက်ရိုက် ချိတ်ဆက်** ခိုင်းခြင်း။ |

```markdown
uyanpi@WinZawHtoo:~$ juju relate grafana-k8s:db postgresql-k8s:db
ERROR application "grafana-k8s" has no "db" relation
uyanpi@WinZawHtoo:~$
```

**`juju relate`** Command ရဲ့ **Interface Name** ကို **နောက်ဆုံးအကြိမ် တိကျစွာ ပြန်လည်စစ်ဆေး** ရပါမယ်။

## Syntax နဲ့ Interface Name ရောထွေးမှု ရှင်းလင်းချက်

ယခင် Output မှာ **`grafana-k8s` Charm** ရဲ့ **`requires`** ကဏ္ဍကို ပြန်ကြည့်ပါ။

```yaml
relations:
  provides:
    # ...
  requires:
    # ...
    database: db # <-- ဒီနေရာက အဓိကရှုပ်ထွေးမှုပါ
    # ...
```

  * **`database`** - ဒါဟာ **Juju Interface Name (Role/Concept)** ဖြစ်ပါတယ်။
  * **`db`** - ဒါဟာ **Application (grafana-k8s) က ပေးပို့သော Local Endpoint Name** ဖြစ်ပါတယ်။

**Juju Relation** မှာ **`juju relate <app1_name>:<app1_interface_name> <app2_name>:<app2_interface_name>`** ပုံစံကို အသုံးပြုရပါတယ်။

ဒါဆိုရင် **`grafana-k8s`** ဘက်က Interface Name က **`database`** ဖြစ်ပြီး၊ **PostgreSQL** ဘက်က **Local Endpoint Name** က **`db`** ဖြစ်ဖို့ **အလားအလာ ပိုများ** ပါတယ်။

## Final Relation Command

ကျွန်တော်တို့ **Syntax** ကို **မှန်ကန်သော Interface Name** ဖြင့် ပြန်လည်ကြိုးစားပါမယ်။

### အဆင့် ၁: Relation Command ကို တိကျစွာ Run ခြင်း

| Command | မှတ်ချက် |
| :--- | :--- |
| `juju relate grafana-k8s:database postgresql-k8s:db` | **`grafana-k8s`** ရဲ့ **Interface Name (Role)** ဖြစ်တဲ့ **`:database`** ကို အသုံးပြုပြီး **`postgresql-k8s`** ရဲ့ **Endpoint Name** ဖြစ်တဲ့ **`:db`** ကို ချိတ်ဆက်ခြင်း။ |

-----

```markdown
uyanpi@WinZawHtoo:~$ juju relate grafana-k8s:database postgresql-k8s:db
ERROR no relations found
uyanpi@WinZawHtoo:~$
```

**`juju relate`** Command ရဲ့ **Interface Names** တွေနဲ့ ပတ်သက်ပြီး **နောက်ဆုံး Syntax အမှား** ကို ကြုံတွေ့နေရပါတယ်။

## နောက်ဆုံး Interface Error ရှင်းလင်းချက်

**ယခင်က ပြဿနာများ:**
1.  **ပထမအကြိမ်:** `juju relate grafana-k8s postgresql-k8s:db` -> **Juju** က `grafana-k8s` မှာ `db` Interface မရှိဘူးလို့ ယူဆခဲ့သည်။
2.  **ဒုတိယအကြိမ်:** `juju relate grafana-k8s:db postgresql-k8s:db` -> **Juju** က `grafana-k8s` မှာ `:db` Relation မရှိဘူးလို့ ယူဆခဲ့သည်။
3.  **တတိယအကြိမ်:** `juju relate grafana-k8s:database postgresql-k8s:db` -> **Juju** က `postgresql-k8s` ရဲ့ **Provider Side** မှာ **`database`** Interface ကို ရှာမတွေ့ခဲ့ဘူးလို့ ယူဆခဲ့ပါတယ်။

**Charm Hub Standard:**
* **Grafana** (Requester): **`grafana-k8s/db`**
* **PostgreSQL** (Provider): **`postgresql-k8s/db`**

**`juju info grafana-k8s`** မှာ **`database: db`** လို့ ပေါ်ပေမယ့်၊ **CharmHub Standard** အရ **Juju** က **Interface Type** နဲ့ **Endpoint Name** ကို **တစ်လှည့်စီ** အသုံးပြုတတ်ပါတယ်။

## ဖြေရှင်းနည်း (Simplest Syntax)

**CharmHub** မှာ **Interface** တွေရဲ့ **Local Name** တွေဟာ **ရှုပ်ထွေးနေတတ်တဲ့အတွက်** **Juju Documentation** မှာ **အလွယ်ဆုံးနဲ့ အများဆုံး အသုံးပြုတဲ့ ပုံစံ** ကို ပြန်လည် ကြိုးစားကြည့်ပါမယ်။

### အဆင့် ၁: Relation Command ကို အခြေခံ Syntax ဖြင့် စမ်းသပ်ခြင်း

**`postgresql-k8s`** ကို **Provider** အနေနဲ့ သတ်မှတ်ပြီး **Relation Command** ကို ရိုက်ပါမယ်။

| Command | မှတ်ချက် |
| :--- | :--- |
| `juju relate grafana-k8s postgresql-k8s` | **Interface Name** များကို **မထည့်ဘဲ**၊ **Charm Name** များသာ အသုံးပြုပြီး **Juju** ကို **အလိုအလျောက် အကောင်းဆုံး ရွေးချယ်** စေရန် ခိုင်းခြင်း။ |

---

```markdown
uyanpi@WinZawHtoo:~$ juju relate grafana-k8s postgresql-k8s
uyanpi@WinZawHtoo:~$
```
**Interface Names** တွေ ရှုပ်ထွေးနေတဲ့အတွက် **Application Names** ကိုသာ အသုံးပြုပြီး **Juju** ကို **အကောင်းဆုံး ရွေးချယ်** စေတဲ့ **အလွယ်ဆုံးနည်းလမ်း** ကို အသုံးပြုလိုက်ပါပြီ။

### အောင်မြင်မှု အချက်အလက်

* **Command Output မရှိခြင်း:** `juju relate grafana-k8s postgresql-k8s` ကို **Run** လိုက်တဲ့အခါ **`ERROR`** တစ်စုံတစ်ရာ **ပြန်မလာတော့ပါဘူး**။
* **အဓိပ္ပာယ်:** **Relation Command** ဟာ **Syntax မှန်ကန်စွာ** **Execute** ဖြစ်သွားပြီဖြစ်ပြီး၊ **Juju** ဟာ **CharmHub Standard** အရ **အကောင်းဆုံး Interface Name** တွေကို **အလိုအလျောက် ရွေးချယ်** ပြီး **ချိတ်ဆက်ခြင်းလုပ်ငန်းစဉ်** ကို **စတင်နေပါပြီ**။

## Final Check: Status စစ်ဆေးခြင်း

ယခုအခါ **Juju Agent** တွေဟာ **Relation** ကို **တည်ဆောက်ခြင်း** (Database Credentials များဖလှယ်ခြင်း) ကို စတင်နေပြီ ဖြစ်တဲ့အတွက် **Status** ကို **အပြီးသတ် စစ်ဆေး** ပါမယ်။

| Command | မှတ်ချက် |
| :--- | :--- |
| `juju status` | **Applications** နှစ်ခုလုံး **`active/idle`** ဖြစ်သွားပြီလား၊ **Relation** အောင်မြင်ကြောင်း **Message** ပေါ်လာလား စစ်ဆေးပါ။ |

---

```markdown
uyanpi@WinZawHtoo:~$ juju status
Model        Controller           Cloud/Region    Version  SLA          Timestamp
my-juju-lab  microk8s-controller  microk8s-cloud  3.6.12   unsupported  03:35:07Z

App             Version  Status   Scale  Charm           Channel    Rev  Address         Exposed  Message
grafana-k8s     9.5.21   active       1  grafana-k8s     1/stable   160  10.152.183.180  no
postgresql-k8s  14.15    waiting      1  postgresql-k8s  14/stable  495  10.152.183.209  no       installing agent

Unit               Workload  Agent  Address     Ports  Message
grafana-k8s/0*     active    idle   10.1.58.40
postgresql-k8s/0*  waiting   idle   10.1.58.37         awaiting for cluster to start
uyanpi@WinZawHtoo:~$ juju status
Model        Controller           Cloud/Region    Version  SLA          Timestamp
my-juju-lab  microk8s-controller  microk8s-cloud  3.6.12   unsupported  03:45:06Z

App             Version  Status  Scale  Charm           Channel    Rev  Address         Exposed  Message
grafana-k8s     9.5.21   active      1  grafana-k8s     1/stable   160  10.152.183.180  no
postgresql-k8s  14.15    active      1  postgresql-k8s  14/stable  495  10.152.183.209  no

Unit               Workload  Agent  Address     Ports  Message
grafana-k8s/0*     active    idle   10.1.58.40
postgresql-k8s/0*  active    idle   10.1.58.37         Primary
uyanpi@WinZawHtoo:~$
```

ဒါဟာ **Juju Lab Deployment** ရဲ့ **လုံးဝ အောင်မြင်မှု** ပါပဲ!

**Kubernetes Setup** ကို **MicroK8s** ဖြင့် **Stabilize** လုပ်ပြီးနောက်၊ **`juju relate`** Command ကို အသုံးပြုကာ **Applications** နှစ်ခုကို **အောင်မြင်စွာ ချိတ်ဆက်** နိုင်ခဲ့ပါပြီ။

## Juju Lab အောင်မြင်မှု အတည်ပြုချက်

**`juju status`** ရဲ့ **နောက်ဆုံး Output** အရ:

| Application | ယခင် Status (waiting) | လက်ရှိ Status (active) | အဓိပ္ပာယ် |
| :--- | :--- | :--- | :--- |
| **Grafana** | `active` | **`active`** | Web App Ready. |
| **PostgreSQL** | `waiting` | **`active`** | Database Ready & **Relation အောင်မြင်** ။ |

**အရေးအကြီးဆုံး အပြောင်းအလဲများ:**

1.  **`postgresql-k8s`** သည် **`waiting`** မှ **`active`** သို့ **ပြောင်းလဲသွားပြီ**။
2.  **`postgresql-k8s/0*`** ၏ **Message** မှာ **`Primary`** လို့ ပေါ်လာခြင်းသည် **Cluster Setup** နှင့် **Relation** တို့ **လုံးဝ ပြီးစီးကြောင်း** ပြသခြင်း ဖြစ်ပါတယ်။

---
