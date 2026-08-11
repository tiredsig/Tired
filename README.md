<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Spread Love</title>
    <style>
        /* Base page styling */
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            user-select: none; /* Prevents text highlighting on quick taps */
            -webkit-user-select: none;
        }

        body {
            background-color: #ffb6c1; /* Soft pink background */
            color: #ffffff;            /* White text */
            font-family: 'Segoe UI', Roboto, Helvetica, Arial, sans-serif;
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            overflow: hidden;         /* Keeps flying hearts inside the screen */
            position: relative;
            cursor: pointer;          /* Shows the whole screen is interactable */
        }

        /* Central text block */
        .message-container {
            text-align: center;
            padding: 20px;
            z-index: 10;              /* Keeps text layered above background effects */
            pointer-events: none;     /* Allows clicks to pass through to the body */
        }

        h1 {
            font-size: 3.5rem;
            font-weight: 700;
            letter-spacing: 2px;
            text-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
            transition: opacity 0.4s ease, transform 0.4s ease;
        }

        p {
            font-size: 1.8rem;
            margin-top: 15px;
            opacity: 0;               /* Subtitle hidden initially */
            transform: translateY(10px);
            transition: opacity 0.6s ease 0.2s, transform 0.6s ease 0.2s; /* Delayed fade-in */
        }

        /* Classes to activate when clicked */
        .revealed p {
            opacity: 0.9;
            transform: translateY(0);
        }

        /* Falling/Floating heart structures */
        .heart-effect {
            position: absolute;
            font-size: 2rem;
            color: #ffffff;
            pointer-events: none;     /* Prevents hearts from blocking user clicks */
            animation: floatUp 3s linear forwards;
            opacity: 0.8;
        }

        /* Floating motion animation */
        @keyframes floatUp {
            0% {
                transform: translateY(100vh) scale(0.5) rotate(0deg);
                opacity: 0.8;
            }
            50% {
                opacity: 1;
            }
            100% {
                transform: translateY(-10vh) scale(1.3) rotate(360deg);
                opacity: 0;
            }
        }
    </style>
</head>
<body>

    <div class="message-container" id="textWrapper">
        <h1 id="mainHeading">tap me</h1>
        <p id="subHeading">spread love.</p>
    </div>

    <script>
        // Grab elements from the page
        const body = document.body;
        const mainHeading = document.getElementById('mainHeading');
        const textWrapper = document.getElementById('textWrapper');

        // Keep track of whether the screen has been tapped once
        let hasBeenTapped = false;

        // Listen for taps/clicks anywhere on the background screen
        body.addEventListener('click', (event) => {
            
            // 1. Handle text change on first click
            if (!hasBeenTapped) {
                mainHeading.textContent = "love you";
                textWrapper.classList.add('revealed');
                hasBeenTapped = true;
            }

            // 2. Spawn a heart effect at the exact location of the click
            createHeart(event.clientX, event.clientY);
        });

        // Function to build and launch floating hearts
        function createHeart(x, y) {
            const heart = document.createElement('div');
            heart.classList.add('heart-effect');
            heart.innerHTML = '❤️'; // Uses standard white text color overlay or pure emoji

            // Randomize starting location slightly around the cursor position
            const randomOffset = () => (Math.random() - 0.5) * 40; 
            heart.style.left = (x + randomOffset()) + 'px';
            
            // Lock the vertical starting height to where you clicked
            heart.style.top = y + 'px';

            // Randomize size scale for visual variety
            const randomScale = 0.5 + Math.random() * 1.5;
            heart.style.transform = `scale(${randomScale})`;

            // Randomize how long it takes to float off screen (between 2 to 4 seconds)
            const randomDuration = 2 + Math.random() * 2;
            heart.style.animationDuration = randomDuration + 's';

            // Throw it into the HTML structure
            body.appendChild(heart);

            // Clean up the memory by deleting the heart element once it finishes floating away
            setTimeout(() => {
                heart.remove();
            }, randomDuration * 1000);
        }
    </script>
</body>
</html>
