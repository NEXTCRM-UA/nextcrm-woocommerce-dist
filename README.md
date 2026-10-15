# NextCRM for WooCommerce

Public distribution of the NextCRM integration plugin for **WooCommerce 7.0+**
on **WordPress 6.0+** and **PHP 7.4 – 8.3**.

This repository holds release archives only. The plugin source is private; merchants
download the ready `.zip` from [Releases](https://github.com/NEXTCRM-UA/nextcrm-woocommerce-dist/releases).

## Download

Stable (always the latest non-prerelease):

https://github.com/NEXTCRM-UA/nextcrm-woocommerce-dist/releases/latest/download/nextcrm-woocommerce.zip

A versioned copy `nextcrm-woocommerce-<version>.zip` is attached to the same
release. WordPress updates install that versioned file, so do not rename the
archive: it has to unpack to a single directory named `nextcrm-woocommerce`.

## Documentation

For the merchant. A few steps, all in the WordPress admin — **no hosting panel,
no FTP**. Cron is needed only when WP-Cron is switched off on purpose.

### What you need

- WordPress **6.0+**, WooCommerce **7.0+**, PHP **7.4 – 8.3**
- Admin access with permission to manage WooCommerce
- The API key NextCRM showed you when you created the integration (shown **once**;
  reissue it in NextCRM if you lost it)

### Install

1. **Plugins → Add New → Upload Plugin** — choose `nextcrm-woocommerce.zip`.
   Do not unpack it. Press **Install Now**, then **Activate**.
2. Open **WooCommerce → NextCRM**.
3. The key field starts with the sample `CHANGE_ME_key_for_woo_integration`.
   That is not a key: nothing is sent until you replace it. Paste your key and
   press **Save and connect**.

Saving connects to NextCRM immediately. In NextCRM the integration moves from
"Waiting for the module" to **Active** within a few seconds. The field then
shows only the first characters of the saved key, never the key itself.

If WooCommerce is not active, the plugin does nothing and shows a notice in
the admin.

### After install

Orders are written to a queue at checkout and delivered in the background, so a
CRM outage never blocks a purchase. Delivery runs through Action Scheduler
(shipped with WooCommerce) on ordinary shop traffic — no cron required.

Status mapping, which statuses are sent, the product binding key and reverse
sync are configured in NextCRM, not in the plugin. The settings page has three
tabs: **Settings**, **Diagnostics** and **Log**.

**Module status** can be set to Disabled. The shop then behaves as if the
plugin were not there, and this page stays so you can switch it back on.

The optional hosting-cron line on Settings is only for shops with WP-Cron
disabled, or with almost no visitors.

Later versions appear under **Plugins** as an available update. Updating keeps
the key, the queue and the scheduled work.

### If something looks wrong

Open **WooCommerce → NextCRM → Diagnostics** and **Log**.

| What you see | What it means |
|---|---|
| NextCRM still says "Waiting for the module", and the key field shows `CHANGE_ME_...` | The key was never saved. Paste it and save. |
| NextCRM still says "Waiting for the module" | Wrong key, or the shop cannot reach the internet. Check Log. |
| Status "Stopped" | NextCRM refused the key or this plugin version. Events stay queued. Save a valid key. |
| Status "Disabled" | Module status is Disabled. Set it to Enabled and save. |
| Items waiting, number falling | Normal — they are being delivered. |
| Items waiting, number not falling | Log and the last error explain why (often a revoked key, or WP-Cron not running). |
| Orders arrive but prices do not come back | Reverse sync off in NextCRM, or products do not match by SKU. |
| Nothing happens, almost no traffic | Add the hosting-cron line from the settings page. |

A **MISSING** row on Diagnostics means a required hook is not attached — report
that first.

### Uninstalling

**Plugins → Installed plugins → NextCRM for WooCommerce** → **Deactivate**, then
**Delete**.

Deactivate only stops background work; data stays. Delete removes the plugin
tables, the API key and the plugin log. Anything still in the queue is
discarded — wait for Diagnostics to show an empty queue first. Shop orders,
products and customers are untouched.

---

### Документація (українською)

Кроки в адмінці WordPress — **без панелі хостингу і FTP**.

1. **Плагіни → Додати новий → Завантажити плагін** — файл `nextcrm-woocommerce.zip`,
   не розпаковуйте. **Встановити**, потім **Активувати**.
2. **WooCommerce → NextCRM**.
3. Замініть зразок `CHANGE_ME_key_for_woo_integration` на свій API-ключ і
   натисніть **Зберегти й підключити**.

Ключ: NextCRM показує його один раз під час створення інтеграції.

Замовлення йдуть у чергу і доставляються у фоні; збій CRM не зупиняє покупку.
Оновлення з'являється на екрані **Плагіни**. Діагностика і журнал — на сторінці
NextCRM. Повний гайд англійською — у секції вище.
