<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Bouncing Windows</title>
    <style>
        body {
            margin: 0;
            padding: 0;
            background-image: url('https://windows-web.vercel.app/some-image.jpg'); /* Replace with actual image link */
            background-size: cover;
            background-position: center;
            font-family: Arial, sans-serif;
            overflow: hidden;
            border: 5px solid #000; /* Border around the entire page */
        }

        .window {
            width: 300px;
            height: 200px;
            background-color: #f8f8f8; /* Initial window background color */
            border: 2px solid #ccc;
            position: absolute;
            top: 50px;
            left: 50px;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
            z-index: 100;
            display: flex;
            flex-direction: column;
        }

        .window-header {
            background-color: #28a745; /* Initial header background color */
            color: #fff;
            padding: 10px;
            cursor: move;
            user-select: none;
        }

        .window-content {
            flex: 1;
            padding: 10px;
            overflow: auto;
        }

        .window-buttons {
            display: flex;
            justify-content: flex-end;
        }

        .close-btn {
            background-color: #dc3545; /* Initial close button color */
            color: white;
            border: none;
            padding: 5px 10px;
            cursor: pointer;
        }
    </style>
</head>
<body>

    <div id="window1" class="window" style="top: 50px; left: 50px;">
        <div class="window-header">
            大病毒 <!-- Updated title here -->
            <div class="window-buttons">
                <button class="close-btn">X</button>
            </div>
        </div>
        <div class="window-content">
            我是一個大病毒，請不要關閉這個窗口，因為你會後悔的，並且沒有更多的文件給你，因為我要把它們全部刪除
        </div>
    </div>

    <script>
        // Track the number of windows created to generate unique IDs
        let windowCounter = 1;

        // List of HEX color codes to cycle through
        const colors = [
            '#FF5733', '#33FF57', '#3357FF', '#F1C40F', '#8E44AD', 
            '#1ABC9C', '#E74C3C', '#34495E', '#F39C12', '#7F8C8D',
            '#16A085', '#9B59B6', '#D35400', '#27AE60', '#2980B9'
        ];

        let colorIndex = 0;

        // Function to cycle through colors
        function cycleColors(windowElement) {
            // Change window background color
            windowElement.style.backgroundColor = colors[colorIndex % colors.length];
            
            // Change header background color
            const header = windowElement.querySelector('.window-header');
            header.style.backgroundColor = colors[(colorIndex + 1) % colors.length];

            // Change close button color
            const closeButton = windowElement.querySelector('.close-btn');
            closeButton.style.backgroundColor = colors[(colorIndex + 2) % colors.length];

            colorIndex++;
        }

        // Allow dragging of windows
        function makeDraggable(windowElement) {
            const header = windowElement.querySelector('.window-header');
            header.onmousedown = function(e) {
                let offsetX = e.clientX - windowElement.getBoundingClientRect().left;
                let offsetY = e.clientY - windowElement.getBoundingClientRect().top;

                document.onmousemove = function(e) {
                    let newX = e.clientX - offsetX;
                    let newY = e.clientY - offsetY;

                    // Ensure the window stays within the viewport bounds
                    const minX = 0;
                    const minY = 0;
                    const maxX = window.innerWidth - windowElement.offsetWidth;
                    const maxY = window.innerHeight - windowElement.offsetHeight;

                    // Clamp the window within the bounds
                    newX = Math.max(minX, Math.min(newX, maxX));
                    newY = Math.max(minY, Math.min(newY, maxY));

                    windowElement.style.left = newX + 'px';
                    windowElement.style.top = newY + 'px';
                };

                document.onmouseup = function() {
                    document.onmousemove = null;
                    document.onmouseup = null;
                };
            };
        }

        // Function to create and add a new window
        function createNewWindow() {
            const newWindow = document.createElement('div');
            newWindow.classList.add('window');
            newWindow.id = 'window' + (++windowCounter); // Create a unique ID for the new window

            const window1 = document.getElementById('window1');
            const windowContent = window1.querySelector('.window-content').innerHTML;
            const windowHeader = window1.querySelector('.window-header').innerHTML;

            newWindow.style.top = (50 * windowCounter) + 'px'; 
            newWindow.style.left = (50 * windowCounter) + 'px';

            newWindow.innerHTML = `
                <div class="window-header">
                    ${windowHeader}
                </div>
                <div class="window-content">
                    ${windowContent}
                </div>
            `;

            // Create a new close button
            const closeButton = document.createElement('button');
            closeButton.classList.add('close-btn');
            closeButton.innerText = 'X';

            closeButton.addEventListener('click', function() {
                increaseSpeed(newWindow); // Increase speed when close button is clicked
                createNewWindow(); // Duplicate window when close button is clicked
            });

            const windowButtons = newWindow.querySelector('.window-header .window-buttons');
            windowButtons.appendChild(closeButton);

            document.body.appendChild(newWindow);

            // Make the window draggable
            makeDraggable(newWindow);

            // Set up window bouncing and make sure duplicates bounce
            increaseSpeed(newWindow);  // Give it initial speed
            setUpBouncing(newWindow);  // Start the bouncing animation

            // Start rapidly changing the window's colors
            setInterval(() => cycleColors(newWindow), 100);  // Change colors every 100ms
        }

        // Function to increase the speed of the window's bouncing
        function increaseSpeed(windowElement) {
            // Increase the velocity of the window
            const velocityIncrease = 1.5; // Increase speed by 1.5 times each time

            if (!windowElement.velocityX || !windowElement.velocityY) {
                windowElement.velocityX = Math.random() * 3 + 2; // Initial X velocity
                windowElement.velocityY = Math.random() * 3 + 2; // Initial Y velocity
            }

            windowElement.velocityX *= velocityIncrease;
            windowElement.velocityY *= velocityIncrease;
        }

        // Function to make windows bounce
        function setUpBouncing(windowElement) {
            // Animation loop
            function bounce() {
                const rect = windowElement.getBoundingClientRect();
                const maxX = window.innerWidth - windowElement.offsetWidth;
                const maxY = window.innerHeight - windowElement.offsetHeight;

                // Update position
                let newX = rect.left + windowElement.velocityX;
                let newY = rect.top + windowElement.velocityY;

                // Check for collision with the right or left edge
                if (newX <= 0 || newX >= maxX) {
                    windowElement.velocityX = -windowElement.velocityX; // Reverse X direction
                }

                // Check for collision with the top or bottom edge
                if (newY <= 0 || newY >= maxY) {
                    windowElement.velocityY = -windowElement.velocityY; // Reverse Y direction
                }

                // Set new position
                windowElement.style.left = newX + 'px';
                windowElement.style.top = newY + 'px';

                // Continue the bouncing animation
                requestAnimationFrame(bounce);
            }

            // Start the bounce animation
            bounce();
        }

        // Add event listener to the original window's close button
        const originalCloseButton = document.querySelector('#window1 .close-btn');
        originalCloseButton.addEventListener('click', function() {
            increaseSpeed(document.getElementById('window1')); // Increase speed when close button is clicked
            createNewWindow(); // Duplicate window when close button is clicked
        });

        // Make the original window draggable
        makeDraggable(document.getElementById('window1'));

        // Set up bouncing for the first window
        increaseSpeed(document.getElementById('window1')); // Start with a random speed for the first window
        setUpBouncing(document.getElementById('window1')); // Make the first window bounce

        // Start rapidly changing the first window's colors
        setInterval(() => cycleColors(document.getElementById('window1')), 100);  // Change colors every 100ms

    </script>

</body>
</html>
