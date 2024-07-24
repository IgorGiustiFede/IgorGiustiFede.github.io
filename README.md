<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Speech Recognition Example</title>
</head>
<body>

<button onclick="startDictation()">Start Dictation</button>
<form id="labnol">
    <input type="text" id="transcript" placeholder="Speech will be transcribed here...">
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

    recognition.onresult = function(e) {
      // Mettre à jour le champ transcript avec le texte reconnu
      document.getElementById('transcript').value = e.results[0][0].transcript;
      recognition.stop();
      //document.getElementById('labnol').submit();
    };

    recognition.onerror = function(e) {
      recognition.stop();
    };
  } else {
    console.log("Speech Recognition not supported in this browser.");
  }
}




function makeResponse() {
  var text = document.getElementById("transcript");
  var res = document.getElementById("response");

  // Mettre à jour le champ texte avec le résultat de la transcription
  res.innerHTML = text.value;
}

// Déclenche makeResponse toutes les secondes (ou ajustez la fréquence selon vos besoins)
var t = setInterval(makeResponse, 1000);
  
</script>

</body>
</html>
