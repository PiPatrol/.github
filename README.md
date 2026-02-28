# 🛡️ PiPatrol

**Your home network's silent guardian.**

PiPatrol is an open-source network monitoring agent that runs on a Raspberry Pi (or any local machine) and watches over your home network 24/7. It analyzes DNS traffic via Pi-hole, detects suspicious behavior using a local LLM, and sends human-readable alerts in Polish directly to your Telegram.

No cloud. No subscriptions. No data leaving your home.

---

[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)
[![Docker](https://img.shields.io/badge/Docker-ready-blue?logo=docker)](https://www.docker.com/)
[![Raspberry Pi](https://img.shields.io/badge/Raspberry%20Pi-compatible-red?logo=raspberrypi)](https://www.raspberrypi.com/)
[![Pi-hole](https://img.shields.io/badge/Pi--hole-integrated-orange)](https://pi-hole.net/)
[![Status](https://img.shields.io/badge/status-early%20development-yellow)]()

---

## ✨ What it does

- 📡 **Monitors DNS traffic** from Pi-hole in real time
- 🤖 **Analyzes anomalies** using a locally running LLM (via Ollama)
- 📲 **Sends alerts to Telegram** — in plain Polish, not geek-speak
- 🏠 **Runs entirely offline** — your data never leaves your network
- 🐳 **Deployed via Docker Compose** — easy setup, easy updates

---

## 🧱 Tech stack

| Component | Role |
|---|---|
| Raspberry Pi / Mac Mini | Host hardware |
| Pi-hole | DNS monitoring & ad blocking |
| FastAPI | Backend API |
| Ollama | Local LLM inference |
| Docker Compose | Orchestration |
| Telegram Bot | Alert delivery |

---

## 🚀 Getting started

> ⚠️ PiPatrol is in early development. Instructions will be added as the project matures.

```bash
git clone https://github.com/PiPatrol/pi-patrol.git
cd pi-patrol
cp .env.example .env
# Edit .env with your Telegram token and Pi-hole details
docker compose up -d
```

---

## 🗺️ Roadmap

- [x] Project kickoff & name 🎉
- [ ] Core Pi-hole DNS log ingestion
- [ ] FastAPI backend with event processing
- [ ] Ollama integration for anomaly analysis
- [ ] Telegram alert bot (Polish language)
- [ ] Docker Compose setup for Raspberry Pi
- [ ] Web dashboard (local)
- [ ] Device fingerprinting
- [ ] Custom alert rules
- [ ] Multi-language support

---

## 💡 Why PiPatrol?

Most home network monitoring tools are either too complex, cloud-dependent, or designed for enterprise use. PiPatrol is built for regular families — the alerts it sends are meant to be understood by your spouse, not just you.

> *"Something weird is happening on the network. Ktoś łączy się z nieznanym serwerem o 3 w nocy."*

---

## 🤝 Contributing

PiPatrol is in its early stages and contributions are very welcome! Whether it's code, ideas, documentation, or bug reports — feel free to open an issue or a PR.

---

## 📄 License

MIT — free to use, modify, and share.

---

*Built with ❤️ on a Raspberry Pi, somewhere in Poland.*
