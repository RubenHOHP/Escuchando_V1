# Escuchando_V1
<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Vaiti - Escuchando</title>

  <style>
    body {
      font-family: Arial, sans-serif;
      background: #f4f6f8;
      margin: 0;
      padding: 20px;
      text-align: center;
    }

    h1 {
      font-size: 2rem;
      margin-bottom: 10px;
    }

    #status {
      font-size: 2.2rem;
      font-weight: bold;
      margin: 30px 0;
      color: #2e7d32;
      min-height: 50px;
    }

    #text {
      font-size: 1.6rem;
      background: white;
      padding: 20px;
      border-radius: 12px;
      min-height: 120px;
      box-shadow: 0 2px 6px rgba(0,0,0,0.1);
    }

    button {
      margin-top: 25px;
      font-size: 1.4rem;
      padding: 14px 26px;
      border-radius: 12px;
      border: none;
      cursor: pointer;
      background: #1976d2;
      color: white;
    }

    button.stop {
      background: #c62828;
    }
  </style>
</head>

<body>

  <h1>Hola Vaiti 💙</h1>

  <div id="status">No está escuchando</div>

  <div id="text">
    Presiona “Iniciar” para comenzar
  </div>

  <button id="toggleBtn">Iniciar</button>

  <script>
    const statusEl = document.getElementById("status");
    const textEl = document.getElementById("text");
    const btn = document.getElementById("toggleBtn");

    let listening = false;
    let recognition;

    // Compatibilidad Android / Chrome
    const SpeechRecognition =
      window.SpeechRecognition || window.webkitSpeechRecognition;

    if (!SpeechRecognition) {
      alert("Este dispositivo no soporta reconocimiento de voz 😢");
    } else {
      recognition = new SpeechRecognition();
      recognition.lang = "es-CL";
      recognition.continuous = true;
      recognition.interimResults = false;

      recognition.onstart = () => {
        statusEl.textContent = "🎤 Escuchando…";
      };

      recognition.onresult = (event) => {
        const lastResult = event.results[event.results.length - 1][0].transcript;
        textEl.textContent = lastResult;
      };

      recognition.onerror = (event) => {
        statusEl.textContent = "⚠️ Error al escuchar";
        console.error(event.error);
      };

      recognition.onend = () => {
        if (listening) recognition.start(); // reinicia solo
      };
    }

    btn.onclick = () => {
      if (!listening) {
        recognition.start();
        listening = true;
        btn.textContent = "Detener";
        btn.classList.add("stop");
      } else {
        recognition.stop();
        listening = false;
        statusEl.textContent = "No está escuchando";
        btn.textContent = "Iniciar";
        btn.classList.remove("stop");
      }
    };
  </script>

</body>
</html>
