<!DOCTYPE html>
<html>
<head>
  <title>Captura de Imagem</title>
</head>
<body>
  <h1>Captura de Imagem</h1>
  <video id="video" width="320" height="240" autoplay></video>
  <button id="snap">Capturar Imagem</button>
  <canvas id="canvas" width="320" height="240"></canvas>

  <script>
    var video = document.getElementById('video');
    var canvas = document.getElementById('canvas');
    var context = canvas.getContext('2d');
    var snap = document.getElementById('snap');

    // Solicitar permissão para acessar a webcam
    navigator.mediaDevices.getUserMedia({ video: true })
      .then(function(stream) {
        video.srcObject = stream;
        video.play();
      })
      .catch(function(err) {
        console.log("Erro: " + err);
      });

    // Captura a imagem quando o botão é clicado
    snap.addEventListener("click", function() {
      context.drawImage(video, 0, 0, 320, 240);
      // Opcional: Enviar a imagem para um servidor
      var dataURL = canvas.toDataURL('image/png');
      fetch('https://yourserver.com/upload', {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json',
        },
        body: JSON.stringify({ image: dataURL }),
      }).then(response => response.json())
        .then(data => console.log(data))
        .catch(error => console.error('Erro:', error));
    });
  </script>
</body>
</html>
