const http = require('http');
const WebSocket = require('ws');

const server = http.createServer((req, res) => {
  res.writeHead(200);
  res.end("Server OK");
});

const wss = new WebSocket.Server({ server });

// Stocker clients
let clients = [];

wss.on('connection', (ws) => {
  console.log("✅ Client connected");

  clients.push(ws);

  ws.send(JSON.stringify({ message: "connected" }));

  ws.on('message', (data) => {
    console.log("📩 Message:", data.toString());

    // broadcast à tous
    clients.forEach(client => {
      if (client.readyState === WebSocket.OPEN) {
        client.send(data.toString());
      }
    });
  });

  ws.on('close', () => {
    console.log("❌ Client disconnected");
    clients = clients.filter(c => c !== ws);
  });
});

const port = process.env.PORT || 8080;

server.listen(port, () => {
  console.log("🚀 Running on port " + port);
});
