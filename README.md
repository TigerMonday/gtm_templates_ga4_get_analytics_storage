# GA4 Client & Session Info → dataLayer (readAnalyticsStorage Template)

Push **Client ID**, **Session ID**, **Session Number** and **Measurement ID** from
`readAnalyticsStorage()` into the **dataLayer**—optionally namespaced with a custom prefix.

> **Why?**
> Forward GA4 session context to Server‑GTM, Measurement Protocol calls or debugging dashboards without writing custom JavaScript on every page.

---

## ✨ Features

| Capability                | Details                                                                                    |
| ------------------------- | ------------------------------------------------------------------------------------------ |
| **Safe GA4 cookie read**  | Uses Google’s `readAnalyticsStorage()` API (Consent Mode‑ready).                           |
| **Measurement‑ID filter** | Required `measurementId` field targets the exact property when multiple GA4 configs exist. |
| **Key‑name prefixing**    | Optional `valuePrefix` field prepends a string (plus `_`) to every pushed key.             |
| **ES‑5 compliant**        | Works in GTM’s Sandbox (no modern JS syntax).                                              |
| **Zero external deps**    | Only GTM‑supplied APIs (`readAnalyticsStorage`, `callInWindow`).                           |

---

## 🔧 Template fields

| Field               | Type | Required                                 | Description                                                                 |
| ------------------- | ---- | ---------------------------------------- | --------------------------------------------------------------------------- |
| **`measurementId`** | TEXT | **Yes**                                  | GA4 Measurement ID (e.g. `G‑XXXXXXX`).                                      |
| **`eventName`**     | TEXT | No<br>(default `analytics_storage_read`) | Event name pushed to the dataLayer.                                         |
| **`valuePrefix`**   | TEXT | No                                       | Prefix for key names.<br>`test` → `test_client_id`, `test_ga_session_id`, … |

---

## 📤 Example dataLayer output

With `valuePrefix = "test"` and `eventName` left default:

```jsonc
{
  "event": "analytics_storage_read",
  "test_client_id":      "GA1.2.1393238191.1754052334",
  "test_ga_session_id":  "1725206400",
  "test_session_number": "5",
  "test_measurement_id": "G‑XXXXXXX"
}
```

---

## 🛠 Installation

1. **Import the template**
   *GTM UI → Templates → New → Import → select the exported `template.tpl.json`.*

2. **Set permissions** (auto‑prompted on first save)

   | Category                  | Permission             | Value            |
   | ------------------------- | ---------------------- | ---------------- |
   | Requires API              | `readAnalyticsStorage` | —                |
   | Requires API              | `callInWindow`         | —                |
   | Accesses global variables | Execute                | `dataLayer.push` |

3. **Create a tag**
   *Tags → New → Tag Type: **Community Template** → select this template.*

4. **Configure fields**

   * `measurementId` – **required**
   * `eventName` – optional (defaults to `analytics_storage_read`)
   * `valuePrefix` – optional (leave blank for none)

5. **Set a trigger**
   Fire **after** your GA4 Configuration tag to ensure cookies exist, e.g.
   *Trigger:* *Tag Sequencing → “Fire this tag after GA4 Config”*.

6. **Preview & Debug**
   In GTM Debug Mode, open the **Data Layer** tab and confirm the keys and values appear as expected.

---

## 📥 Using the dataLayer Variables

This template pushes GA4 client and session information to the dataLayer, but **you must create corresponding dataLayer variables in GTM to use these values**.

### 📋 Created dataLayer Keys

Create the following dataLayer variables in GTM for each key output by this template:

| dataLayer Key           | Description                             | Example Variable Name  |
| ----------------------- | -------------------------------------- | -------------------- |
| `client_id`             | GA4 Client ID                          | `DLV - Client ID`    |
| `ga_session_id`         | GA4 Session ID                         | `DLV - Session ID`   |
| `session_number`        | Session Number                         | `DLV - Session Number` |
| `measurement_id`        | Measurement ID (G-XXXXXXX)             | `DLV - Measurement ID` |

> 💡 **With Prefix**: When `valuePrefix = "analytics"`, keys become `analytics_client_id`, etc.

### 🔧 GTM Variable Creation Steps

1. **GTM Dashboard** → **Variables** → **New**
2. **Variable Type**: **Data Layer Variable**
3. **Data Layer Variable Name**: Enter key name from above (e.g., `client_id`)
4. **Variable Name**: Set a clear name (e.g., `DLV - Client ID`)

### 🎯 Usage Examples

#### **1. Send to Server-side GTM**
```javascript
// Send as additional parameters in GA4 Event tags
{
  "client_id": "{{DLV - Client ID}}",
  "session_id": "{{DLV - Session ID}}",
  "session_number": "{{DLV - Session Number}}"
}
```

#### **2. Set as Custom Dimensions**
In GA4 Configuration tag custom dimensions:
- `custom_client_id`: `{{DLV - Client ID}}`
- `session_number`: `{{DLV - Session Number}}`

#### **3. Use in Conditional Triggers**
Set trigger conditions to fire only when `{{DLV - Client ID}}` exists

#### **4. Use with Measurement Protocol API**
Utilize as client_id when sending data to GA4 from external systems

### ⚡ Recommended Setup

For efficient usage, we recommend the following workflow:

1. **Create Variables**: Create all 4 dataLayer variables listed above
2. **Configure Trigger**: Use the `analytics_storage_read` event as trigger
3. **Tag Sequencing**: Execute this template tag before other dependent tags
4. **Debug**: Verify dataLayer values in GTM Preview mode

---

## ⚠️ Limitations & Notes

* Requires **analytics\_storage consent**. If denied, values are empty.
* On the very first pageview GA4 cookies may not yet exist; if values are empty, delay the trigger (e.g. `Window Loaded`).

---

## 📝 Changelog

| Version | Date       | Notes                   |
| ------- | ---------- | ----------------------- |
| 1.0.0   | 2025‑08‑03 | Initial public release. |

---

## 📄 License

MIT — use, modify, and share freely. Attribution appreciated but not required.
