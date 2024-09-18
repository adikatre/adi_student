---
layout: page
title: Cookie Clicker
permalink: /cookie/
image: ../images/cookie.png
---
<link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/css/bootstrap.min.css" integrity="sha384-ggOyR0iXCbMQv3Xipma34MD+dH/1fQ784/j6cY/iJTQUOhcWr7x9JvoRxT2MZw1T" crossorigin="anonymous">

# Cookie Clicker

<p>Cookies: <span id="score">0</span></p>

<img id="cookie" src="../images/cookie.png" alt="Cookie" style="cursor:pointer;width:200px;height:200px;">

<audio id="click-sound" src="../sounds/click.mp3"></audio>

## Shop

<!-- Worker Section -->
<button id="buy-worker" type="button" class="btn btn-secondary">Buy Clicker (Cost: <span id="worker-cost">10</span> Clickers)</button>
<p>You have <span id="worker-count">0</span> workers.</p>

<!-- Factory Section -->
<button id="buy-factory" type="button" class="btn btn-secondary">Buy Factory (Cost: <span id="factory-cost">500</span> Cookies)</button>
<p>You have <span id="factory-count">0</span> factories.</p>

<script>
document.addEventListener('DOMContentLoaded', function() {
    // Game Variables
    let score = 0;
    let workers = 0;
    let factories = 0;
    let workerCost = 10;
    let factoryCost = 500;
    const workerCPS = 1;   // Cookies per second per worker
    const factoryCPS = 50; // Cookies per second per factory

    // DOM Elements
    const scoreDisplay = document.getElementById('score');
    const workerCountDisplay = document.getElementById('worker-count');
    const workerCostDisplay = document.getElementById('worker-cost');
    const factoryCountDisplay = document.getElementById('factory-count');
    const factoryCostDisplay = document.getElementById('factory-cost');
    const clickSound = document.getElementById('click-sound');

    // Update Functions
    function updateScore() {
        scoreDisplay.innerText = score;
    }

    function updateWorkerDisplay() {
        workerCountDisplay.innerText = workers;
        workerCostDisplay.innerText = workerCost;
    }

    function updateFactoryDisplay() {
        factoryCountDisplay.innerText = factories;
        factoryCostDisplay.innerText = factoryCost;
    }

    // Cookie Click Event
    document.getElementById('cookie').addEventListener('click', function() {
        score++;
        updateScore();
        // Play sound effect
        clickSound.currentTime = 0;
        clickSound.play();
    });

    // Buy Worker Event
    document.getElementById('buy-worker').addEventListener('click', function() {
        if (score >= workerCost) {
            score -= workerCost;
            workers++;
            workerCost = Math.floor(workerCost * 1.15); // Increase cost by 15%
            updateScore();
            updateWorkerDisplay();
        } else {
            alert('Not enough cookies!');
        }
    });

    // Buy Factory Event
    document.getElementById('buy-factory').addEventListener('click', function() {
        if (score >= factoryCost) {
            score -= factoryCost;
            factories++;
            factoryCost = Math.floor(factoryCost * 1.15); // Increase cost by 15%
            updateScore();
            updateFactoryDisplay();
        } else {
            alert('Not enough cookies!');
        }
    });

    // Passive Income Generation
    setInterval(function() {
        let totalCPS = (workers * workerCPS) + (factories * factoryCPS);
        score += totalCPS;
        updateScore();
    }, 500);
});
</script>

<script src="https://code.jquery.com/jquery-3.3.1.slim.min.js" integrity="sha384-q8i/X+965DzO0rT7abK41JStQIAqVgRVzpbzo5smXKp4YfRvH+8abtTE1Pi6jizo" crossorigin="anonymous"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/popper.js/1.14.7/umd/popper.min.js" integrity="sha384-UO2eT0CpHqdSJQ6hJty5KVphtPhzWj9WO1clHTMGa3JDZwrnQq4sF86dIHNDz0W1" crossorigin="anonymous"></script>
<script src="https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/js/bootstrap.min.js" integrity="sha384-JjSmVgyd0p3pXB1rRibZUAYoIIy6OrQ6VrjIEaFf/nJGzIxFDsf4x0xIM+B07jRM" crossorigin="anonymous"></script>
