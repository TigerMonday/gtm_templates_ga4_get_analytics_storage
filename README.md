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

## ⚠️ Limitations & Notes

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
