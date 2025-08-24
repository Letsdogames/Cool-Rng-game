<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>RNG Character Gacha Game</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #f4f4f4;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            min-height: 100vh;
            margin: 0;
            padding: 20px;
        }
        .top-buttons {
            display: flex;
            gap: 10px;
            margin-bottom: 20px;
            justify-content: center;
        }
        .top-button {
            padding: 10px 40px;
            font-size: 18px;
            cursor: pointer;
            border: none;
            border-radius: 5px;
            color: white;
            transition: background-color 0.3s ease;
        }
        .top-button.shop {
            background-color: #ff9800;
        }
        .top-button.shop:hover {
            background-color: #e68a00;
        }
        .top-button.missions {
            background-color: #2196F3;
        }
        .top-button.missions:hover {
            background-color: #1e87e5;
        }
        .main-container {
            display: flex;
            flex-direction: row;
            align-items: flex-start;
            gap: 30px;
            background-color: #fff;
            padding: 20px;
            border-radius: 10px;
            box-shadow: 0 4px 10px rgba(0,0,0,0.1);
        }
        #grey-box {
            background-color: #d3d3d3;
            width: 450px;
            height: 150px;
            border-radius: 10px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 20px;
            font-weight: bold;
            text-align: center;
            overflow: hidden;
            position: relative;
            border-top: 3px solid #333;
            border-bottom: 3px solid #333;
            transition: box-shadow 0.3s ease;
        }
        #grey-box:hover {
            box-shadow: 0 0 15px rgba(0,0,0,0.2);
        }
        #grey-box.flash {
            animation: flash 0.5s ease;
        }
        @keyframes flash {
            0% { background-color: #d3d3d3; }
            50% { background-color: #e0e0e0; }
            100% { background-color: #d3d3d3; }
        }
        #grey-box .name-container {
            position: absolute;
            transition: transform 0.1s linear;
            white-space: pre-wrap;
            will-change: transform;
        }
        #grey-box .name-container.multi-roll-summary {
            font-size: 14px;
            display: flex;
            flex-direction: row;
            justify-content: space-between;
            width: 100%;
            padding: 10px;
            box-sizing: border-box;
            word-break: break-word;
            overflow-wrap: break-word;
        }
        #grey-box .name-container.multi-roll-summary .column {
            display: flex;
            flex-direction: column;
            align-items: flex-start;
            width: 48%;
        }
        #grey-box .name-container.multi-roll-summary .number {
            display: inline-block;
            width: 30px;
            text-align: right;
        }
        #roll-button {
            padding: 10px 40px;
            font-size: 18px;
            cursor: pointer;
            border: none;
            border-radius: 5px;
            color: white;
            background-color: #4CAF50;
            margin: 10px 0;
            transition: background-color 0.3s ease;
        }
        #roll-button:hover {
            background-color: #45a049;
        }
        #multi-roll {
            padding: 10px 40px;
            font-size: 18px;
            cursor: pointer;
            border: none;
            border-radius: 5px;
            color: white;
            background-color: #2196F3;
            margin: 10px 0;
            transition: background-color 0.3s ease;
        }
        #multi-roll:hover {
            background-color: #1e87e5;
        }
        #main-gold-balance {
            font-size: 18px;
            font-weight: bold;
            color: #ff9800;
            margin: 5px 0;
        }
        #your-cards {
            width: 300px;
        }
        #your-cards h2 {
            font-size: 20px;
            margin: 0;
            color: #333;
            cursor: pointer;
            display: flex;
            align-items: center;
            gap: 5px;
        }
        #your-cards h2 .arrow {
            transition: transform 0.3s ease;
        }
        #your-cards h2 .arrow.open {
            transform: rotate(180deg);
        }
        #your-cards h2 .notification-dot {
            background-color: red;
            color: white;
            border-radius: 50%;
            width: 20px;
            height: 20px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 12px;
            font-weight: bold;
        }
        #cards-container {
            max-height: 0;
            overflow: hidden;
            transition: max-height 0.3s ease;
        }
        #cards-container.open {
            max-height: 300px;
            overflow-y: auto;
        }
        .card {
            background-color: white;
            border: 2px solid #333;
            border-radius: 8px;
            padding: 10px;
            margin: 10px 0;
            box-shadow: 0 2px 5px rgba(0,0,0,0.2);
            text-align: center;
            transition: transform 0.2s ease;
            position: relative;
        }
        .card:hover {
            transform: scale(1.05);
        }
        .rarity-common { color: gray; border-color: gray; }
        .rarity-rare { color: blue; border-color: blue; }
        .rarity-epic { color: purple; border-color: purple; }
        .rarity-legendary { color: gold; border-color: gold; font-weight: bold; }
        .sell-button {
            position: absolute;
            top: 10px;
            right: 10px;
            padding: 5px 10px;
            font-size: 12px;
            cursor: pointer;
            border: none;
            border-radius: 5px;
            color: white;
            background-color: #e74c3c;
            transition: background-color 0.3s ease;
        }
        .sell-button:hover {
            background-color: #c0392b;
        }
        .filter-buttons {
            display: none;
            gap: 5px;
            margin: 10px 0;
            flex-wrap: wrap;
        }
        #cards-container.open ~ .filter-buttons {
            display: flex;
        }
        .filter-button {
            padding: 5px 10px;
            font-size: 14px;
            cursor: pointer;
            border: none;
            border-radius: 5px;
            color: white;
            transition: background-color 0.3s ease;
        }
        .filter-button.common { background-color: gray; }
        .filter-button.common:hover { background-color: #555; }
        .filter-button.rare { background-color: blue; }
        .filter-button.rare:hover { background-color: #0055ff; }
        .filter-button.epic { background-color: purple; }
        .filter-button.epic:hover { background-color: #660099; }
        .filter-button.legendary { background-color: gold; color: black; }
        .filter-button.legendary:hover { background-color: #ffcc00; }
        .filter-button.all { background-color: #666; }
        .filter-button.all:hover { background-color: #555; }
        .filter-button.active {
            filter: brightness(1.2);
            box-shadow: 0 0 5px rgba(0,0,0,0.3);
        }
        .shop-menu {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background-color: rgba(0, 0, 0, 0.8);
            z-index: 1000;
            align-items: center;
            justify-content: center;
        }
        .shop-menu.open {
            display: flex;
        }
        .missions-menu {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background-color: rgba(0, 0, 0, 0.8);
            z-index: 1000;
            align-items: center;
            justify-content: center;
        }
        .missions-menu.open {
            display: flex;
        }
        .shop-content, .missions-content {
            background-color: #fff;
            border-radius: 10px;
            padding: 20px;
            width: 80%;
            max-width: 600px;
            max-height: 80vh;
            overflow-y: auto;
            box-shadow: 0 4px 10px rgba(0,0,0,0.3);
        }
        .shop-header, .missions-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 20px;
        }
        .shop-header h2, .missions-header h2 {
            margin: 0;
            font-size: 24px;
            color: #333;
        }
        .gold-balance {
            font-size: 18px;
            font-weight: bold;
            color: #ff9800;
        }
        .back-button {
            padding: 8px 16px;
            font-size: 16px;
            cursor: pointer;
            border: none;
            border-radius: 5px;
            color: white;
            background-color: #666;
            transition: background-color 0.3s ease;
        }
        .back-button:hover {
            background-color: #555;
        }
        .character-item {
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 10px;
            margin: 10px 0;
            border: 2px solid #333;
            border-radius: 8px;
            background-color: #f9f9f9;
            transition: transform 0.2s ease;
        }
        .character-item:hover {
            transform: scale(1.02);
        }
        .character-info {
            flex-grow: 1;
        }
        .character-item h3 {
            margin: 0;
            font-size: 18px;
        }
        .character-item p {
            margin: 5px 0;
            font-size: 14px;
            color: #555;
        }
        .buy-button {
            padding: 8px 16px;
            font-size: 14px;
            cursor: pointer;
            border: none;
            border-radius: 5px;
            color: white;
            background-color: #4CAF50;
            transition: background-color 0.3s ease;
        }
        .buy-button:hover {
            background-color: #45a049;
        }
        .buy-button:disabled {
            background-color: #ccc;
            cursor: not-allowed;
        }
        .mission-item, .achievement-item {
            padding: 10px;
            margin: 10px 0;
            border: 2px solid #333;
            border-radius: 8px;
            background-color: #f9f9f9;
        }
        .mission-item.completed, .achievement-item.completed {
            border-color: #4CAF50;
            box-shadow: 0 0 10px rgba(76, 175, 80, 0.5);
        }
        .mission-item h3, .achievement-item h3 {
            margin: 0;
            font-size: 18px;
            color: #333;
        }
        .mission-item p, .achievement-item p {
            margin: 5px 0;
            font-size: 14px;
            color: #555;
        }
        .claim-button {
            padding: 8px 16px;
            font-size: 14px;
            cursor: pointer;
            border: none;
            border-radius: 5px;
            color: white;
            background-color: #4CAF50;
            transition: background-color 0.3s ease;
        }
        .claim-button:hover {
            background-color: #45a049;
        }
        .claim-button:disabled {
            background-color: #ccc;
            cursor: not-allowed;
        }
        @media screen and (max-width: 600px) {
            .top-buttons {
                flex-direction: column;
                width: 100%;
                align-items: center;
            }
            .top-button {
                width: 100%;
                padding: 2vw 4vw;
                font-size: min(5vw, 16px);
            }
            .main-container {
                flex-direction: column;
                align-items: center;
                width: 100%;
                padding: 3vw;
            }
            #grey-box {
                width: 100%;
                height: 25vh;
                min-height: 120px;
                font-size: min(5vw, 16px);
            }
            #grey-box .name-container.multi-roll-summary {
                font-size: min(4vw, 12px);
            }
            #grey-box .name-container.multi-roll-summary .number {
                width: 25px;
            }
            #your-cards {
                width: 100%;
            }
            #roll-button, #multi-roll {
                width: 100%;
                padding: 2vw 4vw;
                font-size: min(5vw, 16px);
            }
            #main-gold-balance {
                font-size: min(5vw, 16px);
                margin: 2vw 0;
            }
            #your-cards h2 {
                font-size: min(5vw, 18px);
            }
            #your-cards h2 .notification-dot {
                width: min(5vw, 18px);
                height: min(5vw, 18px);
                font-size: min(3vw, 10px);
            }
            .card {
                padding: 2vw;
                margin: 2vw 0;
            }
            .sell-button {
                padding: 1vw 2vw;
                font-size: min(3vw, 10px);
            }
            .filter-button {
                padding: 1vw 2vw;
                font-size: min(4vw, 14px);
            }
            #cards-container.open {
                max-height: 50vh;
            }
            .shop-content, .missions-content {
                width: 90%;
                padding: 3vw;
            }
            .shop-header h2, .missions-header h2 {
                font-size: min(6vw, 20px);
            }
            .gold-balance {
                font-size: min(5vw, 16px);
            }
            .back-button {
                padding: 2vw 4vw;
                font-size: min(4vw, 14px);
            }
            .character-item, .mission-item, .achievement-item {
                padding: 2vw;
                margin: 2vw 0;
            }
            .character-item h3, .mission-item h3, .achievement-item h3 {
                font-size: min(5vw, 16px);
            }
            .character-item p, .mission-item p, .achievement-item p {
                font-size: min(4vw, 12px);
            }
            .buy-button, .claim-button {
                padding: 2vw 4vw;
                font-size: min(4vw, 12px);
            }
        }
        @media screen and (min-width: 601px) and (max-width: 900px) {
            .top-buttons {
                flex-direction: row;
                width: 100%;
                justify-content: center;
            }
            .top-button {
                width: auto;
                padding: 1.5vw 3vw;
                font-size: min(4.5vw, 17px);
            }
            .main-container {
                flex-direction: row;
                flex-wrap: wrap;
                width: 100%;
                padding: 2vw;
            }
            #grey-box {
                width: 45%;
                height: 20vh;
                min-height: 120px;
                font-size: min(4.5vw, 18px);
            }
            #grey-box .name-container.multi-roll-summary {
                font-size: min(3.5vw, 13px);
            }
            #grey-box .name-container.multi-roll-summary .number {
                width: 25px;
            }
            #your-cards {
                width: 45%;
            }
            #roll-button, #multi-roll {
                width: 100%;
                max-width: 180px;
                padding: 1.5vw 3vw;
                font-size: min(4.5vw, 17px);
            }
            #main-gold-balance {
                font-size: min(4.5vw, 17px);
                margin: 1.5vw 0;
            }
            #your-cards h2 {
                font-size: min(4.5vw, 19px);
            }
            #your-cards h2 .notification-dot {
                width: min(4.5vw, 19px);
                height: min(4.5vw, 19px);
                font-size: min(3vw, 11px);
            }
            .card {
                padding: 1.5vw;
                margin: 1.5vw 0;
            }
            .sell-button {
                padding: 1vw 2vw;
                font-size: min(3.5vw, 11px);
            }
            .filter-button {
                padding: 1vw 2vw;
                font-size: min(4vw, 14px);
            }
            #cards-container.open {
                max-height: 40vh;
            }
            .shop-content, .missions-content {
                width: 85%;
                padding: 2vw;
            }
            .shop-header h2, .missions-header h2 {
                font-size: min(5vw, 22px);
            }
            .gold-balance {
                font-size: min(4.5vw, 17px);
            }
            .back-button {
                padding: 1.5vw 3vw;
                font-size: min(4vw, 15px);
            }
            .character-item, .mission-item, .achievement-item {
                padding: 1.5vw;
                margin: 1.5vw 0;
            }
            .character-item h3, .mission-item h3, .achievement-item h3 {
                font-size: min(4.5vw, 17px);
            }
            .character-item p, .mission-item p, .achievement-item p {
                font-size: min(3.5vw, 13px);
            }
            .buy-button, .claim-button {
                padding: 1.5vw 3vw;
                font-size: min(3.5vw, 13px);
            }
        }
    </style>
</head>
<body>
    <div class="top-buttons">
        <button class="top-button shop" id="top-shop-button">Shop</button>
        <button class="top-button missions" id="top-missions-button">Missions</button>
    </div>
    <div class="main-container">
        <div class="roll-section">
            <div id="grey-box"><div class="name-container">Click Roll to Start</div></div>
            <button id="roll-button">Roll</button>
            <button id="multi-roll">10x Roll</button>
            <div id="main-gold-balance">Gold: 1000</div>
        </div>
        <div id="your-cards">
            <h2>Your Cards <span class="arrow">▼</span><span class="notification-dot" style="display: none;">0</span></h2>
            <div id="cards-container"></div>
            <div class="filter-buttons">
                <button class="filter-button common" data-rarity="Common">Common</button>
                <button class="filter-button rare" data-rarity="Rare">Rare</button>
                <button class="filter-button epic" data-rarity="Epic">Epic</button>
                <button class="filter-button legendary" data-rarity="Legendary">Legendary</button>
                <button class="filter-button all active" data-rarity="all">Show All</button>
            </div>
        </div>
    </div>
    <div class="shop-menu" id="shop-menu">
        <div class="shop-content">
            <div class="shop-header">
                <h2>Shop</h2>
                <div class="gold-balance" id="gold-balance">Gold: 1000</div>
                <button class="back-button" id="shop-back-button">Back</button>
            </div>
            <div id="shop-characters"></div>
        </div>
    </div>
    <div class="missions-menu" id="missions-menu">
        <div class="missions-content">
            <div class="missions-header">
                <h2>Missions & Achievements</h2>
                <div class="gold-balance" id="missions-gold-balance">Gold: 1000</div>
                <button class="back-button" id="missions-back-button">Back</button>
            </div>
            <div id="achievements"></div>
            <div id="daily-missions"></div>
        </div>
    </div>

    <script>
        // Define rarities with fixed probabilities (sum to 100)
        const rarities = [
            { name: 'Common', probability: 85, class: 'rarity-common', sellValue: 50 },
            { name: 'Rare', probability: 10, class: 'rarity-rare', sellValue: 200 },
            { name: 'Epic', probability: 4.8, class: 'rarity-epic', sellValue: 500 },
            { name: 'Legendary', probability: 0.2, class: 'rarity-legendary', sellValue: 2000 }
        ];

        // Define characters by rarity with fixed probabilities and overall probabilities
        const characters = {
            'Common': [
                { name: 'Peasant', probability: 30, overallProbability: 25.50 },
                { name: 'Squire', probability: 25, overallProbability: 21.25 },
                { name: 'Archer', probability: 20, overallProbability: 17.00 },
                { name: 'Footman', probability: 15, overallProbability: 12.75 },
                { name: 'Scout', probability: 10, overallProbability: 8.50 }
            ],
            'Rare': [
                { name: 'Knight', probability: 30, overallProbability: 3.00 },
                { name: 'Mage', probability: 25, overallProbability: 2.50 },
                { name: 'Assassin', probability: 20, overallProbability: 2.00 },
                { name: 'Healer', probability: 15, overallProbability: 1.50 },
                { name: 'Berserker', probability: 10, overallProbability: 1.00 }
            ],
            'Epic': [
                { name: 'Dragon Rider', probability: 33.33, overallProbability: 1.60 },
                { name: 'Elemental Sorcerer', probability: 33.33, overallProbability: 1.60 },
                { name: 'Shadow Ninja', probability: 22.22, overallProbability: 1.07 },
                { name: 'Holy Paladin', probability: 11.12, overallProbability: 0.53 }
            ],
            'Legendary': [
                { name: 'God Emperor', probability: 40, overallProbability: 0.08 },
                { name: 'Phoenix Lord', probability: 30, overallProbability: 0.06 },
                { name: 'Void Walker', probability: 30, overallProbability: 0.06 }
            ]
        };

        // Assign unique gold costs to each character based on rarity
        for (let rarityName in characters) {
            characters[rarityName].forEach(character => {
                if (rarityName === 'Common') {
                    character.goldCost = Math.round(1000 / character.overallProbability) * 5;
                } else if (rarityName === 'Rare') {
                    character.goldCost = Math.round(3000 / character.overallProbability) * 10;
                } else if (rarityName === 'Epic') {
                    character.goldCost = Math.round(5000 / character.overallProbability) * 20;
                } else if (rarityName === 'Legendary') {
                    character.goldCost = Math.round(10000 / character.overallProbability) * 50;
                }
            });
        }

        // Game state
        let goldBalance = parseInt(localStorage.getItem('goldBalance')) || 1000;
        let characterCounts = JSON.parse(localStorage.getItem('characterCounts')) || {};
        let rollCount = parseInt(localStorage.getItem('rollCount')) || 0;
        let multiRollCount = parseInt(localStorage.getItem('multiRollCount')) || 0;
        let unviewedCount = parseInt(localStorage.getItem('unviewedCount')) || 0;
        let rollsSinceLegendary = parseInt(localStorage.getItem('rollsSinceLegendary')) || 0;
        const pityThreshold = 100;
        let currentFilter = localStorage.getItem('currentFilter') || 'all';
        let dailyGoldEarned = parseInt(localStorage.getItem('dailyGoldEarned')) || 0;
        let lastGoldReset = parseInt(localStorage.getItem('lastGoldReset')) || Date.now();
        let dailyMissions = [];
        try {
            dailyMissions = JSON.parse(localStorage.getItem('dailyMissions')) || [];
        } catch (e) {
            console.error('Error loading dailyMissions from localStorage:', e);
            dailyMissions = [];
            localStorage.removeItem('dailyMissions');
        }
        let lastMissionReset = parseInt(localStorage.getItem('lastMissionReset')) || Date.now();

        // Achievements
        const achievements = [
            { id: 'rolls10', name: 'Roll 10 Times', condition: () => rollCount >= 10, reward: 100, claimed: false },
            { id: 'rolls50', name: 'Roll 50 Times', condition: () => rollCount >= 50, reward: 500, claimed: false },
            { id: 'rolls100', name: 'Roll 100 Times', condition: () => rollCount >= 100, reward: 2000, claimed: false },
            { id: 'allCommon', name: 'Collect All Common', condition: () => Object.keys(characterCounts['Common'] || {}).length === characters['Common'].length, reward: 500, claimed: false },
            { id: 'allRare', name: 'Collect All Rare', condition: () => Object.keys(characterCounts['Rare'] || {}).length === characters['Rare'].length, reward: 2000, claimed: false }
        ];

        // Load and merge saved achievements
        try {
            const savedAchievements = JSON.parse(localStorage.getItem('achievements')) || {};
            achievements.forEach(ach => {
                if (savedAchievements[ach.id] && typeof savedAchievements[ach.id].claimed === 'boolean') {
                    ach.claimed = savedAchievements[ach.id].claimed;
                }
            });
        } catch (e) {
            console.error('Error loading achievements from localStorage:', e);
            localStorage.removeItem('achievements');
        }

        // Daily Missions
        const missionTemplates = [
            { id: 'roll5', name: 'Perform 5 Single Rolls', progress: 0, goal: 5, reward: 100, type: 'singleRoll' },
            { id: 'openShop3', name: 'Open Shop 3 Times', progress: 0, goal: 3, reward: 150, type: 'openShop' },
            { id: 'getRare', name: 'Obtain 1 Rare Character', progress: 0, goal: 1, reward: 300, type: 'getRare' },
            { id: 'multiRoll2', name: 'Perform 2 Multi-Rolls', progress: 0, goal: 2, reward: 200, type: 'multiRoll' },
            { id: 'sellDuplicate', name: 'Sell 1 Duplicate Card', progress: 0, goal: 1, reward: 100, type: 'sellDuplicate' },
            { id: 'getEpic', name: 'Obtain 1 Epic Character', progress: 0, goal: 1, reward: 400, type: 'getEpic' }
        ];

        // Function to generate 3 random missions
        function generateDailyMissions() {
            const availableTemplates = [...missionTemplates];
            dailyMissions = [];
            for (let i = 0; i < 3 && availableTemplates.length > 0; i++) {
                const index = Math.floor(Math.random() * availableTemplates.length);
                const mission = { ...availableTemplates[index], progress: 0, claimedAt: null };
                dailyMissions.push(mission);
                availableTemplates.splice(index, 1);
            }
            lastMissionReset = Date.now();
            saveGameState();
        }

        // Function to save game state
        function saveGameState() {
            localStorage.setItem('goldBalance', goldBalance);
            localStorage.setItem('characterCounts', JSON.stringify(characterCounts));
            localStorage.setItem('rollCount', rollCount);
            localStorage.setItem('multiRollCount', multiRollCount);
            localStorage.setItem('unviewedCount', unviewedCount);
            localStorage.setItem('rollsSinceLegendary', rollsSinceLegendary);
            localStorage.setItem('currentFilter', currentFilter);
            localStorage.setItem('dailyGoldEarned', dailyGoldEarned);
            localStorage.setItem('lastGoldReset', lastGoldReset);
            localStorage.setItem('achievements', JSON.stringify(achievements.reduce((acc, ach) => ({ ...acc, [ach.id]: { claimed: ach.claimed } }), {})));
            localStorage.setItem('dailyMissions', JSON.stringify(dailyMissions));
            localStorage.setItem('lastMissionReset', lastMissionReset);
        }

        // Function to check and reset daily missions and gold cap
        function checkDailyReset() {
            const now = Date.now();
            const today = new Date(now).toUTCString().split(' ')[1] + ' ' + new Date(now).toUTCString().split(' ')[2];
            const lastReset = new Date(lastMissionReset).toUTCString().split(' ')[1] + ' ' + new Date(lastMissionReset).toUTCString().split(' ')[2];
            if (today !== lastReset) {
                generateDailyMissions();
                dailyGoldEarned = 0;
                lastGoldReset = now;
                saveGameState();
            }
        }

        // Function to replace a claimed mission
        function replaceMission(missionId) {
            const missionIndex = dailyMissions.findIndex(m => m.id === missionId);
            if (missionIndex === -1) return;

            const availableTemplates = missionTemplates.filter(mt => !dailyMissions.some(dm => dm.id === mt.id));
            if (availableTemplates.length === 0) {
                dailyMissions.splice(missionIndex, 1);
            } else {
                const newMissionIndex = Math.floor(Math.random() * availableTemplates.length);
                const newMission = { ...availableTemplates[newMissionIndex], progress: 0, claimedAt: null };
                dailyMissions[missionIndex] = newMission;
            }

            // Ensure exactly 3 missions if possible
            while (dailyMissions.length < 3 && availableTemplates.length > 0) {
                const remainingTemplates = missionTemplates.filter(mt => !dailyMissions.some(dm => dm.id === mt.id));
                if (remainingTemplates.length === 0) break;
                const newMissionIndex = Math.floor(Math.random() * remainingTemplates.length);
                const newMission = { ...remainingTemplates[newMissionIndex], progress: 0, claimedAt: null };
                dailyMissions.push(newMission);
            }

            saveGameState();
            renderMissions();
        }

        // Function to update gold balance display
        function updateGoldBalance() {
            document.getElementById('main-gold-balance').textContent = `Gold: ${goldBalance}`;
            document.getElementById('gold-balance').textContent = `Gold: ${goldBalance}`;
            document.getElementById('missions-gold-balance').textContent = `Gold: ${goldBalance}`;
            if (document.getElementById('shop-menu').classList.contains('open')) {
                renderShopCharacters();
            }
            saveGameState();
        }

        // Function to update notification dot
        function updateNotificationDot() {
            const notificationDot = document.querySelector('.notification-dot');
            if (unviewedCount > 0) {
                notificationDot.style.display = 'flex';
                notificationDot.textContent = unviewedCount;
            } else {
                notificationDot.style.display = 'none';
                notificationDot.textContent = '0';
            }
            saveGameState();
        }

        // Passive gold accumulation
        const dailyGoldCap = 200;
        setInterval(() => {
            checkDailyReset();
            if (dailyGoldEarned < dailyGoldCap) {
                const goldToAdd = Math.min(10, dailyGoldCap - dailyGoldEarned);
                goldBalance += goldToAdd;
                dailyGoldEarned += goldToAdd;
                updateGoldBalance();
            }
        }, 5 * 60 * 1000);

        // Function to get a random rarity based on fixed probabilities, with pity
        function getRandomRarity() {
            if (rollsSinceLegendary >= pityThreshold) {
                rollsSinceLegendary = 0;
                saveGameState();
                return rarities.find(r => r.name === 'Legendary');
            }
            let rand = Math.random() * 100;
            let cumulative = 0;
            for (let rarity of rarities) {
                cumulative += rarity.probability;
                if (rand < cumulative) {
                    return rarity;
                }
            }
            return rarities[rarities.length - 1];
        }

        // Function to get a random character for a given rarity
        function getRandomCharacter(rarityName) {
            let rand = Math.random() * 100;
            let cumulative = 0;
            const charList = characters[rarityName];
            for (let char of charList) {
                cumulative += char.probability;
                if (rand < cumulative) {
                    return char;
                }
            }
            return charList[charList.length - 1];
        }

        // Function to update mission progress
        function updateMissionProgress(type, increment = 1) {
            checkDailyReset();
            dailyMissions.forEach(mission => {
                if (mission.type === type && mission.progress < mission.goal && !mission.claimedAt) {
                    mission.progress = Math.min(mission.progress + increment, mission.goal);
                }
            });
            dailyMissions.forEach(mission => {
                if (mission.claimedAt && Date.now() - mission.claimedAt >= 2 * 60 * 60 * 1000) {
                    replaceMission(mission.id);
                }
            });
            renderMissions();
            saveGameState();
        }

        // Function to perform a single roll
        function performSingleRoll() {
            rollCount++;
            rollsSinceLegendary++;
            goldBalance += 50;
            updateGoldBalance();
            updateMissionProgress('singleRoll');
            const greyBox = document.getElementById('grey-box');
            greyBox.classList.add('flash');
            setTimeout(() => greyBox.classList.remove('flash'), 500);
            animateRoll((rarity, character) => {
                addToCards(rarity, character);
                if (rarity.name === 'Legendary') {
                    rollsSinceLegendary = 0;
                }
                if (rarity.name === 'Rare') {
                    updateMissionProgress('getRare');
                }
                if (rarity.name === 'Epic') {
                    updateMissionProgress('getEpic');
                }
            }, 5000);
            setTimeout(() => {
                const nameContainer = document.getElementById('grey-box').querySelector('.name-container');
                nameContainer.innerHTML = 'Click Roll to Start';
                nameContainer.style.color = 'black';
                nameContainer.classList.remove('multi-roll-summary');
            }, 7000);
            renderMissions();
            saveGameState();
        }

        // Function to display multi-roll summary
        function displayMultiRollSummary(results) {
            const nameContainer = document.getElementById('grey-box').querySelector('.name-container');
            const leftColumn = results.slice(0, 5).map(({ rarity, character }, index) => 
                `<span class="number">${index + 1}.</span> ${rarity.name}:${character.name}`
            ).join('\n');
            const rightColumn = results.slice(5).map(({ rarity, character }, index) => 
                `<span class="number">${index + 6}.</span> ${rarity.name}:${character.name}`
            ).join('\n');
            nameContainer.innerHTML = `<div class="column">${leftColumn}</div><div class="column">${rightColumn}</div>`;
            nameContainer.style.color = 'black';
            nameContainer.style.animation = 'none';
            nameContainer.style.transform = 'translateY(0)';
            nameContainer.classList.add('multi-roll-summary');
            setTimeout(() => {
                nameContainer.innerHTML = 'Click Roll to Start';
                nameContainer.style.color = 'black';
                nameContainer.classList.remove('multi-roll-summary');
            }, 5000);
        }

        // Function to perform multi-roll (10x)
        async function performMultiRoll() {
            multiRollCount++;
            rollCount += 10;
            rollsSinceLegendary += 10;
            goldBalance += 500;
            if (Math.random() < 0.05) {
                goldBalance += 2000;
                alert('Jackpot! Earned 2000 extra gold!');
            }
            if (multiRollCount % 5 === 0) {
                goldBalance += 1000;
                alert('5th Multi-Roll Bonus: Earned 1000 extra gold!');
            }
            updateGoldBalance();
            updateMissionProgress('multiRoll');
            const results = [];
            const greyBox = document.getElementById('grey-box');
            greyBox.classList.add('flash');
            setTimeout(() => greyBox.classList.remove('flash'), 500);
            for (let i = 0; i < 10; i++) {
                const rarity = getRandomRarity();
                const character = getRandomCharacter(rarity.name);
                results.push({ rarity, character });
                await animateRoll((r, c) => {
                    addToCards(r, c);
                    if (r.name === 'Legendary') {
                        rollsSinceLegendary = 0;
                    }
                    if (r.name === 'Rare') {
                        updateMissionProgress('getRare');
                    }
                    if (r.name === 'Epic') {
                        updateMissionProgress('getEpic');
                    }
                }, 2000, rarity, character);
            }
            displayMultiRollSummary(results);
            renderMissions();
            saveGameState();
        }

        // Function to animate rarities and characters in the grey box
        function animateRoll(callback, duration, fixedRarity = null, fixedCharacter = null) {
            return new Promise(resolve => {
                const greyBox = document.getElementById('grey-box');
                const nameContainer = greyBox.querySelector('.name-container');
                const totalNames = 20;
                let animationCount = 0;
                let currentInterval = 100;
                const slowdownFactor = 1.2;
                const maxInterval = duration / 10;

                function showRandomName() {
                    const rarity = getRandomRarity();
                    const character = getRandomCharacter(rarity.name);
                    nameContainer.innerHTML = `${rarity.name}: ${character.name}`;
                    nameContainer.style.color = rarity.class === 'rarity-common' ? 'gray' :
                                              rarity.class === 'rarity-rare' ? 'blue' :
                                              rarity.class === 'rarity-epic' ? 'purple' : 'gold';
                    nameContainer.classList.remove('multi-roll-summary');

                    nameContainer.style.animation = 'none';
                    void nameContainer.offsetWidth;
                    nameContainer.style.animation = `slide-up ${currentInterval}ms linear`;

                    animationCount++;
                    if (animationCount < totalNames) {
                        currentInterval = Math.min(currentInterval * slowdownFactor, maxInterval);
                        requestAnimationFrame(() => setTimeout(showRandomName, currentInterval));
                    } else {
                        const finalRarity = fixedRarity || getRandomRarity();
                        const finalCharacter = fixedCharacter || getRandomCharacter(finalRarity.name);
                        nameContainer.innerHTML = `You got! ${finalRarity.name}:${finalCharacter.name}`;
                        nameContainer.style.color = finalRarity.class === 'rarity-common' ? 'gray' :
                                                   finalRarity.class === 'rarity-rare' ? 'blue' :
                                                   finalRarity.class === 'rarity-epic' ? 'purple' : 'gold';
                        nameContainer.style.animation = 'none';
                        nameContainer.style.transform = 'translateY(0)';
                        nameContainer.classList.remove('multi-roll-summary');
                        callback(finalRarity, finalCharacter);
                        resolve();
                    }
                }

                requestAnimationFrame(showRandomName);
            });
        }

        // Function to render cards based on current filter
        function renderCards() {
            const cardsContainer = document.getElementById('cards-container');
            cardsContainer.innerHTML = '';

            for (let rarityName in characterCounts) {
                if (currentFilter !== 'all' && currentFilter !== rarityName) {
                    continue;
                }
                const rarity = rarities.find(r => r.name === rarityName);
                for (let charName in characterCounts[rarityName]) {
                    const character = characters[rarityName].find(c => c.name === charName);
                    const key = `${rarityName}:${charName}`;
                    const card = document.createElement('div');
                    card.classList.add('card', rarity.class);
                    card.setAttribute('data-key', key);
                    const overallProbability = character.overallProbability.toFixed(2);
                    const count = characterCounts[rarityName][charName];
                    card.innerHTML = `
                        <h3 class="${rarity.class}">${rarityName}</h3>
                        <p>${charName} (${overallProbability}%)${count > 1 ? ` cards: ${count}x` : ''}</p>
                        ${count > 1 ? `<button class="sell-button" data-rarity="${rarityName}" data-character="${charName}">Sell (1 for ${rarity.sellValue} Gold)</button>` : ''}
                    `;
                    cardsContainer.appendChild(card);
                }
            }

            // Add event listeners to sell buttons
            const sellButtons = cardsContainer.querySelectorAll('.sell-button');
            sellButtons.forEach(button => {
                button.replaceWith(button.cloneNode(true));
                const newButton = cardsContainer.querySelector(`.sell-button[data-rarity="${button.getAttribute('data-rarity')}"][data-character="${button.getAttribute('data-character')}"]`);
                newButton.addEventListener('click', () => {
                    const rarityName = newButton.getAttribute('data-rarity');
                    const characterName = newButton.getAttribute('data-character');
                    const rarity = rarities.find(r => r.name === rarityName);
                    if (characterCounts[rarityName][characterName] > 1) {
                        characterCounts[rarityName][characterName]--;
                        if (characterCounts[rarityName][characterName] === 0) {
                            delete characterCounts[rarityName][characterName];
                            if (Object.keys(characterCounts[rarityName]).length === 0) {
                                delete characterCounts[rarityName];
                            }
                        }
                        goldBalance += rarity.sellValue;
                        alert(`Sold ${rarityName}: ${characterName} for ${rarity.sellValue} Gold!`);
                        updateGoldBalance();
                        updateMissionProgress('sellDuplicate');
                        renderCards();
                        renderMissions();
                        saveGameState();
                    }
                });
            });
        }

        // Function to add result to the cards or update count
        function addToCards(rarity, character) {
            const cardsContainer = document.getElementById('cards-container');
            const key = `${rarity.name}:${character.name}`;
            
            if (!characterCounts[rarity.name]) {
                characterCounts[rarity.name] = {};
            }
            
            characterCounts[rarity.name][character.name] = (characterCounts[rarity.name][character.name] || 0) + 1;
            
            renderCards();
            renderMissions();
            
            if (!cardsContainer.classList.contains('open')) {
                unviewedCount++;
                updateNotificationDot();
            }
        }

        // Function to render shop characters
        function renderShopCharacters() {
            const shopCharacters = document.getElementById('shop-characters');
            shopCharacters.innerHTML = '';

            for (let rarity of rarities) {
                const charactersList = characters[rarity.name];
                for (let character of charactersList) {
                    const characterItem = document.createElement('div');
                    characterItem.classList.add('character-item', rarity.class);
                    const overallProbability = character.overallProbability.toFixed(2);
                    characterItem.innerHTML = `
                        <div class="character-info">
                            <h3 class="${rarity.class}">${rarity.name}: ${character.name}</h3>
                            <p>Chance to get: ${overallProbability}% | Cost: ${character.goldCost} Gold</p>
                        </div>
                        <button class="buy-button" data-rarity="${rarity.name}" data-character="${character.name}" data-cost="${character.goldCost}">Buy</button>
                    `;
                    shopCharacters.appendChild(characterItem);
                }
            }

            // Add event listeners to buy buttons
            const buyButtons = shopCharacters.querySelectorAll('.buy-button');
            buyButtons.forEach(button => {
                button.replaceWith(button.cloneNode(true));
                const newButton = shopCharacters.querySelector(`.buy-button[data-rarity="${button.getAttribute('data-rarity')}"][data-character="${button.getAttribute('data-character')}"]`);
                newButton.addEventListener('click', () => {
                    const cost = parseInt(newButton.getAttribute('data-cost'));
                    const rarityName = newButton.getAttribute('data-rarity');
                    const characterName = newButton.getAttribute('data-character');
                    if (goldBalance >= cost) {
                        goldBalance -= cost;
                        updateGoldBalance();
                        const rarity = rarities.find(r => r.name === rarityName);
                        const character = characters[rarityName].find(c => c.name === characterName);
                        addToCards(rarity, character);
                        alert(`Purchased ${rarityName}: ${characterName} for ${cost} Gold!`);
                        if (rarityName === 'Rare') {
                            updateMissionProgress('getRare');
                        }
                        if (rarityName === 'Epic') {
                            updateMissionProgress('getEpic');
                        }
                    } else {
                        alert('Not enough gold to purchase this character.');
                    }
                });
            });

            // Update buy button states
            buyButtons.forEach(button => {
                const cost = parseInt(button.getAttribute('data-cost'));
                button.disabled = goldBalance < cost;
            });
        }

        // Function to render missions and achievements
        function renderMissions() {
            checkDailyReset();
            const achievementsContainer = document.getElementById('achievements');
            const missionsContainer = document.getElementById('daily-missions');
            achievementsContainer.innerHTML = '<h3>Achievements</h3>';
            missionsContainer.innerHTML = '<h3>Daily Missions</h3>';

            // Render achievements
            achievements.forEach(ach => {
                const isComplete = ach.condition();
                const achievementItem = document.createElement('div');
                achievementItem.classList.add('achievement-item');
                if (isComplete && !ach.claimed) {
                    achievementItem.classList.add('completed');
                }
                achievementItem.innerHTML = `
                    <h3>${ach.name}</h3>
                    <p>Reward: ${ach.reward} Gold ${ach.claimed ? '(Claimed)' : isComplete ? '' : '(Incomplete)'}</p>
                    ${isComplete && !ach.claimed ? `<button class="claim-button" data-achievement-id="${ach.id}">Claim</button>` : ''}
                `;
                achievementsContainer.appendChild(achievementItem);
            });

            // Render daily missions
            dailyMissions.forEach(mission => {
                const missionItem = document.createElement('div');
                missionItem.classList.add('mission-item');
                if (mission.progress >= mission.goal && !mission.claimedAt) {
                    missionItem.classList.add('completed');
                }
                let statusText = '';
                if (mission.claimedAt) {
                    const timeLeft = 2 * 60 * 60 * 1000 - (Date.now() - mission.claimedAt);
                    if (timeLeft > 0) {
                        statusText = `(Claimed, New Mission in ${Math.ceil(timeLeft / 1000 / 60)} min)`;
                    } else {
                        replaceMission(mission.id);
                        return;
                    }
                } else if (mission.progress >= mission.goal) {
                    statusText = '(Complete)';
                }
                missionItem.innerHTML = `
                    <h3>${mission.name}</h3>
                    <p>Progress: ${mission.progress}/${mission.goal} | Reward: ${mission.reward} Gold ${statusText}</p>
                    ${mission.progress >= mission.goal && !mission.claimedAt ? `<button class="claim-button" data-mission-id="${mission.id}">Claim</button>` : ''}
                `;
                missionsContainer.appendChild(missionItem);
            });

            // Add event listeners to claim buttons
            const claimButtons = document.querySelectorAll('.claim-button');
            claimButtons.forEach(button => {
                button.replaceWith(button.cloneNode(true));
                const newButton = document.querySelector(`.claim-button[data-achievement-id="${button.getAttribute('data-achievement-id') || ''}"][data-mission-id="${button.getAttribute('data-mission-id') || ''}"]`);
                if (newButton) {
                    newButton.addEventListener('click', () => {
                        const achievementId = newButton.getAttribute('data-achievement-id');
                        const missionId = newButton.getAttribute('data-mission-id');
                        if (achievementId) {
                            const ach = achievements.find(a => a.id === achievementId);
                            if (ach && ach.condition() && !ach.claimed) {
                                ach.claimed = true;
                                goldBalance += ach.reward;
                                alert(`Claimed ${ach.name} for ${ach.reward} Gold!`);
                                updateGoldBalance();
                                renderMissions();
                                saveGameState();
                            }
                        } else if (missionId) {
                            const mission = dailyMissions.find(m => m.id === missionId);
                            if (mission && mission.progress >= mission.goal && !mission.claimedAt) {
                                goldBalance += mission.reward;
                                mission.claimedAt = Date.now();
                                alert(`Claimed ${mission.name} for ${mission.reward} Gold! New mission in 2 hours.`);
                                updateGoldBalance();
                                renderMissions();
                                saveGameState();
                                setTimeout(() => {
                                    replaceMission(missionId);
                                    renderMissions();
                                }, 2 * 60 * 60 * 1000);
                            }
                        }
                    });
                }
            });
        }

        // Toggle slide-down menu and reset notification
        const yourCardsHeader = document.querySelector('#your-cards h2');
        const cardsContainer = document.getElementById('cards-container');
        const arrow = yourCardsHeader.querySelector('.arrow');
        yourCardsHeader.addEventListener('click', () => {
            const isOpen = cardsContainer.classList.toggle('open');
            arrow.classList.toggle('open', isOpen);
            if (isOpen && unviewedCount > 0) {
                unviewedCount = 0;
                updateNotificationDot();
            }
        });

        // Event listeners for filter buttons
        const filterButtons = document.querySelectorAll('.filter-button');
        filterButtons.forEach(button => {
            button.addEventListener('click', () => {
                currentFilter = button.getAttribute('data-rarity');
                filterButtons.forEach(btn => btn.classList.remove('active'));
                button.classList.add('active');
                renderCards();
                saveGameState();
            });
        });

        // Event listener for shop button
        document.getElementById('top-shop-button').addEventListener('click', () => {
            const shopMenu = document.getElementById('shop-menu');
            shopMenu.classList.add('open');
            renderShopCharacters();
            updateMissionProgress('openShop');
        });

        // Event listener for shop back button
        document.getElementById('shop-back-button').addEventListener('click', () => {
            const shopMenu = document.getElementById('shop-menu');
            shopMenu.classList.remove('open');
        });

        // Event listener for missions button
        document.getElementById('top-missions-button').addEventListener('click', () => {
            const missionsMenu = document.getElementById('missions-menu');
            missionsMenu.classList.add('open');
            renderMissions();
        });

        // Event listener for missions back button
        document.getElementById('missions-back-button').addEventListener('click', () => {
            const missionsMenu = document.getElementById('missions-menu');
            missionsMenu.classList.remove('open');
        });

        // Initialize game state
        if (dailyMissions.length === 0) {
            generateDailyMissions();
        } else {
            checkDailyReset();
        }
        dailyMissions.forEach(mission => {
            if (mission.claimedAt && Date.now() - mission.claimedAt >= 2 * 60 * 60 * 1000) {
                replaceMission(mission.id);
            } else if (mission.claimedAt) {
                setTimeout(() => {
                    replaceMission(mission.id);
                    renderMissions();
                }, 2 * 60 * 60 * 1000 - (Date.now() - mission.claimedAt));
            }
        });
        updateGoldBalance();
        updateNotificationDot();
        renderCards();
        renderMissions();
    </script>
</body>
</html>
