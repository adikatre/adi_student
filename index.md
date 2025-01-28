---
layout: base
title: 
description: Home Page
image: /images/mario_animation.png
hide: true
---

<link href="https://stackpath.bootstrapcdn.com/bootstrap/4.5.2/css/bootstrap.min.css" rel="stylesheet">

<!-- Liquid:  statements -->

<!-- Include submenu from _includes to top of pages -->
{% include nav/home.html %}
<!--- Concatenation of site URL to frontmatter image  --->
{% assign sprite_file = site.baseurl | append: page.image %}
<!--- Has is a list variable containing mario metadata for sprite --->
{% assign hash = site.data.mario_metadata %}  
<!--- Size width/height of Sprit images --->
{% assign pixels = 256 %}

<!--- HTML for page contains <p> tag named "Mario" and class properties for a "sprite"  -->

<p id="mario" class="sprite"></p>
  
<!--- Embedded Cascading Style Sheet (CSS) rules, 
        define how HTML elements look 
--->
<style>

  /*CSS style rules for the id and class of the sprite...
  */
  .sprite {
    height: {{pixels}}px;
    width: {{pixels}}px;
    background-image: url('{{sprite_file}}');
    background-repeat: no-repeat;
  }

  /*background position of sprite element
  */
  #mario {
    background-position: calc({{animations[0].col}} * {{pixels}} * -1px) calc({{animations[0].row}} * {{pixels}}* -1px);
  }
</style>

<!--- Embedded executable code--->
<script>
  ////////// convert YML hash to javascript key:value objects /////////

  var mario_metadata = {}; //key, value object
  {% for key in hash %}  
  
  var key = "{{key | first}}"  //key
  var values = {} //values object
  values["row"] = {{key.row}}
  values["col"] = {{key.col}}
  values["frames"] = {{key.frames}}
  mario_metadata[key] = values; //key with values added

  {% endfor %}

  ////////// game object for player /////////

  class Mario {
    constructor(meta_data) {
      this.tID = null;  //capture setInterval() task ID
      this.positionX = 0;  // current position of sprite in X direction
      this.currentSpeed = 0;
      this.marioElement = document.getElementById("mario"); //HTML element of sprite
      this.pixels = {{pixels}}; //pixel offset of images in the sprite, set by liquid constant
      this.interval = 100; //animation time interval
      this.obj = meta_data;
      this.marioElement.style.position = "absolute";
    }

    animate(obj, speed) {
      let frame = 0;
      const row = obj.row * this.pixels;
      this.currentSpeed = speed;

      this.tID = setInterval(() => {
        const col = (frame + obj.col) * this.pixels;
        this.marioElement.style.backgroundPosition = `-${col}px -${row}px`;
        this.marioElement.style.left = `${this.positionX}px`;

        this.positionX += speed;
        frame = (frame + 1) % obj.frames;

        const viewportWidth = window.innerWidth;
        if (this.positionX > viewportWidth - this.pixels) {
          document.documentElement.scrollLeft = this.positionX - viewportWidth + this.pixels;
        }
      }, this.interval);
    }

    startWalking() {
      this.stopAnimate();
      this.animate(this.obj["Walk"], 3);
    }

    startRunning() {
      this.stopAnimate();
      this.animate(this.obj["Run1"], 6);
    }

    startPuffing() {
      this.stopAnimate();
      this.animate(this.obj["Puff"], 0);
    }

    startCheering() {
      this.stopAnimate();
      this.animate(this.obj["Cheer"], 0);
    }

    startFlipping() {
      this.stopAnimate();
      this.animate(this.obj["Flip"], 0);
    }

    startResting() {
      this.stopAnimate();
      this.animate(this.obj["Rest"], 0);
    }

    stopAnimate() {
      clearInterval(this.tID);
    }
  }

  const mario = new Mario(mario_metadata);

  ////////// event control /////////

  window.addEventListener("keydown", (event) => {
    if (event.key === "ArrowRight") {
      event.preventDefault();
      if (event.repeat) {
        mario.startCheering();
      } else {
        if (mario.currentSpeed === 0) {
          mario.startWalking();
        } else if (mario.currentSpeed === 3) {
          mario.startRunning();
        }
      }
    } else if (event.key === "ArrowLeft") {
      event.preventDefault();
      if (event.repeat) {
        mario.stopAnimate();
      } else {
        mario.startPuffing();
      }
    }
  });

  //touch events that enable animations
  window.addEventListener("touchstart", (event) => {
    event.preventDefault(); // prevent default browser action
    if (event.touches[0].clientX > window.innerWidth / 2) {
      // move right
      if (currentSpeed === 0) { // if at rest, go to walking
        mario.startWalking();
      } else if (currentSpeed === 3) { // if walking, go to running
        mario.startRunning();
      }
    } else {
      // move left
      mario.startPuffing();
    }
  });

  //stop animation on window blur
  window.addEventListener("blur", () => {
    mario.stopAnimate();
  });

  //start animation on window focus
  window.addEventListener("focus", () => {
     mario.startFlipping();
  });

  //start animation on page load or page refresh
  document.addEventListener("DOMContentLoaded", () => {
    // adjust sprite size for high pixel density devices
    const scale = window.devicePixelRatio;
    const sprite = document.querySelector(".sprite");
    sprite.style.transform = `scale(${0.2 * scale})`;
    mario.startResting();
  });

</script>
<br>
<a href="sprint5/">Sprint 5 Blog</a>

<div>
  <p>Click here to view my journey setting up GitHub Pages:</p>
  <a href="journey/">
  <button type="button" class="btn btn-success btn-lg" href="//journey/">My Journey</button>
  </a>
</div>
<br>


<div class="container mt-5">
    <h2>Jupyter Notebooks</h2>
    <div class="dropdown">
        <button class="btn btn-primary dropdown-toggle" type="button" id="dropdownMenuButton" data-toggle="dropdown" aria-haspopup="true" aria-expanded="false">
            Notebooks
        </button>
        <div class="dropdown-menu" aria-labelledby="dropdownMenuButton">
            <a class="dropdown-item" href="../adi_student/posts/fruits">Fruits Model</a>
            <a class="dropdown-item" href="posts/hello">Emoji Fun!</a>
            <a class="dropdown-item" href="/posts/js/">Javascript Notebook!</a>
        </div>
    </div>
</div>

<!-- Include Bootstrap JS and correct Popper.js version -->
<script src="https://code.jquery.com/jquery-3.5.1.slim.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/popper.js/1.16.0/umd/popper.min.js"></script>
<script src="https://stackpath.bootstrapcdn.com/bootstrap/4.5.2/js/bootstrap.min.js"></script>

<br>

Rooooooooooter1!!!!
<img src="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcTSMCjwqVaDSqOwzreh-JdJwm2h3885Qs-tJw&s" alt="roooter">

<!---addition calculator-->

<p>Enter first number: </p>
<input type="text" id="numInput1" placeholder="Enter your first number here: " class="form-control">

<p>Enter second number: </p>
<input type="text" id="numInput2" placeholder="Enter your second number here: " class="form-control">

<button onclick="addNumbers()" class="btn btn-success">Add</button>
<button onclick="subtractNumbers()" class="btn btn-success">Subtract</button>
<button onclick="divideNumbers()" class="btn btn-success">Divide</button>
<button onclick="multiplyNumbers()" class="btn btn-success">Multiply</button>

<p id="displayText"></p>

<script>
  function addNumbers() {
      // Get the values from the input fields
      let num1 = parseFloat(document.getElementById("numInput1").value);
      let num2 = parseFloat(document.getElementById("numInput2").value);

      // Check if the inputs are numbers
      if (isNaN(num1) || isNaN(num2)) {
          document.getElementById("displayText").innerText = "Please enter valid numbers.";
      } else {
          // Add the two numbers
          let sum = num1 + num2;

          document.getElementById("displayText").innerText = "The sum is: " + sum;
      }
  }

  function subtractNumbers() {
      // Get the values from the input fields
      let num1 = parseFloat(document.getElementById("numInput1").value);
      let num2 = parseFloat(document.getElementById("numInput2").value);

      // Check if the inputs are numbers
      if (isNaN(num1) || isNaN(num2)) {
          document.getElementById("displayText").innerText = "Please enter valid numbers.";
      } else {
          // Subtract the two numbers
          let difference = num1 - num2;

          document.getElementById("displayText").innerText = "The difference is: " + difference;
      }
  }

  function multiplyNumbers() {
      // Get the values from the input fields
      let num1 = parseFloat(document.getElementById("numInput1").value);
      let num2 = parseFloat(document.getElementById("numInput2").value);

      // Check if the inputs are numbers
      if (isNaN(num1) || isNaN(num2)) {
          document.getElementById("displayText").innerText = "Please enter valid numbers.";
      } else {
          // Multiply the two numbers
          let product = num1 * num2;

          document.getElementById("displayText").innerText = "The product is: " + product;
      }
  }

  function divideNumbers() {
      // Get the values from the input fields
      let num1 = parseFloat(document.getElementById("numInput1").value);
      let num2 = parseFloat(document.getElementById("numInput2").value);

      // Check if the inputs are numbers
      if (isNaN(num1) || isNaN(num2)) {
          document.getElementById("displayText").innerText = "Please enter valid numbers.";
      } else {
          // Check if the second number is zero
          if (num2 === 0) {
              document.getElementById("displayText").innerText = "Cannot divide by zero.";
          } else {
              // Divide the two numbers
              let quotient = num1 / num2;

              document.getElementById("displayText").innerText = "The quotient is: " + quotient;
          }
      }
  }

  function addNumbers() {
      // Get the values from the input fields
      let num1 = parseFloat(document.getElementById("numInput1").value);
      let num2 = parseFloat(document.getElementById("numInput2").value);

      // Check if the inputs are numbers
      if (isNaN(num1) || isNaN(num2)) {
          document.getElementById("displayText").innerText = "Please enter valid numbers.";
      } else {
          // Add the two numbers
          let sum = num1 + num2;

          document.getElementById("displayText").innerText = "The sum is: " + sum;
      }
  }

  function subtractNumbers() {
      // Get the values from the input fields
      let num1 = parseFloat(document.getElementById("numInput1").value);
      let num2 = parseFloat(document.getElementById("numInput2").value);

      // Check if the inputs are numbers
      if (isNaN(num1) || isNaN(num2)) {
          document.getElementById("displayText").innerText = "Please enter valid numbers.";
      } else {
          // Subtract the two numbers
          let difference = num1 - num2;

          document.getElementById("displayText").innerText = "The difference is: " + difference;
      }
  }

  function multiplyNumbers() {
      // Get the values from the input fields
      let num1 = parseFloat(document.getElementById("numInput1").value);
      let num2 = parseFloat(document.getElementById("numInput2").value);

      // Check if the inputs are numbers
      if (isNaN(num1) || isNaN(num2)) {
          document.getElementById("displayText").innerText = "Please enter valid numbers.";
      } else {
          // Multiply the two numbers
          let product = num1 * num2;

          document.getElementById("displayText").innerText = "The product is: " + product;
      }
  }

  function divideNumbers() {
      // Get the values from the input fields
      let num1 = parseFloat(document.getElementById("numInput1").value);
      let num2 = parseFloat(document.getElementById("numInput2").value);

      // Check if the inputs are numbers
      if (isNaN(num1) || isNaN(num2)) {
          document.getElementById("displayText").innerText = "Please enter valid numbers.";
      } else {
          // Check if the second number is zero
          if (num2 === 0) {
              document.getElementById("displayText").innerText = "Cannot divide by zero.";
          } else {
              // Divide the two numbers
              let quotient = num1 / num2;

              document.getElementById("displayText").innerText = "The quotient is: " + quotient;
          }
      }
  }
  </script>

<div id="paragraph">
      <p id="text">The links are not switched.</p>
      <a id="switchLinkButton" onclick="switchText()" target="_blank">Click me to switch links!</a>
  </div>
<script id="paragraph_text">
  function switchText() {
    let displayText = document.getElementById("text");
    let displayLink1 = document.getElementById("link1").href;
    let displayLink2 = document.getElementById("link2").href;
    let currentText = displayText.innerHTML;
    if (currentText === "The links are not switched.") {
      displayText.innerHTML = "Switched!";
      document.getElementById('link1').href = displayLink2;
      document.getElementById('link2').href = displayLink1;
    } else {
      displayText.innerHTML = "The links are not switched.";
    }
  }
</script>

<a id="link1" href="https://www.amromusic.com/clarinet-fingering-chart">Link #1</a><br>
<a id="link2" href="https://www.amromusic.com/saxophone-fingering-chart">Link #2</a>

<!-- <style>
    body {
        margin: 0;
        background-color: #000;
        display: flex;
        justify-content: center;
        align-items: center;
        height: 100vh;
    }
    canvas {
        border: 1px solid #fff;
    }
</style> -->

<style>
  canvas {
    border: 1px solid #fff;
  }
</style>

<h1>Press space to play snake!</h1>
<canvas id="gameCanvas" width="400" height="400"></canvas>
<br>
<label for="snakeColor">Snake Color:</label>
<input type="color" id="snakeColor" value="#ff8000">
<label for="foodColor">Food Color:</label>
<input type="color" id="foodColor" value="#99ccff">

<script>
    window.onload = function() {
        var canvas = document.getElementById('gameCanvas');
        var ctx = canvas.getContext('2d');

        // Game variables
        var gridSize = 20; // Size of the grid cell
        var tileCount = canvas.width / gridSize;

        var snake = [];
        snake[0] = { x: 10, y: 10 }; // Start position in grid units
        var direction = { x: 0, y: 0 }; // Snake is not moving at start
        var food = { x: 15, y: 15 }; // Initial food position

        var snakeColor = document.getElementById('snakeColor').value;
        var foodColor = document.getElementById('foodColor').value;

        var gameStarted = false;

        document.addEventListener('keydown', keyDown);
        document.getElementById('snakeColor').addEventListener('input', function(event) {
            snakeColor = event.target.value;
        });
        document.getElementById('foodColor').addEventListener('input', function(event) {
            foodColor = event.target.value;
        });

        function keyDown(event) {
            switch(event.keyCode) {
                case 32: // Space bar
                    if (!gameStarted) {
                        gameStarted = true;
                        direction.x = 1; // Start moving to the right
                        gameLoop();
                    }
                    break;
                case 37: // Left arrow
                    if (direction.x !== 1) {
                        direction.x = -1;
                        direction.y = 0;
                    }
                    break;
                case 38: // Up arrow
                    if (direction.y !== 1) {
                        direction.x = 0;
                        direction.y = -1;
                    }
                    break;
                case 39: // Right arrow
                    if (direction.x !== -1) {
                        direction.x = 1;
                        direction.y = 0;
                    }
                    break;
                case 40: // Down arrow
                    if (direction.y !== -1) {
                        direction.x = 0;
                        direction.y = 1;
                    }
                    break;
            }
        }

        function gameLoop() {
            if (!gameStarted) return; // Stop the loop if game is not started

            update();
            draw();

            setTimeout(gameLoop, 100); // Game speed
        }

        function update() {
            // Move snake
            var headX = snake[0].x + direction.x;
            var headY = snake[0].y + direction.y;

            // Check for wall collision
            if (headX < 0) headX = tileCount - 1;
            if (headX >= tileCount) headX = 0;
            if (headY < 0) headY = tileCount - 1;
            if (headY >= tileCount) headY = 0;

            // Check for collision with self
            for (var i = 0; i < snake.length; i++) {
                if (snake[i].x === headX && snake[i].y === headY) {
                    // Game over
                    gameStarted = false;
                    alert("Game Over");
                    // Reset game
                    snake = [];
                    snake[0] = { x: 10, y: 10 };
                    direction = { x: 0, y: 0 };
                    food = { x: Math.floor(Math.random() * tileCount), y: Math.floor(Math.random() * tileCount) };
                    return;
                }
            }

            // Add new head to snake
            snake.unshift({ x: headX, y: headY });

            // Check for food collision
            if (headX === food.x && headY === food.y) {
                // Generate new food
                food = { x: Math.floor(Math.random() * tileCount), y: Math.floor(Math.random() * tileCount) };
            } else {
                // Remove tail
                snake.pop();
            }
        }

        function draw() {
            // Clear canvas
            ctx.fillStyle = '#000';
            ctx.fillRect(0, 0, canvas.width, canvas.height);

            // Draw snake
            ctx.fillStyle = snakeColor;
            for (var i = 0; i < snake.length; i++) {
                ctx.fillRect(snake[i].x * gridSize, snake[i].y * gridSize, gridSize - 2, gridSize - 2);
            }

            // Draw food
            ctx.fillStyle = foodColor;
            ctx.fillRect(food.x * gridSize, food.y * gridSize, gridSize - 2, gridSize - 2);
        }

    }
</script>

<button id="jokeButton">Get a Joke!</button>
<div id="joke">Press the button to see a joke!</div>

<script>
    document.getElementById('jokeButton').addEventListener('click', getJoke);

    function getJoke() {
        fetch('https://v2.jokeapi.dev/joke/Programming?blacklistFlags=nsfw,religious,political,racist,sexist,explicit')
            .then(response => response.json())
            .then(data => {
                let joke = '';
                if (data.type === 'single') {
                    joke = data.joke;
                } else {
                    joke = `${data.setup} ... ${data.delivery}`;
                }
                document.getElementById('joke').textContent = joke;
            })
            .catch(error => {
                document.getElementById('joke').textContent = 'Oops! Something went wrong. Try again later.';
                console.error('Error fetching joke:', error);
            });
    }
</script>
