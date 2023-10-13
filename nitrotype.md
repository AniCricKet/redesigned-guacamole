---
toc: true
comments: true
layout: post
title: Nitro Type
permalink: /nitrotype/
courses: {csa: {week: 3} }
type: hacks
---
<html>
<head>
  <style>
    body {
      text-align: center;
    }
    #game-container {
      width: 400px;
      margin: 0 auto;
    }
    #paragraph-display {
      font-size: 24px;
      margin-bottom: 20px;
    }
    #input-field {
      font-size: 18px;
      padding: 5px;
      width: 100%;
      box-sizing: border-box;
    }
    #timer {
      font-size: 18px;
      margin-top: 20px;
    }
    #result {
      font-size: 18px;
      margin-top: 20px;
    }
    .result {
      border-radius: 12px;
      border: 1px solid black;
      padding: 20px;
      max-width: 300px;
      flex-shrink: 0;
    }
  </style>
  <!-- Importing table and sorting code -->
  <link rel="stylesheet" type="text/css" href="https://cdn.datatables.net/1.13.4/css/jquery.dataTables.min.css">
  <script type="text/javascript" language="javascript" src="https://code.jquery.com/jquery-3.6.0.min.js"></script>
  <script>var define = null;</script>
  <script type="text/javascript" language="javascript" src="https://cdn.datatables.net/1.13.4/js/jquery.dataTables.min.js"></script>
</head>
<body>
  <div id="game-container">
    <p id="paragraph-display">Start typing...</p>
    <textarea id="input-field" rows="4"></textarea>
    <p id="timer"></p>
    <p id="result"></p>
  </div>

  <script>
    var paragraphs = [
      "Determine retiree thought improve truth active",
      "Polish curve stun addicted extreme affect present",
      "Certain dramatic greeting order twin fade",
      "Relevance glimpse grain debt tell morning",
      "Genetic suggest reduce demonstrate lift make",
      "Entry circulation supply accountant admire spot",
      "Assignment bracket satellite agony equal afford",
      "Wash throw mistreat measure competition education",
    ];

    // This is the counter for how many paragraphs have been completed
    var paragraphsComplete = 0;

    // This generates a random integer from 0 to the number of paragraphs - 1
    var currentParagraphIndex = Math.floor(Math.random() * paragraphs.length);

    // This uses the random integer from above as an index for a random paragraph from the paragraph bank
    var currentParagraph = paragraphs[currentParagraphIndex];

    // Track the current character index within the paragraph
    var currentCharIndex = 0;

    // This sets the startTime and the timerInterval to undefined values
    var startTime = null;
    var timerInterval = null;

    // This is the code that replaces the previous paragraph
    var paragraphDisplay = document.getElementById("paragraph-display");
    // This gets the input from the text box
    var inputField = document.getElementById("input-field");
    // This is the code that allows the timer to update
    var timer = document.getElementById("timer");
    // Result element for displaying WPM and accuracy
    var resultElement = document.getElementById("result");

    // Function starts as soon as it detects an input
    inputField.addEventListener("input", function(event) {
      var enteredText = event.target.value;

      // Starts the timer after the user inputs something into the textbox
      if (!startTime) {
        startTime = new Date();
        startTimer();
      }

      // Verify if the entered text matches the current paragraph
      var paragraphText = currentParagraph.slice(0, enteredText.length);

      // Check each character and apply highlighting
      var highlightedText = '';
      for (var i = 0; i < enteredText.length; i++) {
        if (enteredText[i] === paragraphText[i]) {
          highlightedText += '<span style="background-color: yellow;">' + enteredText[i] + '</span>';
        } else {
          highlightedText += enteredText[i];
        }
      }

      paragraphDisplay.innerHTML = highlightedText + currentParagraph.slice(enteredText.length);

      // If the entered text matches the entire paragraph
      if (enteredText === currentParagraph) {
        // Display a "You Win!" message
        paragraphDisplay.textContent = "You Win!";
        // Hide the text box
        inputField.style.display = "none";
        // Stop the timer
        stopTimer();
      }
    });

    // Starts repeated action (timer) that updates every 10 milliseconds (0.01)
    function startTimer() {
      timerInterval = setInterval(updateTimer, 10);
    }

    // Stops the timer when it is called. It is called after the user has typed the entire paragraph
    function stopTimer() {
      // Makes the action above (timer) stop
      clearInterval(timerInterval);

      // Calculate WPM and accuracy
      var currentTime = new Date();
      var elapsedTime = (currentTime - startTime) / 1000; // in seconds
      var wordCount = currentParagraph.split(" ").length;
      var typedCharacters = inputField.value.length;
      var accuracy = (typedCharacters / currentParagraph.length) * 100;
      var wpm = (wordCount / (elapsedTime / 60)).toFixed(2); // words per minute

      // Display results
      resultElement.textContent = "Accuracy: " + accuracy.toFixed(2) + "% | WPM: " + wpm;
      
      // Waits 1 second after the game is complete. Then it asks for the user's name.
      setTimeout(()=> {
         var username = prompt('Congratulations! You completed the paragraph in ' + elapsedTime.toFixed(2) + ' seconds! Enter your name:');
         // Save or process the username as needed
         // Then reload the page
         location.reload();
      }, 1000);
    }

    // This function updates the timer every millisecond
    function updateTimer() {
      var currentTime = new Date();
      // Subtract the currentTime from the startTime to calculate the elapsed time in hundredths of a second
      var elapsedTime = Math.floor((currentTime - startTime) / 10);
      // Converts the elapsed time to seconds with two decimal places
      var actualTime = (elapsedTime / 100).toFixed(2);
      // Displays on the frontend
      timer.textContent = "Time: " + actualTime + " seconds";
    }
  </script>
</body>
</html>
