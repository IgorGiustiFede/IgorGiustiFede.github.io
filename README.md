<!DOCTYPE html>
<html>
<head>
    <style>
        body {
            font-family: Arial, sans-serif;
        }

        button {
            margin: 10px;
            padding: 10px;
            font-size: 16px;
        }
    </style>
</head>
<body>
    <p id="word"></p>
    <button id="dictate">Dictate</button>
    <button id="next">Next</button>
    <script>
        // Extract parameter from URL
        const urlParams = new URLSearchParams(window.location.search);
        const myParam = urlParams.get('myParam');

        // Speech to text
        const recognition = new webkitSpeechRecognition();
        recognition.onresult = (event) => {
          const spokenWord = event.results[0][0].transcript;
          // Display the spoken word
          document.getElementById('word').textContent = spokenWord;
        };
        document.getElementById('dictate').addEventListener('click', () => {
          recognition.start();
        });

        // Send POST request
        document.getElementById('next').addEventListener('click', () => {
          fetch('https://example.com/api', {
            method: 'POST',
            headers: {
              'Content-Type': 'application/json',
            },
            body: JSON.stringify({ word: spokenWord }),
          })
          .then(response => response.json())
          .then(data => console.log(data))
          .catch((error) => {
            console.error('Error:', error);
          });
        });
    </script>
</body>
</html>
