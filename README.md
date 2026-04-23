# Vemo
<!DOCTYPE html>
<html>
<head>
  <title>Socket.io Group Chat</title>
  <style>
    body { font-family: sans-serif; }
    #messages { list-style-type: none; margin: 0; padding: 0; }
    #messages li { padding: 8px; margin-bottom: 5px; background: #f4f4f4; border-radius: 4px; }
    #form { display: flex; position: fixed; bottom: 0; width: 100%; padding: 10px; background: #fff; }
    #input { border: 1px solid #ddd; padding: 10px; flex-grow: 1; border-radius: 4px; }
    button { padding: 10px 20px; background: #28a745; color: white; border: none; cursor: pointer; }
  </style>
</head>
<body>
  <ul id="messages"></ul>
  <form id="form" action="">
    <input id="input" autocomplete="off" placeholder="Type a message..." /><button>Send</button>
  </form>

  <script src="/socket.io/socket.io.js"></script>
  <script>
    const socket = io();

    const form = document.getElementById('form');
    const input = document.getElementById('input');
    const messages = document.getElementById('messages');

    // 1. Send message to server when form is submitted
    form.addEventListener('submit', (e) => {
      e.preventDefault();
      if (input.value) {
        socket.emit('chat message', input.value);
        input.value = '';
      }
    });

    // 2. Listen for 'chat message' from server to display it
    socket.on('chat message', (msg) => {
      const item = document.createElement('li');
      item.textContent = msg;
      messages.appendChild(item);
      window.scrollTo(0, document.body.scrollHeight);
    });
  </script>
</body>
</html>
