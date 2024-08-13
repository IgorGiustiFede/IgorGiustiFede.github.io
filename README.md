<!DOCTYPE html>
<html>
<head>
    <style>
        body {
            font-family: Arial, sans-serif;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            height: 100vh;
            margin: 0;
        }

        .container {
            display: flex;
            align-items: center;
            margin-bottom: 50px;
        }

        .microphone {
            width: 50px;
            height: 50px;
            cursor: pointer;
            margin-right: 20px;
            background-color: #ebba47;
            border-radius: 50%;
            border: none;
            outline: none;
            display: flex;
            justify-content: center;
            align-items: center;
            position: relative;
        }

        .microphone svg {
            fill: white;
        }

        .microphone.active {
            background-color: red;
        }

        .microphone .tooltiptext {
            visibility: visible;
            width: 280px;
            background-color: #555;
            color: #fff;
            text-align: center;
            border-radius: 6px;
            padding: 5px;
            position: absolute;
            z-index: 1;
            bottom: 125%;
            left: 50%;
            margin-left: -140px;
            opacity: 0;
            transition: opacity 0.3s;
        }

        .microphone:hover .tooltiptext,
        .microphone:focus .tooltiptext,
        .microphone:active .tooltiptext {
            visibility: visible;
            opacity: 1;
        }

        #word {
            margin: 0;
            border: 2px solid #000;
            padding: 5px;
            width: 100px;
            height: 30px;
            display: flex;
            justify-content: center;
            align-items: center;
        }

        #next {
            background-color: #ebba47;
            color: white;
            border: none;
            padding: 10px 20px;
            border-radius: 5px;
            cursor: pointer;
        }
    </style>
</head>
<body>
    <div class="container">
        <button class="microphone inactive" id="dictate">
            <svg viewBox="0 0 24 24" fill="white">
                <path d="M12 14c1.66 0 2.99-1.34 2.99-3L15 5c0-1.66-1.34-3-3-3S9 3.34 9 5v6c0 1.66 1.34 3 3 3zm5.3-3c0 3-2.54 5.1-5.3 5.1S6.7 14 6.7 11H5c0 3.41 2.72 6.23 6 6.72V21h2v-3.28c3.28-.48 6-3.3 6-6.72h-1.7z"/>
            </svg>
            <span class="tooltiptext">Pour dicter le premier mot, merci d'appuyer sur le bouton en forme de micro et de dire le mot à voix haute et intelligible.</span>
        </button>
        <p id="word"></p>
    </div>
    <button id="next">Continuer</button>
    <script>
        // Extract parameter from URL
        const urlParams = new URLSearchParams(window.location.search);
        const ksaarID = urlParams.get('KsaarID');

        // Speech to text
        const recognition = new webkitSpeechRecognition();
        const microphone = document.getElementById('dictate');

        recognition.onresult = (event) => {
          const spokenWord = event.results[0][0].transcript;
          // Display the spoken word
          document.getElementById('word').textContent = spokenWord;
        };

        recognition.onstart = () => {
          microphone.classList.remove('inactive');
          microphone.classList.add('active');
        };

        recognition.onend = () => {
          microphone.classList.remove('active');
          microphone.classList.add('inactive');
        };

        microphone.addEventListener('click', () => {
          recognition.start();
        });

        // Send POST request
        document.getElementById('next').addEventListener('click', () => {
          fetch('https://example.com/api', {
            method: 'POST',
            headers: {
              'Content-Type': 'application/json',
            },
            body: JSON.stringify({ word: spokenWord, ksaarID: ksaarID }),
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

