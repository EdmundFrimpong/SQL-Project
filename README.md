<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>SQL Project - Coming Soon</title>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Montserrat:wght@700&display=swap');

        body {
            font-family: 'Montserrat', sans-serif;
            background: #f0f0f0;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            margin: 0;
            text-align: center;
        }

        .coming-soon-container {
            background: #ffffff;
            padding: 50px;
            border-radius: 10px;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
            position: relative;
        }

        .coming-soon {
            font-size: 48px;
            color: #ff8c00;
            animation: pulse 1.5s infinite;
        }

        @keyframes pulse {
            0% {
                transform: scale(1);
            }
            50% {
                transform: scale(1.1);
            }
            100% {
                transform: scale(1);
            }
        }
    </style>
</head>
<body>
    <div class="coming-soon-container">
        <h1>SQL Project</h1>
        <p class="coming-soon">COMING SOON</p>
    </div>
</body>
</html>
