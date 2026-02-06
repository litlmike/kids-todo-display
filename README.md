# Kids To-Do TV Display

Display the kids' daily to-do lists on the Samsung TV at 3pm when they come home from school.

## TV Info

- **Model:** Samsung UN60KU630D (6 Series, 60", 2016)
- **IP:** 192.168.1.29
- **MAC:** e4:7d:bd:e1:00:35
- **OS:** Tizen

## Setup

### 1. First-Time TV Pairing

The TV must accept the connection once:

```bash
samsungtv --host 192.168.1.29 device-info
```

A popup will appear on the TV asking to allow "SamsungTvRemoteCli". Click **Allow**.

### 2. Enable Wake-on-LAN on TV

TV Settings > General > Network > Expert Settings > **Power On with Mobile** = On

### 3. Start Local Web Server

From this directory, start a simple HTTP server:

```bash
python -m http.server 8080
```

Or add to startup for persistent hosting.

### 4. Test the Display

Open in browser: http://localhost:8080/index.html

### 5. Test TV Control

```bash
# Wake TV
samsungtv --host 192.168.1.29 wol --mac e4:7d:bd:e1:00:35

# Open browser (replace IP with your server's IP)
samsungtv --host 192.168.1.29 open-browser http://192.168.1.51:8080/index.html
```

### 6. Set Up Cron Job (OpenClaw)

Add to OpenClaw cron for 3pm daily trigger.

## File Structure

```
kids-todo-display/
├── index.html      # Display page (4 columns, one per kid)
├── tasks.json      # Task data (daily + today's tasks)
├── trigger.py      # Script to wake TV + open browser
├── icons/          # Custom task icons (future)
└── README.md       # This file
```

## Kids

| Name | Age | Can Read? | Display Format |
|------|-----|-----------|----------------|
| Magnus | 9 | ✅ | Text + icons |
| Samson | 8 | ✅ | Text + icons |
| Gideon | 6 | ❌ | Large icons + minimal text |
| Sigrid | 4 | ❌ | Large icons + minimal text |

## Customizing Tasks

Edit `tasks.json` to modify:
- `dailyRoutines`: Tasks shown every day
- `todayTasks`: Date-specific tasks (keyed by YYYY-MM-DD)

Future: Build a simple admin UI to manage tasks.

## Troubleshooting

### TV shows "Unauthorized"
Accept the pairing popup on the TV.

### TV doesn't wake
1. Enable "Power On with Mobile" in TV settings
2. Make sure TV is on Ethernet (WiFi WoL is less reliable)

### Browser doesn't open
1. TV may need to be manually turned on first
2. Wait longer after WoL (increase sleep in trigger.py)
