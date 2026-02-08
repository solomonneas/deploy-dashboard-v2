# Deploy Dashboard v2

**Project monitoring and deployment control center for when you have 20+ dev servers running and can't remember which port is which.**

Deploy Dashboard v2 tracks all your portfolio projects in one view: server status, ports, git state, build health, disk usage, and deployment history. Start, stop, and restart dev servers without touching the terminal. Or at least, without touching it as much.

Built as a single-file dev tool for the [Solomon Neas](https://solomonneas.dev) portfolio toolchain.

---

## Features

- **Dashboard view** with project cards showing live status (online/offline/error), port assignments, uptime, and git branch
- **Server controls**: start, stop, restart, and kill dev servers per project via API calls
- **Bulk operations**: start all / stop all with a single click (for when it's that kind of morning)
- **Build status tracking**: last build time, pass/fail indicators, and build duration
- **Git state awareness**: dirty/clean working tree, current branch, last commit info
- **Disk usage monitoring**: per-project size, `node_modules` breakdown, and total disk consumption
- **Deployment history** with timestamps, durations, and rollback capability
- **Environment checks**: flags missing `.env` files or env vars that differ from `.env.example`
- **Auto-refresh** with configurable interval for live status polling
- **Resource view** with port map, disk usage bars, and health warnings
- **Category color-coding**: Security (red), Infrastructure (blue), Dev Tools (green), Fun (purple)
- **3 visual themes**: Terminal, Mission Control, and a default dark theme
- **Keyboard shortcuts** for common operations
- **Live + Demo modes**: works standalone with mock project data, or connects to the Dev Tools API for real server management

## Screenshots

> *Screenshots coming soon. The "17 servers online" counter is oddly satisfying.*

## How to Run

Single `index.html` file (~2,100 lines). Static HTML. No build step.

### Option A: Open directly

```bash
# Just double-click index.html, or:
open index.html        # macOS
xdg-open index.html    # Linux
start index.html       # Windows
```

Runs in **Demo mode** with mock project data. You can browse the dashboard, view history, and check resource usage, but server controls won't actually start/stop anything.

### Option B: Serve locally

```bash
# Python
python3 -m http.server 8080

# Node
npx serve .

# Or any static file server you prefer
```

### Option C: Connect to the Dev Tools API (Live mode)

For real server management, run the Dev Tools API backend on port `8005`, then toggle to **Live** mode. The dashboard hits:
- `GET /api/projects?disk=true` for project list and status
- `POST /api/projects/<slug>/start` to start a dev server
- `POST /api/projects/<slug>/stop` to stop a dev server
- `POST /api/projects/<slug>/restart` to restart a dev server

Dev servers in the portfolio toolchain typically run on ports in the `5173-5199` range (Vite defaults and custom assignments).

## How It Works

**Inputs:**
- In Demo mode: embedded mock data for ~20 portfolio projects with realistic status, port, git, and build information
- In Live mode: real project metadata and server status from the Dev Tools API backend

**Processing:**
1. Fetches project list with status, port assignments, git info, and disk usage
2. Renders cards grouped by category with color-coded status indicators
3. Polls for status changes on a configurable auto-refresh interval
4. Sends start/stop/restart commands through the API and updates the UI on response

**Outputs:**
- Visual dashboard of all projects with at-a-glance health status
- Deployment history timeline
- Resource utilization summary (ports, disk, env health)
- Toast notifications for server control actions

## Common Workflows

### 1. "Start my dev environment for the day"

Open the dashboard > Click **Start All** (or selectively start the 3-4 projects you're working on today) > Watch the cards flip from offline to online > Note the port numbers > Open your browser tabs.

### 2. "Something is broken, which server is down?"

Open the dashboard > Scan for red (error) or gray (offline) cards > Click the card to expand details > Check the build status and last error. If it's a port conflict, check the **Resources** tab for the port map.

### 3. "How much disk space are these projects eating?"

Click the **Resources** tab > Check the disk usage bars and total consumption > Sort by `node_modules` size to find the worst offenders > Make peace with the fact that `node_modules` is half your drive.

## Roadmap

- [ ] Log viewer per project (tail the dev server output)
- [ ] One-click "open in browser" buttons with correct port
- [ ] Git operations (pull, checkout, stash) from the dashboard
- [ ] Build trigger with live output streaming
- [ ] Webhook integration for deploy notifications
- [ ] Project grouping/workspace presets ("security stack", "fun projects", etc.)

## Contributing

Personal dev tool. PRs welcome if they stay single-file and don't add external dependencies. Bug reports are appreciated, feature requests are considered, and "you should rewrite this in React" suggestions are politely ignored.

## License

MIT. Monitor your servers, or don't. They'll keep running either way. Probably.
