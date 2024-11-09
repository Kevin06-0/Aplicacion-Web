# Aplicacion-Web
const express = require('express');
const http = require('http');
const { Server } = require('socket.io');

const app = express();
const server = http.createServer(app);
const io = new Server(server);

app.use(express.static('public'));

// Cuando un jugador se conecta
io.on('connection', (socket) => {
  console.log('Nuevo jugador conectado:', socket.id);

  // Configurar los eventos del juego, por ejemplo: "jugada"
  socket.on('jugada', (data) => {
    // Emitir la jugada a los otros jugadores
    socket.broadcast.emit('jugada', data);
  });

  // Cuando un jugador se desconecta
  socket.on('disconnect', () => {
    console.log('Jugador desconectado:', socket.id);
  });
});

const PORT = process.env.PORT || 3000;
server.listen(PORT, () => {
  console.log(`Servidor escuchando en el puerto ${PORT}`);
});
