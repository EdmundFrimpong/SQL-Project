# SQL-Project
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Coming Soon Projects</title>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Montserrat:wght@700&display=swap');
        
        :root {
            --primary-color: #ff8c00;
            --secondary-color: #ffd700;
            --background-color: black;
            --text-color: white;
            --card-radius: 10px;
            --shadow-color: rgba(0, 0, 0, 0.4);
        }

        body {
            font-family: 'Montserrat', sans-serif;
            background: var(--background-color);
            color: var(--text-color);
            text-align: center;
            padding: 50px;
            margin: 0;
        }

        .container {
            display: flex;
            flex-direction: column;
            align-items: center;
            gap: 20px;
        }

        h1 {
            font-size: 48px;
            margin-bottom: 40px;
        }

        .project {
            margin-bottom: 20px;
            padding: 20px;
            background: rgba(0, 0, 0, 0.7);
            border-radius: var(--card-radius);
            font-size: 24px;
            font-weight: bold;
            color: var(--text-color);
            box-shadow: 0 4px 8px var(--shadow-color);
            position: relative;
        }

        .coming-soon {
            font-size: 48px;
            color: var(--primary-color);
            animation: comingSoonAnimation 2s infinite alternate;
        }

        @keyframes comingSoonAnimation {
            0% {
                transform: scale(1);
                color: var(--primary-color);
            }
            100% {
                transform: scale(1.2);
                color: var(--secondary-color);
            }
        }

        .absurd-animation {
            font-size: 64px;
            animation: absurdAnimation 1s infinite;
        }

        @keyframes absurdAnimation {
            0% { transform: rotate(0deg); }
            25% { transform: rotate(45deg); }
            50% { transform: rotate(0deg); }
            75% { transform: rotate(-45deg); }
            100% { transform: rotate(0deg); }
        }

        @media (max-width: 768px) {
            h1 {
                font-size: 36px;
            }
            .project {
                font-size: 20px;
            }
        }
    </style>
</head>
<body>
    <h1>Project Landing Page</h1>
    <div class="container">
        <div class="project">
            <h2>SQL Project</h2>
            <p class="coming-soon absurd-animation">COMING SOON!!!</p>
        </div>
        <div class="project">
            <h2>Capstone Project</h2>
            <p class="coming-soon absurd-animation">COMING SOON!!!</p>
        </div>
        <div class="project">
            <h2>Excel Project</h2>
            <p class="coming-soon absurd-animation">COMING SOON!!!</p>
        </div>
    </div>
</body>
</html>
