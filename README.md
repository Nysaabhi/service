<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Deep Chatbot</title>
    <link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0-beta3/css/all.min.css" rel="stylesheet">
    <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@400;500;600;700&display=swap" rel="stylesheet">
    <style>
        :root {
            --primary-color: #FFD700;
            --secondary-color: #FDB931;
            --background-dark: #1a1a1f;
            --text-light: #ffffff;
            --text-dark: #000000;
            --accent-gradient: linear-gradient(135deg, var(--primary-color), var(--secondary-color));
            --error-color: #ff4444;
            --success-color: #00C851;
        }
        
        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
        }
        
        body {
            font-family: 'Poppins', sans-serif;
            background-color: var(--background-dark);
            color: var(--text-light);
            line-height: 1.6;
            min-height: 100vh;
            overflow-x: hidden;
        }
        
        .header {
            padding: 8px 16px;
            background: rgba(26, 26, 31, 0.95);
            position: fixed;
            width: 100%;
            top: 0;
            left: 0;
            z-index: 30;
            border-bottom: 2px solid var(--primary-color);
            backdrop-filter: blur(10px);
            height: 60px;
        }
        
        .header-content {
            max-width: 1400px;
            margin: 0 auto;
            display: flex;
            justify-content: space-between;
            align-items: center;
            height: 100%;
        }
        
        .logo {
            font-size: 1.8em;
            font-weight: 900;
            background: var(--accent-gradient);
            -webkit-background-clip: text;
            color: transparent;
            letter-spacing: 1.2px;
        }
        
        .wallet {
            padding: 10px 20px;
            background: var(--accent-gradient);
            border: none;
            border-radius: 50px;
            color: var(--text-dark);
            font-weight: 600;
            cursor: pointer;
            transition: all 0.3s ease;
            font-size: 0.95em;
            display: flex;
            align-items: center;
            gap: 8px;
        }
        
        .chatbot-container {
            position: fixed;
            top: 60px;
            left: 0;
            right: 0;
            bottom: 60px;
            background: rgba(26, 26, 31, 0.95);
            display: flex;
            flex-direction: column;
        }
        
        .chat-header {
            padding: 15px;
            background: linear-gradient(135deg, rgba(255, 215, 0, 0.1), rgba(253, 185, 49, 0.1));
            border-bottom: 1px solid rgba(255, 215, 0, 0.2);
            display: flex;
            justify-content: space-between;
            align-items: center;
        }
        
        .chat-status {
            display: flex;
            align-items: center;
            color: var(--primary-color);
            font-weight: 600;
        }
        
        .status-dot {
            width: 8px;
            height: 8px;
            background: var(--primary-color);
            border-radius: 50%;
            margin-right: 8px;
            animation: pulse 2s infinite;
        }

        .filters-container {
            padding: 10px 15px;
            background: rgba(255, 255, 255, 0.05);
            display: flex;
            gap: 10px;
            overflow-x: auto;
            white-space: nowrap;
            -webkit-overflow-scrolling: touch;
        }

        .filter-chip {
            padding: 5px 12px;
            background: rgba(255, 215, 0, 0.1);
            border: 1px solid rgba(255, 215, 0, 0.2);
            border-radius: 20px;
            color: var(--primary-color);
            font-size: 0.9em;
            cursor: pointer;
            display: flex;
            align-items: center;
            gap: 5px;
        }

        .filter-chip.active {
            background: var(--accent-gradient);
            color: var(--text-dark);
        }
        
        .chat-messages {
            flex: 1;
            overflow-y: auto;
            padding: 20px;
            scroll-behavior: smooth;
        }
        
        .message {
            display: flex;
            gap: 12px;
            max-width: 85%;
            margin-bottom: 16px;
            animation: messageSlide 0.3s ease-out;
        }
        
        .bot-message {
            align-self: flex-start;
        }
        
        .user-message {
            align-self: flex-end;
            flex-direction: row-reverse;
        }
        
        .message-avatar {
            width: 36px;
            height: 36px;
            background: rgba(255, 215, 0, 0.1);
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            color: var(--primary-color);
        }
        
        .message-content {
            background: rgba(255, 255, 255, 0.05);
            padding: 12px 16px;
            border-radius: 16px;
            color: var(--text-light);
        }
        
        .user-message .message-content {
            background: rgba(255, 215, 0, 0.1);
        }

        .service-card {
            background: rgba(255, 255, 255, 0.05);
            border-radius: 12px;
            padding: 15px;
            margin-top: 10px;
            border: 1px solid rgba(255, 215, 0, 0.2);
            position: relative;
            transition: transform 0.2s ease;
        }

        .service-card:hover {
            transform: translateY(-2px);
        }

        .service-info-button {
            position: absolute;
            top: 15px;
            right: 15px;
            background: rgba(255, 215, 0, 0.1);
            border: none;
            border-radius: 50%;
            width: 32px;
            height: 32px;
            display: flex;
            align-items: center;
            justify-content: center;
            color: var(--primary-color);
            cursor: pointer;
            transition: all 0.3s ease;
        }

        .service-info-button:hover {
            background: rgba(255, 215, 0, 0.2);
        }

        .verified-badge {
            display: inline-flex;
            align-items: center;
            gap: 4px;
            padding: 4px 8px;
            background: rgba(0, 200, 81, 0.1);
            color: var(--success-color);
            border-radius: 4px;
            font-size: 0.8em;
            margin-left: 8px;
        }

        .service-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 10px;
        }

        .service-rating {
            display: flex;
            align-items: center;
            color: var(--primary-color);
            gap: 4px;
        }

        .service-details {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 10px;
            margin-top: 10px;
        }

        .service-detail-item {
            display: flex;
            align-items: center;
            gap: 8px;
            font-size: 0.9em;
        }

        .service-actions {
            display: flex;
            gap: 10px;
            margin-top: 15px;
        }

        .service-button {
            padding: 8px 15px;
            border-radius: 8px;
            border: none;
            cursor: pointer;
            font-weight: 500;
            transition: all 0.3s ease;
            display: flex;
            align-items: center;
            gap: 6px;
        }

        .book-button {
            background: var(--accent-gradient);
            color: var(--text-dark);
        }

        .contact-button {
            background: rgba(255, 215, 0, 0.1);
            color: var(--primary-color);
            border: 1px solid var(--primary-color);
        }

        .modal {
            position: fixed;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            background: var(--background-dark);
            border: 1px solid rgba(255, 215, 0, 0.2);
            border-radius: 16px;
            padding: 20px;
            width: 90%;
            max-width: 500px;
            max-height: 90vh;
            overflow-y: auto;
            z-index: 1000;
            display: none;
        }

        .modal.active {
            display: block;
            animation: modalFade 0.3s ease-out;
        }

        .modal-overlay {
            position: fixed;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background: rgba(0, 0, 0, 0.7);
            z-index: 999;
            display: none;
        }

        .modal-overlay.active {
            display: block;
        }

        .modal-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 15px;
            padding-bottom: 10px;
            border-bottom: 1px solid rgba(255, 215, 0, 0.2);
        }

        .modal-close {
            background: none;
            border: none;
            color: var(--text-light);
            cursor: pointer;
            font-size: 1.5em;
        }

        .provider-stats {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 15px;
            margin: 15px 0;
        }

        .stat-item {
            background: rgba(255, 255, 255, 0.05);
            padding: 10px;
            border-radius: 8px;
            text-align: center;
        }

        .stat-value {
            font-size: 1.2em;
            color: var(--primary-color);
            font-weight: 600;
        }

        .stat-label {
            font-size: 0.9em;
            opacity: 0.8;
        }
        
        .chat-input-container {
            padding: 16px;
            background: rgba(26, 26, 31, 0.98);
            border-top: 1px solid rgba(255, 215, 0, 0.1);
            display: flex;
            gap: 12px;
            align-items: center;
        }
        
        #chatInput {
            flex: 1;
            background: rgba(255, 255, 255, 0.05);
            border: 1px solid rgba(255, 215, 0, 0.2);
            border-radius: 12px;
            padding: 12px 16px;
            color: var(--text-light);
            font-size: 0.95em;
        }
        
        .input-actions {
            display: flex;
            gap: 8px;
        }
        
        .input-actions button {
            background: none;
            border: none;
            color: rgba(255, 215, 0, 0.7);
            cursor: pointer;
            padding: 8px;
            border-radius: 50%;
            transition: all 0.3s ease;
            display: flex;
            align-items: center;
            justify-content: center;
        }
        
        .input-actions button:hover {
            background: rgba(255, 215, 0, 0.1);
        }
        
        .category-popup {
            position: absolute;
            bottom: 80px;
            left: 16px;
            right: 16px;
            background: rgba(26, 26, 31, 0.98);
            border: 1px solid rgba(255, 215, 0, 0.2);
            border-radius: 12px;
            padding: 16px;
            display: none;
            max-height: 70vh;
            overflow-y: auto;
        }

        .category-popup.active {
            display: block;
            animation: slideUp 0.3s ease-out;
        }

        .popup-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 15px;
        }

        .popup-close {
            background: none;
            border: none;
            color: var(--text-light);
            cursor: pointer;
            font-size: 1.2em;
        }

        .category-search {
            padding: 10px;
            background: rgba(255, 255, 255, 0.05);
            border: 1px solid rgba(255, 215, 0, 0.2);
            border-radius: 8px;
            color: var(--text-light);
            width: 100%;
            margin-bottom: 15px;
        }

        .category-grid {
            display: grid;
            grid-template-columns: repeat(3, 1fr);
            gap: 12px;
        }

        .category-item {
            background: rgba(255, 215, 0, 0.1);
            border: 1px solid rgba(255, 215, 0, 0.2);
            border-radius: 8px;
            padding: 15px 10px;
            text-align: center;
            cursor: pointer;
            transition: all 0.3s ease;
            display: flex;
            flex-direction: column;
            align-items: center;
            gap: 8px;
        }

        .category-item:hover {
            background: rgba(255, 215, 0, 0.2);
            transform: translateY(-2px);
        }

        .category-item i {
            font-size: 1.5em;
            color: var(--primary-color);
        }

        .category-item.selected {
            background: var(--accent-gradient);
            color: var(--text-dark);
        }

        .bottom-nav {
            position: fixed;
            bottom: 0;
            left: 0;
            right: 0;
            height: 60px;
            background: rgba(26, 26, 31, 0.98);
            padding: 8px 0;
            border-top: 1px solid rgba(255, 215, 0, 0.2);
        }
        
        .nav-container {
            display: flex;
            justify-content: space-around;
            align-items: center;
            height: 100%;
            max-width: 600px;
            margin: 0 auto;
        }
        
        .nav-item {
            display: flex;
            flex-direction: column;
            align-items: center;
            text-decoration: none;
            color: var(--text-light);
            transition: all 0.3s ease;
            padding: 5px;
            cursor: pointer;
            position: relative;
        }
        
        .nav-item i {
            font-size: 20px;
            margin-bottom: 4px;
        }
        
        .nav-item span {
            font-size: 12px;
        }
        
        .nav-item.active {
            color: var(--primary-color);
        }

        .nav-item.active::after {
            content: '';
            position: absolute;
            bottom: -8px;
            left: 50%;
            transform: translateX(-50%);
            width: 4px;
            height: 4px;
            background: var(--primary-color);
            border-radius: 50%;
        }

        /* Filter Panel Styles */
        .filter-panel {
            position: fixed;
            right: -300px;
            top: 60px;
            bottom: 60px;
            width: 300px;
            background: var(--background-dark);
            border-left: 1px solid rgba(255, 215, 0, 0.2);
            padding: 20px;
            transition: right 0.3s ease;
            z-index: 100;
        }

        .filter-panel.active {
            right: 0;
        }

        .filter-section {
            margin-bottom: 20px;
        }

        .filter-section h3 {
            margin-bottom: 10px;
            color: var(--primary-color);
        }

        .range-slider {
            width: 100%;
            margin: 10px 0;
        }

        .checkbox-group {
            display: flex;
            flex-direction: column;
            gap: 8px;
        }

        .checkbox-item {
            display: flex;
            align-items: center;
            gap: 8px;
        }

        /* Animations */
        @keyframes pulse {
            0% { opacity: 1; }
            50% { opacity: 0.5; }
            100% { opacity: 1; }
        }
        
        @keyframes messageSlide {
            from {
                opacity: 0;
                transform: translateY(10px);
            }
            to {
                opacity: 1;
                transform: translateY(0);
            }
        }

        @keyframes slideUp {
            from {
                opacity: 0;
                transform: translateY(20px);
            }
            to {
                opacity: 1;
                transform: translateY(0);
            }
        }

        @keyframes modalFade {
            from {
                opacity: 0;
                transform: translate(-50%, -45%);
            }
            to {
                opacity: 1;
                transform: translate(-50%, -50%);
            }
        }
        
        /* Responsive Styles */
        @media (max-width: 768px) {
            .header {
                padding: 8px 12px;
            }
            
            .logo {
                font-size: 1.5em;
            }
            
            .chat-messages {
                padding: 15px;
            }
            
            .message {
                max-width: 90%;
            }
            
            .category-grid {
                grid-template-columns: repeat(2, 1fr);
            }
            
            .service-details {
                grid-template-columns: 1fr;
            }
            
            .filter-panel {
                width: 100%;
                right: -100%;
            }
        }

        /* Utility Classes */
        .hidden {
            display: none;
        }

        .flex {
            display: flex;
        }

        .items-center {
            align-items: center;
        }

        .justify-between {
            justify-content: space-between;
        }

        .gap-2 {
            gap: 8px;
        }

                body > h1:first-of-type:not(.heading) {
          display: none !important;
        }
        
        /* Alternative method if the above doesn't work */
        .markdown-body h1:first-child {
          display: none !important;
        }
        
        /* If the number appears in a different container */
        .position-relative h1:first-child {
          display: none !important;
        }

    </style>
</head>
<body>
    <header class="header">
        <div class="header-content">
            <div class="logo">Deep</div>
            <button class="wallet">
                <i class="fas fa-wallet"></i>
                <span> Wallet</span>
            </button>
        </div>
    </header>

    <div class="chatbot-container">
        <div class="chat-header">
            <div class="chat-status">
                <span class="status-dot"></span>
                Deep AI Assistant
            </div>
            <div class="chat-actions">
            </div>
        </div>

        <div class="filters-container">
            <div class="filter-chip active">
                <i class="fas fa-star"></i>
                <span>Top Rated</span>
            </div>
            <div class="filter-chip">
                <i class="fas fa-clock"></i>
                <span>Available Now</span>
            </div>
            <div class="filter-chip">
                <i class="fas fa-money-bill"></i>
                <span>Best Price</span>
            </div>
            <div class="filter-chip">
                <i class="fas fa-award"></i>
                <span>Verified</span>
            </div>
        </div>
        
        <div class="chat-messages" id="chatMessages">
    </div>


        <div class="category-popup" id="categoryPopup">
            <div class="popup-header">
                <h3>Select Service Category</h3>
                <button class="popup-close">
                    <i class="fas fa-times"></i>
                </button>
            </div>
            <input type="text" class="category-search" placeholder="Search categories...">
            <div class="category-grid">
                <div class="category-item" data-category="plumber">
                    <i class="fas fa-wrench"></i>
                    <p>Plumber</p>
                </div>
                <div class="category-item" data-category="electrician">
                    <i class="fas fa-bolt"></i>
                    <p>Electrician</p>
                </div>
                <div class="category-item" data-category="carpenter">
                    <i class="fas fa-hammer"></i>
                    <p>Carpenter</p>
                </div>
                <div class="category-item" data-category="painter">
                    <i class="fas fa-paint-roller"></i>
                    <p>Painter</p>
                </div>
                <div class="category-item" data-category="cleaner">
                    <i class="fas fa-broom"></i>
                    <p>Cleaner</p>
                </div>
                <div class="category-item" data-category="designer">
                    <i class="fas fa-pencil-ruler"></i>
                    <p>Designer</p>
                </div>
                <div class="category-item" data-category="beautician">
                    <i class="fas fa-spa"></i>
                    <p>Beautician</p>
                </div>
                <div class="category-item" data-category="photographer">
                    <i class="fas fa-camera"></i>
                    <p>Photographer</p>
                </div>
                <div class="category-item" data-category="tutor">
                    <i class="fas fa-book"></i>
                    <p>Tutor</p>
                </div>
            </div>
        </div>
        
        <div class="chat-input-container">
            <input type="text" id="chatInput" placeholder="Search for services or ask a question..." />
            <div class="input-actions">
                <button class="select-input" id="categoryButton">
                    <i class="fas fa-th-large"></i>
                </button>
                <button class="send-message" id="sendButton">
                    <i class="fas fa-paper-plane"></i>
                </button>
            </div>
        </div>
    </div>

    <div class="filter-panel" id="filterPanel">
        <div class="filter-section">
            <h3>Price Range</h3>
            <input type="range" class="range-slider" min="0" max="5000" step="100">
            <div class="flex justify-between">
                <span>₹0</span>
                <span>₹5000</span>
            </div>
        </div>

        <div class="filter-section">
            <h3>Rating</h3>
            <div class="checkbox-group">
                <label class="checkbox-item">
                    <input type="checkbox" checked> 4.5+ Stars
                </label>
                <label class="checkbox-item">
                    <input type="checkbox"> 4.0+ Stars
                </label>
                <label class="checkbox-item">
                    <input type="checkbox"> 3.5+ Stars
                </label>
            </div>
        </div>

        <div class="filter-section">
            <h3>Availability</h3>
            <div class="checkbox-group">
                <label class="checkbox-item">
                    <input type="checkbox"> Available Now
                </label>
                <label class="checkbox-item">
                    <input type="checkbox"> Available Today
                </label>
                <label class="checkbox-item">
                    <input type="checkbox"> Available This Week
                </label>
            </div>
        </div>
    </div>

    <nav class="bottom-nav">
        <div class="nav-container">
            <div class="nav-item active" data-page="subscription">
                <i class="fas fa-user-check"></i>
                <span>Subscription</span>
            </div>
            <div class="nav-item" data-page="deals">
                <i class="fas fa-ticket-alt"></i>
                <span>Deals</span>
            </div>
            <div class="nav-item" data-page="filter">
                <i class="fas fa-filter"></i>
                <span>Filter</span>
            </div>
            <div class="nav-item" data-page="bookings">
                <i class="fas fa-calendar-alt"></i>
                <span>Bookings</span>
            </div>
            <div class="nav-item" data-page="location">
                <i class="fas fa-location"></i>
                <span>Location</span>
            </div>
        </div>
    </nav>

    <div class="modal-overlay" id="modalOverlay"></div>
    
    <script>
    // Service providers database simulation
const serviceProviders = [
    {
        id: 'sp1',
        name: 'John Smith',
        category: 'plumber',
        subCategories: ['pipe repair', 'bathroom', 'kitchen', 'water heater'],
        rating: 4.8,
        totalReviews: 127,
        hourlyRate: 499,
        experience: 8,
        isVerified: true,
        isAvailable: true,
        location: 'Delhi',
        distance: '2.5km',
        responseTime: '5 mins',
        completedJobs: 342,
        languages: ['English', 'Spanish'],
        skills: ['Emergency Repairs', 'Installation', 'Maintenance']
    },
    // Add more service providers with different categories and attributes
];

// Keywords and synonyms database for smart search
const searchKeywords = {
    'plumber': ['pipe', 'leak', 'drain', 'tap', 'faucet', 'toilet', 'bathroom', 'kitchen', 'water'],
    'electrician': ['wire', 'switch', 'light', 'power', 'circuit', 'electrical', 'outlet', 'fan'],
    'carpenter': ['wood', 'furniture', 'door', 'cabinet', 'repair', 'table', 'chair'],
    // Add more categories and related keywords
};

// Initialize chat functionality
document.addEventListener('DOMContentLoaded', () => {
    const chatMessages = document.getElementById('chatMessages');
    const chatInput = document.getElementById('chatInput');
    const sendButton = document.getElementById('sendButton');
    const categoryButton = document.getElementById('categoryButton');
    const categoryPopup = document.getElementById('categoryPopup');
    const filterPanel = document.getElementById('filterPanel');
    const modalOverlay = document.getElementById('modalOverlay');
    
    // Chat state management
    let chatContext = {
        currentCategory: null,
        priceRange: { min: 0, max: 5000 },
        selectedFilters: {
            rating: 4.5,
            availability: 'all',
            verified: false
        },
        userLocation: null,
        lastSearchResults: [],
        conversationHistory: []
    };

    // Advanced search algorithm
    function searchServices(query) {
        query = query.toLowerCase();
        const searchTerms = query.split(' ');
        let matches = new Map();

        // Score each provider based on search relevance
        serviceProviders.forEach(provider => {
            let score = 0;
            let matchedKeywords = [];

            // Check direct category matches
            if (provider.category.toLowerCase().includes(query)) {
                score += 10;
                matchedKeywords.push(provider.category);
            }

            // Check keyword matches
            searchTerms.forEach(term => {
                // Check category keywords
                Object.entries(searchKeywords).forEach(([category, keywords]) => {
                    if (keywords.some(keyword => term.includes(keyword.toLowerCase()))) {
                        if (provider.category === category) {
                            score += 5;
                            matchedKeywords.push(term);
                        }
                    }
                });

                // Check subcategories
                provider.subCategories.forEach(sub => {
                    if (sub.toLowerCase().includes(term)) {
                        score += 3;
                        matchedKeywords.push(sub);
                    }
                });

                // Check skills
                provider.skills.forEach(skill => {
                    if (skill.toLowerCase().includes(term)) {
                        score += 2;
                        matchedKeywords.push(skill);
                    }
                });
            });

            // Apply filters
            if (provider.rating >= chatContext.selectedFilters.rating) score += 2;
            if (provider.isVerified && chatContext.selectedFilters.verified) score += 2;
            if (provider.isAvailable && chatContext.selectedFilters.availability === 'now') score += 3;

            if (score > 0) {
                matches.set(provider, {
                    score,
                    matchedKeywords: [...new Set(matchedKeywords)]
                });
            }
        });

        // Sort results by score
        return Array.from(matches.entries())
            .sort((a, b) => b[1].score - a[1].score)
            .map(([provider, match]) => ({
                provider,
                relevance: match.score,
                matchedTerms: match.matchedKeywords
            }));
    }

    // Render chat messages with advanced formatting
    function renderMessage(content, isUser = false) {
        const messageDiv = document.createElement('div');
        messageDiv.className = `message ${isUser ? 'user-message' : 'bot-message'}`;

        const avatar = document.createElement('div');
        avatar.className = 'message-avatar';
        avatar.innerHTML = `<i class="fas fa-${isUser ? 'user' : 'robot'}"></i>`;

        const messageContent = document.createElement('div');
        messageContent.className = 'message-content';

        if (typeof content === 'string') {
            messageContent.textContent = content;
        } else if (Array.isArray(content)) {
            // Render service provider cards
            content.forEach(result => {
                const serviceCard = createServiceCard(result.provider, result.matchedTerms);
                messageContent.appendChild(serviceCard);
            });
        }

        messageDiv.appendChild(avatar);
        messageDiv.appendChild(messageContent);
        chatMessages.appendChild(messageDiv);
        chatMessages.scrollTop = chatMessages.scrollHeight;
    }

    // Create service provider card
    function createServiceCard(provider, matchedTerms = []) {
        const card = document.createElement('div');
        card.className = 'service-card';
        
        card.innerHTML = `
            <div class="service-header">
                <h3>${provider.name}
                    ${provider.isVerified ? 
                        '<span class="verified-badge"><i class="fas fa-check-circle"></i> Verified</span>' 
                        : ''}
                </h3>
                <div class="service-rating">
                    <i class="fas fa-star"></i>
                    <span>${provider.rating}</span>
                    <span>(${provider.totalReviews})</span>
                </div>
            </div>
            <div class="service-details">
                <div class="service-detail-item">
                    <i class="fas fa-money-bill"></i>
                    <span>₹${provider.hourlyRate}/hr</span>
                </div>
                <div class="service-detail-item">
                    <i class="fas fa-briefcase"></i>
                    <span>${provider.experience} years</span>
                </div>
                <div class="service-detail-item">
                    <i class="fas fa-map-marker-alt"></i>
                    <span>${provider.distance}</span>
                </div>
                <div class="service-detail-item">
                    <i class="fas fa-clock"></i>
                    <span>${provider.responseTime} response</span>
                </div>
            </div>
            ${matchedTerms.length > 0 ? `
                <div class="matched-terms">
                    ${matchedTerms.map(term => 
                        `<span class="tag">${term}</span>`
                    ).join('')}
                </div>
            ` : ''}
            <div class="service-actions">
                <button class="service-button book-button">
                    <i class="fas fa-calendar-check"></i>
                    Book Now
                </button>
                <button class="service-button contact-button">
                    <i class="fas fa-comment"></i>
                    Contact
                </button>
            </div>
        `;

        // Add event listeners for buttons
        const bookButton = card.querySelector('.book-button');
        const contactButton = card.querySelector('.contact-button');
        
        bookButton.addEventListener('click', () => handleBooking(provider));
        contactButton.addEventListener('click', () => handleContact(provider));

        return card;
    }

    // Handle booking process
    function handleBooking(provider) {
        showModal({
            title: `Book ${provider.name}`,
            content: `
                <div class="booking-form">
                    <div class="datetime-picker">
                        <input type="date" min="${new Date().toISOString().split('T')[0]}">
                        <select>
                            ${generateTimeSlots()}
                        </select>
                    </div>
                    <textarea placeholder="Describe your requirement..."></textarea>
                    <button class="book-button">Confirm Booking</button>
                </div>
            `
        });
    }

    // Generate time slots
    function generateTimeSlots() {
        const slots = [];
        for (let i = 9; i <= 20; i++) {
            slots.push(`<option value="${i}:00">${i}:00</option>`);
            slots.push(`<option value="${i}:30">${i}:30</option>`);
        }
        return slots.join('');
    }

    // Handle contact initiation
    function handleContact(provider) {
        showModal({
            title: `Contact ${provider.name}`,
            content: `
                <div class="contact-options">
                    <button class="chat-option">
                        <i class="fas fa-comment"></i>
                        Start Chat
                    </button>
                    <button class="call-option">
                        <i class="fas fa-phone"></i>
                        Voice Call
                    </button>
                </div>
            `
        });
    }

    // Show modal
    function showModal({ title, content }) {
        const modal = document.createElement('div');
        modal.className = 'modal';
        modal.innerHTML = `
            <div class="modal-header">
                <h3>${title}</h3>
                <button class="modal-close">&times;</button>
            </div>
            <div class="modal-content">
                ${content}
            </div>
        `;

        modalOverlay.classList.add('active');
        document.body.appendChild(modal);
        setTimeout(() => modal.classList.add('active'), 10);

        modal.querySelector('.modal-close').addEventListener('click', () => {
            modal.remove();
            modalOverlay.classList.remove('active');
        });
    }

    // Process user input
    function processUserInput(input) {
        renderMessage(input, true);
        chatContext.conversationHistory.push({ role: 'user', content: input });

        // Perform search
        const searchResults = searchServices(input);
        chatContext.lastSearchResults = searchResults;

        if (searchResults.length > 0) {
            renderMessage(searchResults);
            renderMessage(`I found ${searchResults.length} service providers matching your requirements. Would you like to apply any filters or see more details?`);
        } else {
            renderMessage("I couldn't find any exact matches. Would you like to broaden your search or try a different category?");
        }
    }

    // Event listeners
    sendButton.addEventListener('click', () => {
        const input = chatInput.value.trim();
        if (input) {
            processUserInput(input);
            chatInput.value = '';
        }
    });

    chatInput.addEventListener('keypress', (e) => {
        if (e.key === 'Enter' && !e.shiftKey) {
            e.preventDefault();
            sendButton.click();
        }
    });

    // Smart input suggestions
    chatInput.addEventListener('input', (e) => {
        const input = e.target.value.toLowerCase();
        if (input.length >= 2) {
            const suggestions = generateSuggestions(input);
            showSuggestions(suggestions);
        }
    });

    // Generate input suggestions
    function generateSuggestions(input) {
        const suggestions = new Set();
        
        Object.entries(searchKeywords).forEach(([category, keywords]) => {
            if (category.toLowerCase().includes(input)) {
                suggestions.add(category);
            }
            keywords.forEach(keyword => {
                if (keyword.toLowerCase().includes(input)) {
                    suggestions.add(keyword);
                }
            });
        });

        return Array.from(suggestions).slice(0, 5);
    }

    // Show suggestions dropdown
    function showSuggestions(suggestions) {
        let existing = document.querySelector('.suggestions-dropdown');
        if (existing) existing.remove();

        if (suggestions.length === 0) return;

        const dropdown = document.createElement('div');
        dropdown.className = 'suggestions-dropdown';
        
        suggestions.forEach(suggestion => {
            const item = document.createElement('div');
            item.className = 'suggestion-item';
            item.textContent = suggestion;
            item.addEventListener('click', () => {
                chatInput.value = suggestion;
                dropdown.remove();
            });
            dropdown.appendChild(item);
        });

        chatInput.parentElement.appendChild(dropdown);
    }

    // Initialize chat
    renderMessage("Hello! I'm your On-Demand services assistant. How can I help you today? You can search for specific services or browse our categories.");
});
    </script>
