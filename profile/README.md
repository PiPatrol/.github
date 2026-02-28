<img width="1024" height="1024" alt="image" src="https://github.com/user-attachments/assets/3d7a979f-0dfe-4045-b1cd-d6af3523ed6b" />


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
be understood by your spouse, not just you.

> *"Something weird is happening on the network. Ktoś łączy się z nieznanym serwerem o 3 w nocy."*

---

## 🤝 Contributing

PiPatrol is in active early development and contributions are very welcome! Whether it's code, ideas, documentation, or bug reports — feel free to open an issue or a PR.

## 📄 License

MIT — free to use, modify, and share.

---

*Built with ❤️ on a Raspberry Pi, somewhere in Poland.*
