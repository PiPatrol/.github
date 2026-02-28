# 🛡️ PiPatrol

**Your home network's silent guardian.**

PiPatrol is an open-source network monitoring agent that runs on a Raspberry Pi and watches over your home network 24/7. It analyzes DNS traffic via Pi-hole, detects suspicious behavior using a local LLM, and sends human-readable alerts directly to your Telegram — in plain language, not geek-speak.

**No cloud. No subscriptions. No data leaving your home.**

---

## ✨ What it does

- 📡 Monitors all DNS queries in real time via Pi-hole
- 🤖 Analyzes traffic patterns using a local LLM (Ollama) or cloud fallback (OpenRouter)
- 📲 Sends Telegram alerts in plain Polish — understood by your whole family
- 📊 Sends a **daily report** every evening with a full activity summary
- 💬 Responds to **Telegram commands** (`/status`, `/raport`, `/urzadzenia`)
- 🖥️ Local web dashboard with charts, alert history, and LLM chat
- 💾 Persistent alert history in **SQLite**
- 🐳 Deployed with a single `docker compose up`

---

## 🚨 Example alerts

> 🚨 Tablet Zosi odwiedził kilkanaście stron z treściami dla dorosłych między 22:00 a 23:30. Warto porozmawiać.

> ⚠️ Smart TV przez ostatnią godzinę wysyłał zapytania do ponad 30 różnych serwerów reklamowych.

## 📊 Example daily report

```
📊 PiPatrol — raport dzienny 2024-01-15

Alerty łącznie: 3
  🚨 Wysokie:  1
  ⚠️  Średnie: 2
  ℹ️  Niskie:  0

Najbardziej aktywne urządzenia:
  • Tablet Zosi: 2 alerty
  • Laptop Kacpra: 1 alert

Kategorie aktywności:
  • Treści dla dorosłych: 14 zapytań
  • Social media: 87 zapytań
```

## 💬 Telegram commands

| Command | Description |
|---------|-------------|
| `/status` | System status and today's alert count |
| `/raport` | Daily report on demand |
| `/urzadzenia` | List of known devices on the network |
| `/pomoc` | Show all available commands |

---

## 🧱 Tech stack

| Component | Technology |
|-----------|------------|
| Backend | FastAPI + Python 3.11 (async) |
| Database | SQLite (aiosqlite) |
| Local LLM | Ollama (mistral:7b or any other model) |
| Cloud LLM | OpenRouter (fallback) |
| Alerts | Telegram Bot API |
| Frontend | Vanilla JS + SVG charts (zero dependencies) |
| Deployment | Docker Compose |

---

## 🚀 Getting started

### Requirements

- Raspberry Pi 4 (4GB+ RAM recommended)
- Any machine running [Ollama](https://ollama.com) on your local network (e.g. Mac Mini), **or** an [OpenRouter](https://openrouter.ai) API key
- A router that lets you set a custom DNS server

### Installation

```bash
git clone https://github.com/PiPatrol/pi-patrol.git
cd pi-patrol
cp .env.example .env
# Fill in your details (see Configuration below)
./install.sh
```

### Configure `.env`

```env
PIHOLE_PASSWORD=your_pihole_password
OLLAMA_URL=http://macmini.local:11434
OLLAMA_MODEL=mistral:7b
TELEGRAM_TOKEN=token_from_botfather
TELEGRAM_CHAT_ID=your_chat_id
```

### Point your router's DNS to the Raspberry Pi

In your router settings, set the primary DNS server to your Pi's local IP address.

### Add your devices in `config/config.yaml`

```yaml
devices:
  profiles:
    - name: "Tablet Zosi"
      mac: "aa:bb:cc:dd:ee:ff"   # find it in Pi-hole → Network → Clients
      type: "child"              # child | adult | iot | unknown
      alert_adult_content: true
      alert_late_night: true
      late_night_after: "22:00"
```

### Tune alert thresholds (optional)

Thresholds are set per device type — children get tighter rules, IoT devices looser ones:

```yaml
analyzer:
  interval_minutes: 5
  alert_threshold_percent: 20        # default threshold

  thresholds_by_device_type:
    child:
      alert_threshold_percent: 10    # more sensitive
      min_requests_for_session: 5
    adult:
      alert_threshold_percent: 30    # less sensitive
      min_requests_for_session: 15
    iot:
      alert_threshold_percent: 50    # IoT generates a lot of noise
      min_requests_for_session: 20

  daily_report_time: "22:00"         # when to send the daily report
```

### Extend domain categories in `config/categories.yaml`

```yaml
categories:
  adult:
    label: "Adult content"
    alert: true            # counts toward alert threshold
    domains:
      - pornhub.com
    patterns:
      - "\\bporn\\b"       # regex supported
```

### Get a Telegram Bot Token

1. Message `@BotFather` on Telegram
2. Send `/newbot`, follow the prompts, copy your token
3. Send any message to your new bot, then open:
   `https://api.telegram.org/bot<TOKEN>/getUpdates` — your `chat.id` will be there

---

## 🏗️ Architecture

```
[Router] → DNS = Raspberry Pi
    ↓
[Pi-hole] → /var/log/pihole/pihole.log
    ↓
[pihole_watcher]   reads logs every 5 min, categorizes domains
    ↓
[analyzer]         groups by IP, applies per-device-type thresholds
    ↓
[device_mapper]    IP → MAC → name  (Pi-hole v6/v5 API → ARP → cache)
    ↓
[llm_provider]     Ollama (local) or OpenRouter (cloud fallback)
    ↓
[telegram_bot]     real-time alerts + interactive commands
[daily_report]     scheduled daily summary at 22:00
    ↓
[database]         SQLite — persistent alert history
    ↓
[FastAPI + Dashboard]   http://RPI_IP:8000
```

---

## 🔌 API

| Endpoint | Description |
|----------|-------------|
| `GET /api/health` | System status (LLM, Pi-hole) |
| `GET /api/alerts` | Alert history from SQLite |
| `GET /api/stats?date=YYYY-MM-DD` | Daily statistics |
| `GET /api/devices` | Device list |
| `POST /api/devices/refresh` | Force device map refresh |
| `POST /api/chat` | Chat with the LLM agent |
| `GET /api/config` | Current configuration |
| `POST /api/config` | Update configuration |
| `POST /api/test/llm` | Test Ollama connection |
| `POST /api/test/pihole` | Test Pi-hole connection |
| `POST /api/test/telegram` | Send a test Telegram message |

---

## 🗺️ Roadmap

**v0.1 — Core (done ✅)**
- [x] Pi-hole DNS log ingestion and domain categorization
- [x] Device mapping: IP → MAC → name (Pi-hole API → ARP → cache)
- [x] LLM-powered alerts via Ollama / OpenRouter
- [x] Telegram alert delivery
- [x] Interactive Telegram commands (`/status`, `/raport`, `/urzadzenia`)
- [x] Scheduled daily report
- [x] Persistent SQLite alert history
- [x] Per-device-type alert thresholds (child / adult / iot)
- [x] External domain categories with regex support (`categories.yaml`)
- [x] Web dashboard with SVG charts (zero JS dependencies)
- [x] Docker Compose deployment

**v0.2 — Smarter monitoring**
- [ ] Hourly DNS activity chart (last 24h)
- [ ] Time-based rules (school hours, quiet hours per schedule)
- [ ] Anomaly detection: flag new/unseen domains automatically

**v0.3 — Polish & reach**
- [ ] Daily report export to PDF
- [ ] Email notifications as Telegram alternative
- [ ] English language support (i18n)
- [ ] Setup wizard (interactive first-run config)

---

## 💡 Why PiPatrol?

Most home network monitoring tools are either too complex, cloud-dependent, or built for enterprise use. PiPatrol is designed for regular families — the alerts it sends are meant to be understood by your spouse, not just you.

> *"Something weird is happening on the network. Ktoś łączy się z nieznanym serwerem o 3 w nocy."*

---

## 🤝 Contributing

PiPatrol is in active early development and contributions are very welcome! Whether it's code, ideas, documentation, or bug reports — feel free to open an issue or a PR.

## 📄 License

MIT — free to use, modify, and share.

---

*Built with ❤️ on a Raspberry Pi, somewhere in Poland.*
