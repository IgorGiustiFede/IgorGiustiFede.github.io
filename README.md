<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Reconnaissance Vocale</title>
</head>
<body>

<button onclick="startDictation()">Démarrer la Dictée</button>
<form id="labnol">
    <input type="text" id="transcript" name="transcript" placeholder="La parole sera transcrite ici...">
    <div id="response"></div>
</form>

<script>
    
function startDictation() {
  var SpeechRecognition = window.SpeechRecognition || window.webkitSpeechRecognition;

  if (SpeechRecognition) {
    var recognition = new SpeechRecognition();

    recognition.continuous = false;
    recognition.interimResults = false;
    recognition.lang = "fr-FR";
    
    recognition.start();

    recognition.onstart = function() {
      console.log('Reconnaissance vocale commencée');
    };

    recognition.onresult = function(event) {
      console.log('Résultat reçu: ', event.results[0][0].transcript);
      document.getElementById('transcript').value = event.results[0][0].transcript;
      document.getElementById('response').innerHTML = event.results[0][0].transcript;
      recognition.stop();
    };

    recognition.onerror = function(event) {
      console.log('Erreur de reconnaissance vocale: ', event.error);
      recognition.stop();
    };

    recognition.onend = function() {
      console.log('Reconnaissance vocale terminée');
    };
  } else {
    console.log("API de reconnaissance vocale non supportée dans ce navigateur.");
  }
}

function makeResponse() {
  var text = document.getElementById("transcript");
  var res = document.getElementById("response");

  res.innerHTML = text.value;
}

// Déclenche makeResponse toutes les secondes (ou ajustez la fréquence selon vos besoins)
var t = setInterval(makeResponse, 1000);
</script>

</body>
</html>
