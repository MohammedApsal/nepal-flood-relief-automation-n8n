# 🇳🇵 Nepal Flood Relief — Request Intake Automation

> **A small contribution using automation to help relief teams receive, organize, and escalate flood-relief requests faster.**

This project is an **n8n-based flood relief request intake automation** designed to organize incoming humanitarian requests and immediately escalate critical cases.

The workflow receives a request through a webhook, validates the submitted information, determines urgency, geocodes the location, stores the request in Airtable, confirms receipt through WhatsApp, and alerts a rescue team through Telegram when the request is critical.

## ❤️ Why I Built This

Technology is often built to improve business workflows and productivity.

But it can also be used for something more meaningful.

During a disaster, getting the right information to the right people quickly can matter.

This project is my **small contribution using AI/automation technology** to support a more organized flood-relief request process.

Please feel free to share, improve, or adapt the workflow for humanitarian use.

---

## 🚨 What The Automation Does

```text
Relief Request
      ↓
Webhook Intake
      ↓
Normalize Request Data
      ↓
Validate Request
      ↓
Determine Urgency
      ↓
Geocode Location
      ↓
Store Request in Airtable
      ↓
WhatsApp Confirmation
      ↓
Is Request Critical?
      ↓
 YES ─────────────→ Telegram Rescue Alert
```

### Request Types

The workflow supports:

* Rescue
* Medical
* Food
* Water
* Shelter
* Clothing
* Other

### Urgency Logic

| Request Type | Urgency     |
| ------------ | ----------- |
| Rescue       | 🚨 Critical |
| Medical      | 🚨 Critical |
| Food         | 🔴 High     |
| Water        | 🔴 High     |
| Shelter      | 🔴 High     |
| Clothing     | 🟡 Normal   |
| Other        | 🟡 Normal   |

The workflow implements this classification directly in its validation/code step.

---

# ⚙️ Workflow Features

### 1. Webhook Intake

Incoming requests are received through an HTTP `POST` webhook:

```text
/flood-relief-intake
```

The webhook is configured to return a response through a dedicated response node.

### 2. Data Normalization

The workflow extracts and normalizes:

* Name
* Phone number
* Ward/Village
* District
* Need type
* Number of people
* Description
* Submission time
* Request source

This creates a consistent structure for downstream processing.

### 3. Validation & Urgency Scoring

The workflow validates important fields and normalizes Nepal phone numbers to E.164 format.

It then assigns urgency based on the requested need.

Invalid requests are rejected with a `400` response, while valid requests continue through the relief workflow.

### 4. Location Geocoding

The submitted ward/village and district are sent to Google Maps Geocoding.

The workflow extracts:

* Latitude
* Longitude
* Geocoded address
* Google Maps link

This helps relief teams identify the requested location more easily.

### 5. Airtable Request Tracking

Validated requests are stored in an Airtable `Requests` table with information including:

* Request ID
* Name
* Phone
* Ward/Village
* District
* Need Type
* Description
* Number of People
* Urgency
* Latitude
* Longitude
* Maps Link
* Status
* Submitted At
* Source

New requests are initially stored with status **New**.

### 6. WhatsApp Confirmation

After the request is registered, the requester receives a WhatsApp confirmation containing their request ID and confirmation that the relief request has been recorded.

### 7. Critical Rescue Escalation

When a request is classified as **Critical**, the workflow sends a Telegram alert to the rescue team.

The alert includes:

* Request ID
* Urgency
* Type of need
* Number of people in danger
* Requester name
* Phone number
* Location
* Map pin
* Additional notes

This creates a direct escalation path for rescue and medical cases.

---

# 🛠️ Tech Stack

* **n8n** — Workflow automation
* **Google Maps Geocoding API** — Location resolution
* **Airtable** — Relief request database
* **Twilio / WhatsApp** — Requester confirmation
* **Telegram** — Critical rescue-team alerts
* **Webhook / HTTP** — Request intake

---

# 🚀 Setup

## 1. Install / Run n8n

Run your own n8n instance or use an n8n-hosted environment.

## 2. Import the Workflow

Import:

```text
workflow/Nepal-Flood-Relief-Request-Intake.json
```

into n8n.

## 3. Configure Credentials

You will need to configure the required credentials for:

### Google Maps

The workflow expects:

```text
GOOGLE_MAPS_API_KEY
```

as an environment variable for geocoding.

### Airtable

Connect your Airtable account and configure the target `Requests` table.

### Twilio

Configure your Twilio credentials for WhatsApp messaging.

### Telegram

Configure a Telegram bot and the destination chat for critical alerts.

> **Important:** Replace the credentials and destination IDs with your own values before production use. Never publish API keys, tokens, or private credentials in GitHub.

---

# 🧪 Example Request

Send a `POST` request to your n8n webhook.

Example:

```json
{
  "name": "Example User",
  "phone": "98XXXXXXXX",
  "location": "Ward 5",
  "district": "Example District",
  "need_type": "rescue",
  "num_people": 4,
  "description": "Family stranded due to flooding",
  "source": "web_form"
}
```

The workflow will validate the request, assign **Critical** urgency because the need type is `rescue`, geocode the location, create a request record, send confirmation, and trigger the critical Telegram alert.

---

# 🔐 Production Considerations

This workflow is intended as an **automation prototype / reusable foundation**, not a substitute for official emergency-response infrastructure.

Before real-world deployment:

* Use secure credential management.
* Protect the webhook from abuse.
* Add authentication/rate limiting.
* Verify incoming requests.
* Configure reliable monitoring and logging.
* Confirm that the receiving rescue organization actually controls the Telegram/WhatsApp channels.
* Add retry and failure-handling procedures.
* Test with local relief organizations before operational use.

---

# 📂 Project Files

```text
workflow/
└── Nepal-Flood-Relief-Request-Intake.json
```

The JSON file contains the complete n8n workflow configuration.

---

# 🤝 Contribution

This project is shared openly so developers, automation builders, NGOs, and humanitarian organizations can learn from it and improve it.

You can:

* Fork the project
* Improve the workflow
* Add additional communication channels
* Add dashboards
* Improve validation
* Add multilingual support
* Connect it to other relief-management systems

---

# 🇳🇵 A Small Contribution

This project is not about claiming that one automation can solve a disaster.

It is about using the skills I have to contribute something useful.

**If this project can help even one team receive critical information faster, it was worth building.**

Please **share and repost** this project so it can reach developers, NGOs, rescue organizations, and people who may be able to turn the idea into something useful on the ground.

❤️ **Technology should not only make businesses faster.
Sometimes, it should help people.**

---

## 📜 License

Add your preferred open-source license before publishing.

For humanitarian reuse, consider an appropriate permissive license such as MIT, subject to your own review.
