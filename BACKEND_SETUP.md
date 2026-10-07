# Minecraft Bedrock Voice Chat - Backend Documentation

## Setup

### Requirements
- Node.js 14+ (download from https://nodejs.org/)
- A static IP or domain for your server
- Open port 3000 (or configure a different port)

### Installation

1. Navigate to the backend folder:
```bash
cd backend
```

2. Run the setup script:
```bash
bash setup.sh
# On Windows: run setup.sh via Git Bash or use: npm install
```

3. Start the server:
```bash
npm start
```

You should see:
```
╔════════════════════════════════════════╗
║  Minecraft Bedrock Voice Chat Server   ║
║          Running on port 3000          ║
╚════════════════════════════════════════╝
```

## API Endpoints

### Player Registration
```
POST /api/players/register
Body: { uuid, username, x, y, z }
Response: { success: true }
```

### Update Position
```
POST /api/players/position
Body: { uuid, x, y, z }
Response: { success: true }
```

### Get Nearby Players (16 block radius)
```
GET /api/players/nearby/:uuid
Response: { nearby: [...] }
```

### Mute Player (5 minutes)
```
POST /api/mute
Body: { uuid, duration: 300000 }
Response: { success: true }
```

### Unmute Player
```
POST /api/unmute
Body: { uuid }
Response: { success: true }
```

### Check Mute Status
```
GET /api/mute/status/:uuid
Response: { muted: true/false }
```

## Admin Dashboard

### Get All Players
```
GET /api/admin/players
Response: { players: [...] }
```

### Get Muted Players
```
GET /api/admin/muted
Response: { muted: [...] }
```

### Health Check
```
GET /api/health
Response: { status: 'online', players: 5 }
```

## WebSocket Events (Socket.io)

### Client → Server
- `join-voice` - Player joins voice chat
- `voice-data` - Send audio chunk
- `start-talking` - Player starts talking
- `stop-talking` - Player stops talking
- `disconnect` - Player disconnects

### Server → Client
- `voice-data` - Receive audio from nearby player

## Configuration

### Change Port
Edit `server.js` and modify:
```javascript
const PORT = process.env.PORT || 3000;
```

Or set environment variable:
```bash
PORT=8080 npm start
```

### Change Proximity Range
Edit `server.js` and modify (currently 16 blocks):
```javascript
const proximityRange = 16;
```

### Customize Mute Duration
Default is 5 minutes (300000 ms). Change in the mute endpoint or when calling:
```
POST /api/mute
Body: { uuid, duration: 600000 } // 10 minutes
```

## Hosting

### Local Testing
Just run `npm start` - server runs on localhost:3000

### For Realm (You need a public server)

**Option 1: Heroku (Free tier ending soon)**
1. Create Heroku account
2. Install Heroku CLI
3. Run: `heroku create your-app-name`
4. Run: `git push heroku main`

**Option 2: DigitalOcean/AWS/Linode**
1. Create a droplet/instance with Node.js
2. Upload this folder
3. Run `npm install && npm start`
4. Use PM2 to keep it running: `npm install -g pm2 && pm2 start server.js`

**Option 3: Your Home Server**
- Forward port 3000 on your router
- Use a domain or update DNS
- Run on startup with PM2

## Troubleshooting

**"Port 3000 already in use"**
```bash
# Find what's using it
lsof -i :3000

# Use a different port
PORT=3001 npm start
```

**"Connection refused"**
- Check firewall is allowing port 3000
- Check server is running
- Verify correct IP/domain in MCBE addon config

**"Players not connecting"**
- Check server URL in addon config
- Verify firewall rules
- Check console for errors

## Monitoring

Check admin panel:
```
http://your-server:3000/api/admin/players
```

Monitor console output for:
- `[CONNECT]` - Player connections
- `[VOICE]` - Voice chat activity
- `[MUTE]` - Mute events
- `[TALK]` - Talk start/stop

---

For the MCBE addon setup, see README.md
