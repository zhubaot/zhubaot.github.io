<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>智能气象分析系统 - 全国雷达图优化版</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <style>
        :root {
            --primary: #1a73e8;
            --secondary: #0d47a1;
            --accent: #00bcd4;
            --light: #f5f9ff;
            --dark: #0a1929;
            --ai-color: #9c27b0;
            --nmc-color: #f57c00;
            --success: #4caf50;
            --warning: #ff9800;
            --danger: #f44336;
        }
        
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Segoe UI', 'Microsoft YaHei', sans-serif;
        }
        
        body {
            background: linear-gradient(135deg, var(--secondary), var(--dark));
            color: var(--light);
            min-height: 100vh;
            padding: 20px;
            background-attachment: fixed;
        }
        
        .container {
            max-width: 1400px;
            margin: 0 auto;
        }
        
        header {
            text-align: center;
            padding: 30px 0;
            margin-bottom: 20px;
            animation: fadeIn 1s ease;
        }
        
        h1 {
            font-size: 2.8rem;
            margin-bottom: 15px;
            text-shadow: 0 2px 10px rgba(0,0,0,0.3);
            background: linear-gradient(to right, var(--accent), var(--ai-color));
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
        }
        
        .subtitle {
            font-size: 1.2rem;
            opacity: 0.9;
            max-width: 800px;
            margin: 0 auto;
            line-height: 1.6;
            color: #e0f7fa;
        }
        
        .card {
            background: rgba(255, 255, 255, 0.08);
            backdrop-filter: blur(12px);
            border-radius: 16px;
            padding: 25px;
            margin-bottom: 25px;
            box-shadow: 0 8px 32px rgba(0, 0, 0, 0.2);
            border: 1px solid rgba(255, 255, 255, 0.15);
            transition: transform 0.3s ease, box-shadow 0.3s ease;
            animation: slideUp 0.8s ease;
        }
        
        .card:hover {
            transform: translateY(-5px);
            box-shadow: 0 12px 40px rgba(0, 0, 0, 0.3);
        }
        
        .card-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 20px;
            padding-bottom: 15px;
            border-bottom: 1px solid rgba(255, 255, 255, 0.2);
        }
        
        .card-title {
            font-size: 1.5rem;
            display: flex;
            align-items: center;
            gap: 10px;
            color: var(--accent);
        }
        
        .ai-title {
            color: var(--ai-color);
        }
        
        .nmc-title {
            color: var(--nmc-color);
        }
        
        .location-info {
            display: flex;
            align-items: center;
            gap: 10px;
            background: rgba(0, 0, 0, 0.2);
            padding: 8px 15px;
            border-radius: 50px;
            font-size: 0.9rem;
        }
        
        .current-weather {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 30px;
        }
        
        @media (max-width: 768px) {
            .current-weather {
                grid-template-columns: 1fr;
            }
        }
        
        .weather-main {
            text-align: center;
            padding: 20px;
            background: rgba(0, 0, 0, 0.15);
            border-radius: 15px;
        }
        
        .temperature {
            font-size: 5rem;
            font-weight: 700;
            margin: 20px 0;
            position: relative;
            color: #fff;
            text-shadow: 0 0 10px rgba(0, 188, 212, 0.5);
        }
        
        .weather-icon {
            font-size: 5rem;
            margin-bottom: 20px;
            filter: drop-shadow(0 5px 10px rgba(0,0,0,0.3));
            color: #ffeb3b;
        }
        
        .weather-description {
            font-size: 1.4rem;
            margin-bottom: 15px;
            text-transform: capitalize;
        }
        
        .weather-details {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 15px;
        }
        
        .detail-item {
            display: flex;
            align-items: center;
            gap: 10px;
            background: rgba(255, 255, 255, 0.05);
            padding: 15px;
            border-radius: 15px;
            transition: background 0.3s ease;
        }
        
        .detail-item:hover {
            background: rgba(255, 255, 255, 0.1);
        }
        
        .detail-icon {
            font-size: 1.8rem;
            width: 50px;
            text-align: center;
            color: var(--accent);
        }
        
        .detail-text {
            flex: 1;
        }
        
        .detail-label {
            font-size: 0.9rem;
            opacity: 0.8;
            margin-bottom: 5px;
        }
        
        .detail-value {
            font-size: 1.3rem;
            font-weight: 600;
        }
        
        .hourly-forecast {
            overflow-x: auto;
            padding-bottom: 15px;
        }
        
        .hourly-container {
            display: flex;
            gap: 15px;
            min-width: 1000px;
        }
        
        .hour-item {
            background: rgba(255, 255, 255, 0.05);
            border-radius: 15px;
            padding: 15px;
            text-align: center;
            min-width: 100px;
            transition: all 0.3s ease;
        }
        
        .hour-item:hover {
            transform: translateY(-8px);
            background: rgba(255, 255, 255, 0.1);
            box-shadow: 0 8px 16px rgba(0,0,0,0.2);
        }
        
        .hour-time {
            font-weight: 600;
            margin-bottom: 10px;
            color: var(--accent);
        }
        
        .hour-icon {
            font-size: 2rem;
            margin: 10px 0;
            color: #ffeb3b;
        }
        
        .hour-temp {
            font-size: 1.3rem;
            font-weight: 600;
        }
        
        .chart-container {
            height: 300px;
            margin-top: 20px;
        }
        
        .explanation {
            line-height: 1.8;
            font-size: 1.1rem;
        }
        
        .ai-analysis {
            background: rgba(156, 39, 176, 0.1);
            border-left: 4px solid var(--ai-color);
            padding: 20px;
            border-radius: 8px;
            margin: 20px 0;
            position: relative;
        }
        
        .ai-header {
            display: flex;
            align-items: center;
            gap: 10px;
            margin-bottom: 15px;
        }
        
        .ai-icon {
            color: var(--ai-color);
            font-size: 1.5rem;
        }
        
        .radar-container {
            margin-top: 25px;
            border-radius: 15px;
            overflow: hidden;
            box-shadow: 0 8px 32px rgba(0, 0, 0, 0.3);
            background: rgba(0, 0, 0, 0.2);
        }
        
        .radar-header {
            background: rgba(0, 0, 0, 0.4);
            padding: 15px 20px;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }
        
        .radar-title {
            font-size: 1.4rem;
            font-weight: 600;
            display: flex;
            align-items: center;
            gap: 10px;
            color: var(--nmc-color);
        }
        
        .radar-controls {
            display: flex;
            gap: 10px;
        }
        
        .radar-btn {
            background: rgba(255, 255, 255, 0.1);
            border: none;
            color: white;
            padding: 8px 15px;
            border-radius: 5px;
            cursor: pointer;
            transition: all 0.3s ease;
            display: flex;
            align-items: center;
            gap: 5px;
            font-size: 0.9rem;
        }
        
        .radar-btn:hover {
            background: rgba(255, 255, 255, 0.2);
            transform: translateY(-2px);
        }
        
        .radar-btn.fullscreen {
            background: rgba(245, 124, 0, 0.3);
        }
        
        .radar-btn.fullscreen:hover {
            background: rgba(245, 124, 0, 0.5);
        }
        
        .radar-viewport {
            position: relative;
            width: 100%;
            height: 600px; /* 增加高度 */
            overflow: hidden;
            background: #000;
        }
        
        .radar-iframe {
            width: 100%;
            height: 100%;
            border: none;
            transition: transform 0.3s ease;
        }
        
        .radar-footer {
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 12px 20px;
            background: rgba(0, 0, 0, 0.4);
            font-size: 0.9rem;
            color: #bbdefb;
        }
        
        .radar-info {
            display: flex;
            align-items: center;
            gap: 15px;
        }
        
        .radar-update {
            display: flex;
            align-items: center;
            gap: 5px;
        }
        
        .radar-scale {
            display: flex;
            align-items: center;
            gap: 5px;
        }
        
        .loading {
            text-align: center;
            padding: 50px;
            font-size: 1.2rem;
        }
        
        .spinner {
            border: 4px solid rgba(255, 255, 255, 0.3);
            border-radius: 50%;
            border-top: 4px solid var(--accent);
            width: 50px;
            height: 50px;
            animation: spin 1s linear infinite;
            margin: 20px auto;
        }
        
        .ai-spinner {
            border-top-color: var(--ai-color);
        }
        
        .nmc-spinner {
            border-top-color: var(--nmc-color);
        }
        
        @keyframes spin {
            0% { transform: rotate(0deg); }
            100% { transform: rotate(360deg); }
        }
        
        .api-status {
            display: flex;
            justify-content: center;
            gap: 20px;
            margin-top: 15px;
        }
        
        .status-item {
            display: flex;
            align-items: center;
            gap: 8px;
            padding: 8px 15px;
            border-radius: 50px;
            background: rgba(0, 0, 0, 0.3);
            font-size: 0.9rem;
        }
        
        .ai-status {
            background: rgba(156, 39, 176, 0.2);
        }
        
        .nmc-status {
            background: rgba(245, 124, 0, 0.2);
        }
        
        .status-dot {
            width: 12px;
            height: 12px;
            border-radius: 50%;
            background: #4caf50;
        }
        
        .status-dot.ai {
            background: var(--ai-color);
        }
        
        .status-dot.nmc {
            background: var(--nmc-color);
        }
        
        .status-dot.offline {
            background: #f44336;
        }
        
        footer {
            text-align: center;
            padding: 30px 0 20px;
            margin-top: 20px;
            font-size: 0.9rem;
            opacity: 0.7;
            border-top: 1px solid rgba(255, 255, 255, 0.1);
        }
        
        .update-time {
            font-size: 0.9rem;
            opacity: 0.8;
            text-align: right;
            margin-top: 10px;
            color: #bbdefb;
        }
        
        @keyframes fadeIn {
            from { opacity: 0; }
            to { opacity: 1; }
        }
        
        @keyframes slideUp {
            from { 
                opacity: 0;
                transform: translateY(30px);
            }
            to { 
                opacity: 1;
                transform: translateY(0);
            }
        }
        
        .highlight {
            background: rgba(0, 188, 212, 0.2);
            padding: 2px 6px;
            border-radius: 4px;
            font-weight: bold;
            color: var(--accent);
        }
        
        .error-message {
            background: rgba(244, 67, 54, 0.2);
            border-left: 4px solid var(--danger);
            padding: 15px;
            border-radius: 4px;
            margin: 15px 0;
            display: none;
        }
        
        .source-info {
            background: rgba(0, 0, 0, 0.2);
            padding: 15px;
            border-radius: 10px;
            margin-top: 20px;
            font-size: 0.9rem;
        }
        
        .source-info h4 {
            margin-bottom: 10px;
            color: var(--accent);
        }
        
        .zoom-controls {
            position: absolute;
            bottom: 20px;
            right: 20px;
            display: flex;
            flex-direction: column;
            gap: 10px;
            z-index: 100;
        }
        
        .zoom-btn {
            width: 40px;
            height: 40px;
            border-radius: 50%;
            background: rgba(255, 255, 255, 0.2);
            border: none;
            color: white;
            font-size: 1.2rem;
            cursor: pointer;
            display: flex;
            align-items: center;
            justify-content: center;
            transition: all 0.3s ease;
        }
        
        .zoom-btn:hover {
            background: rgba(255, 255, 255, 0.3);
            transform: scale(1.1);
        }
        
        .fullscreen-overlay {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0, 0, 0, 0.9);
            z-index: 1000;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            padding: 20px;
        }
        
        .fullscreen-header {
            width: 100%;
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 15px 20px;
            background: rgba(0, 0, 0, 0.7);
        }
        
        .fullscreen-title {
            font-size: 1.5rem;
            color: var(--nmc-color);
        }
        
        .close-fullscreen {
            background: transparent;
            border: none;
            color: white;
            font-size: 1.5rem;
            cursor: pointer;
            padding: 10px;
        }
        
        .fullscreen-iframe {
            width: 95%;
            height: 90%;
            border: none;
        }
        
        .radar-legend {
            position: absolute;
            bottom: 20px;
            left: 20px;
            background: rgba(0, 0, 0, 0.6);
            padding: 8px 15px;
            border-radius: 8px;
            color: white;
            font-size: 0.85rem;
            max-width: 300px;
        }
        
        .legend-title {
            font-weight: bold;
            margin-bottom: 5px;
            color: var(--accent);
        }
        
        .legend-items {
            display: flex;
            gap: 10px;
            margin-top: 8px;
        }
        
        .legend-item {
            display: flex;
            align-items: center;
            gap: 5px;
        }
        
        .legend-color {
            width: 20px;
            height: 15px;
            border-radius: 3px;
        }
        
        .radar-loading {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0, 0, 0, 0.7);
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            z-index: 10;
            color: white;
        }
    </style>
</head>
<body>
    <div class="container">
        <header>
            <h1><i class="fas fa-cloud-sun"></i> 智能气象分析系统</h1>
            <p class="subtitle">全国雷达图优化版 - 更清晰、更专业的天气监测</p>
            
            <div class="api-status">
                <div class="status-item">
                    <div class="status-dot" id="ipapi-status"></div>
                    <span>定位服务</span>
                </div>
                <div class="status-item">
                    <div class="status-dot" id="owm-status"></div>
                    <span>气象数据</span>
                </div>
                <div class="status-item ai-status">
                    <div class="status-dot ai" id="ai-status"></div>
                    <span>DeepSeek AI</span>
                </div>
                <div class="status-item nmc-status">
                    <div class="status-dot nmc" id="nmc-status"></div>
                    <span>中央气象台</span>
                </div>
            </div>
        </header>
        
        <main>
            <!-- 天气数据卡片 -->
            <div class="card">
                <div class="card-header">
                    <h2 class="card-title"><i class="fas fa-location-dot"></i> 当前位置信息</h2>
                    <div class="location-info">
                        <i class="fas fa-globe-americas"></i>
                        <span id="location">正在获取您的位置信息...</span>
                    </div>
                </div>
                
                <div class="error-message" id="error-message">
                    <i class="fas fa-exclamation-triangle"></i> 
                    <span id="error-text"></span>
                </div>
                
                <div class="loading" id="loading">
                    <div class="spinner"></div>
                    <p>正在获取实时气象数据...</p>
                </div>
                
                <div id="weather-content" style="display: none;">
                    <div class="current-weather">
                        <div class="weather-main">
                            <div class="weather-icon" id="current-icon">
                                <i class="fas fa-sun"></i>
                            </div>
                            <div class="temperature" id="current-temp">--°C</div>
                            <div class="weather-description" id="weather-desc">--</div>
                            <div class="feels-like">体感温度: <span id="feels-like">--°C</span></div>
                        </div>
                        
                        <div class="weather-details">
                            <div class="detail-item">
                                <div class="detail-icon"><i class="fas fa-tint"></i></div>
                                <div class="detail-text">
                                    <div class="detail-label">湿度</div>
                                    <div class="detail-value" id="humidity">--%</div>
                                </div>
                            </div>
                            
                            <div class="detail-item">
                                <div class="detail-icon"><i class="fas fa-wind"></i></div>
                                <div class="detail-text">
                                    <div class="detail-label">风速</div>
                                    <div class="detail-value" id="wind-speed">-- m/s</div>
                                </div>
                            </div>
                            
                            <div class="detail-item">
                                <div class="detail-icon"><i class="fas fa-compress-arrows-alt"></i></div>
                                <div class="detail-text">
                                    <div class="detail-label">气压</div>
                                    <div class="detail-value" id="pressure">-- hPa</div>
                                </div>
                            </div>
                            
                            <div class="detail-item">
                                <div class="detail-icon"><i class="fas fa-cloud"></i></div>
                                <div class="detail-text">
                                    <div class="detail-label">云量</div>
                                    <div class="detail-value" id="clouds">--%</div>
                                </div>
                            </div>
                            
                            <div class="detail-item">
                                <div class="detail-icon"><i class="fas fa-sun"></i></div>
                                <div class="detail-text">
                                    <div class="detail-label">日出</div>
                                    <div class="detail-value" id="sunrise">--:--</div>
                                </div>
                            </div>
                            
                            <div class="detail-item">
                                <div class="detail-icon"><i class="fas fa-moon"></i></div>
                                <div class="detail-text">
                                    <div class="detail-label">日落</div>
                                    <div class="detail-value" id="sunset">--:--</div>
                                </div>
                            </div>
                        </div>
                    </div>
                    
                    <div class="update-time">
                        最后更新: <span id="update-time">--:--:--</span> | 数据来源: OpenWeatherMap
                    </div>
                </div>
            </div>
            
            <!-- 天气预报卡片 -->
            <div class="card">
                <div class="card-header">
                    <h2 class="card-title"><i class="fas fa-chart-line"></i> 24小时天气预报</h2>
                    <div class="location-info">
                        <i class="fas fa-clock"></i>
                        <span>未来24小时预测</span>
                    </div>
                </div>
                
                <div class="hourly-forecast">
                    <div class="hourly-container" id="hourly-forecast">
                        <!-- 小时预报将通过JS动态生成 -->
                    </div>
                </div>
                
                <div class="chart-container">
                    <canvas id="weather-chart"></canvas>
                </div>
            </div>
            
            <!-- AI分析卡片 -->
            <div class="card">
                <div class="card-header">
                    <h2 class="card-title ai-title"><i class="fas fa-robot"></i> DeepSeek气象分析</h2>
                    <div class="location-info">
                        <i class="fas fa-microchip"></i>
                        <span>DeepSeek-R1模型</span>
                    </div>
                </div>
                
                <div class="ai-analysis">
                    <div class="ai-header">
                        <i class="fas fa-robot ai-icon"></i>
                        <h3>DeepSeek气象分析引擎</h3>
                    </div>
                    <p>基于当前气象数据和大气科学原理，DeepSeek AI为您提供专业气象分析：</p>
                    
                    <div class="ai-response" id="ai-analysis-content">
                        <div class="ai-loading">
                            <div class="spinner ai-spinner"></div>
                            <p>DeepSeek AI正在分析气象数据并生成专业报告...</p>
                        </div>
                    </div>
                </div>
            </div>
            
            <!-- 中央气象台雷达图卡片 -->
            <div class="card">
                <div class="card-header">
                    <h2 class="card-title nmc-title"><i class="fas fa-radar"></i> 全国雷达拼图</h2>
                    <div class="location-info">
                        <i class="fas fa-satellite"></i>
                        <span>实时降水监测</span>
                    </div>
                </div>
                
                <div class="radar-container">
                    <div class="radar-header">
                        <div class="radar-title">
                            <i class="fas fa-map"></i>
                            <span>中央气象台全国雷达拼图</span>
                        </div>
                        <div class="radar-controls">
                            <button class="radar-btn" id="refresh-radar">
                                <i class="fas fa-sync-alt"></i> 刷新数据
                            </button>
                            <button class="radar-btn fullscreen" id="fullscreen-btn">
                                <i class="fas fa-expand"></i> 全屏查看
                            </button>
                        </div>
                    </div>
                    
                    <div class="radar-viewport">
                        <div class="radar-loading" id="radar-loading">
                            <div class="spinner nmc-spinner"></div>
                            <p>正在加载最新全国雷达图...</p>
                        </div>
                        <iframe id="radar-iframe" class="radar-iframe" src="https://www.nmc.cn/publish/radar/chinaall.html" frameborder="0"></iframe>
                        
                        <div class="zoom-controls">
                            <button class="zoom-btn" id="zoom-in">
                                <i class="fas fa-plus"></i>
                            </button>
                            <button class="zoom-btn" id="zoom-out">
                                <i class="fas fa-minus"></i>
                            </button>
                            <button class="zoom-btn" id="reset-zoom">
                                <i class="fas fa-sync"></i>
                            </button>
                        </div>
                        
                        <div class="radar-legend">
                            <div class="legend-title">雷达图例说明</div>
                            <div>颜色越暖代表降水强度越大</div>
                            <div class="legend-items">
                                <div class="legend-item">
                                    <div class="legend-color" style="background: #04e9e7;"></div>
                                    <span></span>
                                </div>
                                <div class="legend-item">
                                    <div class="legend-color" style="background: #019ff4;"></div>
                                    <span></span>
                                </div>
                                <div class="legend-item">
                                    <div class="legend-color" style="background: #0300f4;"></div>
                                    <span></span>
                                </div>
                                <div class="legend-item">
                                    <div class="legend-color" style="background: #02fd02;"></div>
                                    <span></span>
                                </div>
                                <div class="legend-item">
                                    <div class="legend-color" style="background: #00c500;"></div>
                                    <span></span>
                                </div>
                            </div>
                        </div>
                    </div>
                    
                    <div class="radar-footer">
                        <div class="radar-info">
                            <div class="radar-update">
                                <i class="fas fa-clock"></i>
                                <span>最后更新: <span id="radar-update-time">--:--:--</span></span>
                            </div>
                            <div class="radar-scale">
                                <i class="fas fa-ruler-combined"></i>
                                <span>比例尺: <span id="radar-scale">100%</span></span>
                            </div>
                        </div>
                        <div class="radar-source">
                            数据来源: 中国气象局国家气象中心 (NMC)
                        </div>
                    </div>
                </div>
                
                <div class="source-info">
                    <h4><i class="fas fa-info-circle"></i> 雷达图使用说明</h4>
                    <p>本雷达图显示全国范围内的降水分布情况，颜色从蓝色到红色表示降水强度逐渐增强。</p>
                    <p>雷达图每6分钟自动更新一次，您也可以手动刷新获取最新数据。</p>
                    <p>使用右上角的缩放按钮可以调整地图显示比例，全屏按钮可进入沉浸式查看模式。</p>
                </div>
            </div>
        </main>
        
        <footer>
            <p>智能气象分析系统 &copy; 2023 | 数据更新于: <span id="data-time">--:--:--</span></p>
            <p>AI分析引擎: DeepSeek-R1 | 天气图来源: 中国气象局国家气象中心 (NMC)</p>
        </footer>
    </div>
    
    <!-- 全屏模式容器 -->
    <div id="fullscreen-overlay" class="fullscreen-overlay" style="display: none;">
        <div class="fullscreen-header">
            <div class="fullscreen-title">
                <i class="fas fa-radar"></i> 全国雷达拼图 (全屏模式)
            </div>
            <button class="close-fullscreen" id="close-fullscreen">
                <i class="fas fa-times"></i> 退出全屏
            </button>
        </div>
        <iframe id="fullscreen-iframe" class="fullscreen-iframe" src="" frameborder="0"></iframe>
    </div>
    
    <script>
        // API配置
        const API_CONFIG = {
            IP_API: 'https://ipapi.co/json/',
            OWM_CURRENT: 'https://api.openweathermap.org/data/2.5/weather',
            OWM_FORECAST: 'https://api.openweathermap.org/data/2.5/forecast',
            OWM_API_KEY: '47a747af7849faec0b3f012363c5a983',
            DEEPSEEK_API: 'https://api.deepseek.com/chat/completions',
            DEEPSEEK_API_KEY: 'sk-a5103293069f46c8ab00c0bf040a7799' // 请替换为您的API密钥
        };
        
        // 天气图标映射
        const WEATHER_ICONS = {
            '01d': 'fa-sun',           // 晴天 (白天)
            '01n': 'fa-moon',          // 晴天 (夜晚)
            '02d': 'fa-cloud-sun',     // 少云 (白天)
            '02n': 'fa-cloud-moon',    // 少云 (夜晚)
            '03d': 'fa-cloud',         // 多云 (白天)
            '03n': 'fa-cloud',         // 多云 (夜晚)
            '04d': 'fa-cloud',         // 阴天 (白天)
            '04n': 'fa-cloud',         // 阴天 (夜晚)
            '09d': 'fa-cloud-rain',    // 小雨 (白天)
            '09n': 'fa-cloud-rain',    // 小雨 (夜晚)
            '10d': 'fa-cloud-showers-heavy', // 大雨 (白天)
            '10n': 'fa-cloud-showers-heavy', // 大雨 (夜晚)
            '11d': 'fa-bolt',          // 雷暴 (白天)
            '11n': 'fa-bolt',          // 雷暴 (夜晚)
            '13d': 'fa-snowflake',     // 雪 (白天)
            '13n': 'fa-snowflake',     // 雪 (夜晚)
            '50d': 'fa-smog',          // 雾 (白天)
            '50n': 'fa-smog'           // 雾 (夜晚)
        };
        
        // 天气描述中英文对照
        const WEATHER_DESCRIPTION_CN = {
            'clear sky': '晴天',
            'few clouds': '少云',
            'scattered clouds': '分散云',
            'broken clouds': '碎云',
            'overcast clouds': '阴天',
            'light rain': '小雨',
            'moderate rain': '中雨',
            'heavy rain': '大雨',
            'thunderstorm': '雷暴',
            'snow': '雪',
            'mist': '薄雾',
            'fog': '雾',
            'haze': '霾',
            'smoke': '烟',
            'dust': '灰尘',
            'sand': '沙尘',
            'ash': '火山灰',
            'squall': '狂风',
            'tornado': '龙卷风'
        };
        
        // 风向映射
        const WIND_DIRECTIONS = {
            'N': '北风', 'NNE': '东北偏北风', 'NE': '东北风', 'ENE': '东北偏东风',
            'E': '东风', 'ESE': '东南偏东风', 'SE': '东南风', 'SSE': '东南偏南风',
            'S': '南风', 'SSW': '西南偏南风', 'SW': '西南风', 'WSW': '西南偏西风',
            'W': '西风', 'WNW': '西北偏西风', 'NW': '西北风', 'NNW': '西北偏北风'
        };
        
        // 全局变量
        let currentScale = 1.0;
        let radarIframe = null;
        
        // 获取用户位置信息
        async function getUserLocation() {
            try {
                const response = await fetch(API_CONFIG.IP_API);
                const data = await response.json();
                
                // 更新API状态
                document.getElementById('ipapi-status').classList.remove('offline');
                
                return {
                    city: data.city || '北京',
                    region: data.region || '北京市',
                    country: data.country_name || '中国',
                    lat: data.latitude || 39.9042,
                    lon: data.longitude || 116.4074,
                    ip: data.ip || "N/A"
                };
            } catch (error) {
                // 显示错误信息
                showError('IP定位服务暂时不可用，使用默认位置数据');
                document.getElementById('ipapi-status').classList.add('offline');
                
                // 返回默认位置（北京）
                return {
                    city: "北京",
                    region: "北京市",
                    country: "中国",
                    lat: 39.9042,
                    lon: 116.4074,
                    ip: "N/A"
                };
            }
        }
        
        // 获取天气数据
        async function getWeatherData(lat, lon) {
            try {
                // 获取当前天气
                const currentUrl = `${API_CONFIG.OWM_CURRENT}?lat=${lat}&lon=${lon}&appid=${API_CONFIG.OWM_API_KEY}&units=metric`;
                const currentResponse = await fetch(currentUrl);
                
                if (!currentResponse.ok) {
                    throw new Error('当前天气数据获取失败');
                }
                const currentData = await currentResponse.json();
                
                // 获取天气预报
                const forecastUrl = `${API_CONFIG.OWM_FORECAST}?lat=${lat}&lon=${lon}&appid=${API_CONFIG.OWM_API_KEY}&units=metric`;
                const forecastResponse = await fetch(forecastUrl);
                
                if (!forecastResponse.ok) {
                    throw new Error('天气预报数据获取失败');
                }
                const forecastData = await forecastResponse.json();
                
                // 更新API状态
                document.getElementById('owm-status').classList.remove('offline');
                
                // 返回组合数据
                return {
                    current: currentData,
                    forecast: forecastData
                };
            } catch (error) {
                // 显示错误信息
                showError('气象数据服务暂时不可用：' + error.message);
                document.getElementById('owm-status').classList.add('offline');
                
                // 返回模拟数据
                return generateMockWeatherData();
            }
        }
        
        // 翻译天气描述为中文
        function translateWeatherDescription(description) {
            return WEATHER_DESCRIPTION_CN[description.toLowerCase()] || description;
        }
        
        // 生成DeepSeek AI气象分析
        async function generateAIAnalysis(weatherData, location) {
            try {
                // 构建系统提示词
                const systemPrompt = `你是一位资深气象学家，请基于提供的实时气象数据生成专业分析报告：
                1. 解释当前天气系统形成原因
                2. 预测未来24小时天气趋势
                3. 提供专业防护建议
                
                要求：
                - 使用中文回答，保持专业但易懂
                - 包含数据支持的关键结论
                - 对专业术语添加简短解释
                - 字数300-500字`;
                
                // 构建用户消息
                const userMessage = `位置：${location.city}, ${location.region}, ${location.country} (纬度: ${location.lat}, 经度: ${location.lon})
                当前时间：${new Date().toLocaleString()}
                
                当前天气：
                - 天气状况: ${translateWeatherDescription(weatherData.current.weather[0].description)}
                - 温度: ${weatherData.current.main.temp}°C (体感 ${weatherData.current.main.feels_like}°C)
                - 气压: ${weatherData.current.main.pressure} hPa
                - 湿度: ${weatherData.current.main.humidity}%
                - 风速: ${weatherData.current.wind.speed} m/s
                - 云量: ${weatherData.current.clouds.all}%
                
                未来24小时预报：
                ${weatherData.forecast.list.slice(0, 8).map((item, index) => {
                    const time = new Date(item.dt * 1000).toLocaleTimeString([], {hour: '2-digit', minute:'2-digit'});
                    return `${time}: ${translateWeatherDescription(item.weather[0].description)}, ${item.main.temp}°C (降水概率 ${Math.round(item.pop * 100)}%)`;
                }).join('\n')}`;
                
                // 构建API请求
                const response = await fetch(API_CONFIG.DEEPSEEK_API, {
                    method: 'POST',
                    headers: {
                        'Content-Type': 'application/json',
                        'Authorization': `Bearer ${API_CONFIG.DEEPSEEK_API_KEY}`
                    },
                    body: JSON.stringify({
                        model: "deepseek-reasoner",
                        messages: [
                            { role: "system", content: systemPrompt },
                            { role: "user", content: userMessage }
                        ],
                        temperature: 0.3,
                        max_tokens: 1000
                    })
                });
                
                if (!response.ok) {
                    throw new Error(`AI API请求失败: ${response.status}`);
                }
                
                const data = await response.json();
                const aiResponse = data.choices[0].message.content;
                
                // 更新AI状态
                document.getElementById('ai-status').classList.remove('offline');
                
                return {
                    fullAnalysis: aiResponse
                };
                
            } catch (error) {
                console.error('AI分析失败:', error);
                // 更新AI状态
                document.getElementById('ai-status').classList.add('offline');
                
                return {
                    fullAnalysis: `AI分析服务暂时不可用: ${error.message}。使用模拟分析数据。\n当前天气系统受西伯利亚高压影响，形成稳定的晴朗天气。未来24小时天气变化平稳，无明显降水迹象。当前无特殊气象风险。`
                };
            }
        }
        
        // 获取风向
        function getWindDirection(degrees) {
            const directions = Object.keys(WIND_DIRECTIONS);
            const index = Math.round(degrees / 22.5) % 16;
            return WIND_DIRECTIONS[directions[index]] || '未知风向';
        }
        
        // 显示错误信息
        function showError(message) {
            const errorEl = document.getElementById('error-message');
            document.getElementById('error-text').textContent = message;
            errorEl.style.display = 'block';
        }
        
        // 更新页面天气信息
        async function updateWeatherUI(weatherData, location) {
            // 更新位置信息
            document.getElementById('location').textContent = `${location.city}, ${location.region}, ${location.country}`;
            
            const current = weatherData.current;
            const forecast = weatherData.forecast;
            
            // 更新当前天气
            const currentIconCode = current.weather[0].icon;
            document.getElementById('current-icon').innerHTML = `<i class="fas ${WEATHER_ICONS[currentIconCode] || 'fa-cloud'}"></i>`;
            document.getElementById('current-temp').textContent = `${Math.round(current.main.temp)}°C`;
            
            // 翻译天气描述
            const weatherDesc = translateWeatherDescription(current.weather[0].description);
            document.getElementById('weather-desc').textContent = weatherDesc;
            
            document.getElementById('feels-like').textContent = `${Math.round(current.main.feels_like)}°C`;
            
            // 更新详细数据
            document.getElementById('humidity').textContent = `${current.main.humidity}%`;
            document.getElementById('wind-speed').textContent = `${current.wind.speed} m/s`;
            document.getElementById('pressure').textContent = `${current.main.pressure} hPa`;
            document.getElementById('clouds').textContent = `${current.clouds.all}%`;
            
            // 转换日出日落时间
            const sunriseTime = new Date(current.sys.sunrise * 1000);
            const sunsetTime = new Date(current.sys.sunset * 1000);
            document.getElementById('sunrise').textContent = `${sunriseTime.getHours()}:${sunriseTime.getMinutes().toString().padStart(2, '0')}`;
            document.getElementById('sunset').textContent = `${sunsetTime.getHours()}:${sunsetTime.getMinutes().toString().padStart(2, '0')}`;
            
            // 更新小时预报
            const hourlyContainer = document.getElementById('hourly-forecast');
            hourlyContainer.innerHTML = '';
            
            // 获取未来24小时预报（每3小时一个数据点）
            const hourlyForecasts = forecast.list.slice(0, 8);
            
            hourlyForecasts.forEach(hour => {
                const date = new Date(hour.dt * 1000);
                const hourStr = date.getHours().toString().padStart(2, '0') + ':00';
                const iconCode = hour.weather[0].icon;
                
                // 翻译小时天气描述
                const hourDesc = translateWeatherDescription(hour.weather[0].description);
                
                const hourElement = document.createElement('div');
                hourElement.className = 'hour-item';
                hourElement.innerHTML = `
                    <div class="hour-time">${hourStr}</div>
                    <div class="hour-icon"><i class="fas ${WEATHER_ICONS[iconCode] || 'fa-cloud'}"></i></div>
                    <div class="hour-temp">${Math.round(hour.main.temp)}°C</div>
                    <div class="hour-desc">${hourDesc}</div>
                    ${hour.pop > 0 ? `<div class="hour-pop">降水: ${Math.round(hour.pop * 100)}%</div>` : ''}
                `;
                hourlyContainer.appendChild(hourElement);
            });
            
            // 生成温度图表
            const tempData = hourlyForecasts.map(h => h.main.temp);
            const timeLabels = hourlyForecasts.map(h => {
                const date = new Date(h.dt * 1000);
                return date.getHours().toString().padStart(2, '0') + ':00';
            });
            
            const ctx = document.getElementById('weather-chart').getContext('2d');
            
            // 如果图表已存在，销毁它
            if (window.weatherChart) {
                window.weatherChart.destroy();
            }
            
            window.weatherChart = new Chart(ctx, {
                type: 'line',
                data: {
                    labels: timeLabels,
                    datasets: [{
                        label: '温度 (°C)',
                        data: tempData,
                        borderColor: '#00bcd4',
                        backgroundColor: 'rgba(0, 188, 212, 0.1)',
                        borderWidth: 3,
                        pointRadius: 5,
                        pointBackgroundColor: '#fff',
                        pointBorderColor: '#00bcd4',
                        tension: 0.4,
                        fill: true
                    }]
                },
                options: {
                    responsive: true,
                    maintainAspectRatio: false,
                    scales: {
                        y: {
                            grid: {
                                color: 'rgba(255, 255, 255, 0.1)'
                            },
                            ticks: {
                                color: 'rgba(255, 255, 255, 0.7)',
                                callback: function(value) {
                                    return value + '°C';
                                }
                            }
                        },
                        x: {
                            grid: {
                                color: 'rgba(255, 255, 255, 0.1)'
                            },
                            ticks: {
                                color: 'rgba(255, 255, 255, 0.7)'
                            }
                        }
                    },
                    plugins: {
                        legend: {
                            labels: {
                                color: 'rgba(255, 255, 255, 0.8)',
                                font: {
                                    size: 14
                                }
                            }
                        },
                        tooltip: {
                            backgroundColor: 'rgba(10, 25, 41, 0.9)',
                            titleColor: '#00bcd4',
                            bodyColor: '#ffffff',
                            borderColor: 'rgba(255, 255, 255, 0.1)',
                            borderWidth: 1,
                            callbacks: {
                                label: function(context) {
                                    return `温度: ${context.parsed.y}°C`;
                                }
                            }
                        }
                    }
                }
            });
            
            // 生成AI分析
            const aiAnalysis = await generateAIAnalysis(weatherData, location);
            
            // 更新AI分析内容
            document.getElementById('ai-analysis-content').innerHTML = `
                <h3><i class="fas fa-chart-line"></i> DeepSeek气象分析报告</h3>
                <p>${aiAnalysis.fullAnalysis.replace(/\n/g, '<br>')}</p>
                <div class="ai-prompt">
                    <i class="fas fa-microchip"></i> 本分析由DeepSeek-R1气象AI引擎生成
                </div>
            `;
            
            // 更新时间
            const nowTime = new Date();
            const timeStr = nowTime.toLocaleTimeString();
            const dateStr = nowTime.toLocaleDateString();
            
            document.getElementById('update-time').textContent = `${dateStr} ${timeStr}`;
            document.getElementById('data-time').textContent = `${dateStr} ${timeStr}`;
            
            // 更新气象台图片时间
            document.getElementById('radar-update-time').textContent = `${dateStr} ${timeStr}`;
            
            // 隐藏加载状态，显示内容
            document.getElementById('loading').style.display = 'none';
            document.getElementById('weather-content').style.display = 'block';
        }
        
        // 生成模拟天气数据
        function generateMockWeatherData() {
            const now = Math.floor(Date.now() / 1000);
            return {
                current: {
                    weather: [{ id: 800, main: 'Clear', description: '晴天', icon: '01d' }],
                    main: {
                        temp: 25.3,
                        feels_like: 26.1,
                        pressure: 1015,
                        humidity: 45,
                        temp_min: 23.5,
                        temp_max: 27.8
                    },
                    wind: {
                        speed: 3.2,
                        deg: 180
                    },
                    clouds: {
                        all: 10
                    },
                    sys: {
                        sunrise: now - 36000,
                        sunset: now + 18000
                    },
                    visibility: 10000,
                    dt: now,
                    id: 1816670,
                    name: 'Beijing'
                },
                forecast: {
                    list: Array.from({ length: 8 }, (_, i) => ({
                        dt: now + i * 3 * 3600,
                        main: {
                            temp: 24 + i * 0.5,
                            feels_like: 24.5 + i * 0.5,
                            temp_min: 23.5 + i * 0.5,
                            temp_max: 25.5 + i * 0.5,
                            pressure: 1015 - i,
                            humidity: 45 + i * 2,
                            sea_level: 1015,
                            grnd_level: 1010
                        },
                        weather: [
                            {
                                id: 800 - i * 10,
                                main: i > 5 ? 'Rain' : 'Clouds',
                                description: i > 5 ? 'light rain' : 'scattered clouds',
                                icon: i > 5 ? '10d' : '02d'
                            }
                        ],
                        clouds: {
                            all: 10 + i * 10
                        },
                        wind: {
                            speed: 2.5 + i * 0.2,
                            deg: 180 + i * 10
                        },
                        pop: i > 5 ? 0.6 : 0.1,
                        dt_txt: new Date(now + i * 3 * 3600 * 1000).toISOString()
                    }))
                }
            };
        }
        
        // 刷新雷达图
        function refreshRadar() {
            const radarIframe = document.getElementById('radar-iframe');
            const radarLoading = document.getElementById('radar-loading');
            const now = new Date();
            const timeStr = now.toLocaleTimeString();
            const dateStr = now.toLocaleDateString();
            
            // 显示加载状态
            radarLoading.style.display = 'flex';
            
            // 设置新的iframe src（添加时间戳防止缓存）
            radarIframe.src = `https://www.nmc.cn/publish/radar/chinaall.html?t=${now.getTime()}`;
            
            // 当iframe加载完成时隐藏加载状态
            radarIframe.onload = () => {
                radarLoading.style.display = 'none';
                document.getElementById('radar-update-time').textContent = `${dateStr} ${timeStr}`;
                document.getElementById('nmc-status').classList.remove('offline');
            };
            
            // 处理加载错误
            radarIframe.onerror = () => {
                radarLoading.innerHTML = `
                    <div class="spinner nmc-spinner"></div>
                    <p>雷达图加载失败，正在重试...</p>
                `;
                document.getElementById('nmc-status').classList.add('offline');
                // 3秒后重试
                setTimeout(refreshRadar, 3000);
            };
        }
        
        // 调整雷达图缩放
        function adjustRadarScale(scale) {
            const radarIframe = document.getElementById('radar-iframe');
            currentScale = scale;
            radarIframe.style.transform = `scale(${scale})`;
            radarIframe.style.transformOrigin = 'top left';
            document.getElementById('radar-scale').textContent = `${Math.round(scale * 100)}%`;
        }
        
        // 初始化雷达图交互
        function initRadarControls() {
            const zoomInBtn = document.getElementById('zoom-in');
            const zoomOutBtn = document.getElementById('zoom-out');
            const resetZoomBtn = document.getElementById('reset-zoom');
            const refreshBtn = document.getElementById('refresh-radar');
            const fullscreenBtn = document.getElementById('fullscreen-btn');
            const closeFullscreenBtn = document.getElementById('close-fullscreen');
            const fullscreenOverlay = document.getElementById('fullscreen-overlay');
            
            // 缩放控制
            zoomInBtn.addEventListener('click', () => {
                if (currentScale < 1.5) {
                    adjustRadarScale(currentScale + 0.1);
                }
            });
            
            zoomOutBtn.addEventListener('click', () => {
                if (currentScale > 0.7) {
                    adjustRadarScale(currentScale - 0.1);
                }
            });
            
            resetZoomBtn.addEventListener('click', () => {
                adjustRadarScale(1.0);
            });
            
            // 刷新雷达图
            refreshBtn.addEventListener('click', refreshRadar);
            
            // 全屏模式
            fullscreenBtn.addEventListener('click', () => {
                const radarIframe = document.getElementById('radar-iframe');
                const fullscreenIframe = document.getElementById('fullscreen-iframe');
                
                fullscreenIframe.src = radarIframe.src;
                fullscreenOverlay.style.display = 'flex';
                document.body.style.overflow = 'hidden';
            });
            
            closeFullscreenBtn.addEventListener('click', () => {
                fullscreenOverlay.style.display = 'none';
                document.body.style.overflow = 'auto';
            });
        }
        
        // 主初始化函数
        async function initWeatherApp() {
            try {
                // 显示加载状态
                document.getElementById('loading').style.display = 'block';
                
                // 获取用户位置
                const location = await getUserLocation();
                
                // 获取天气数据
                const weatherData = await getWeatherData(location.lat, location.lon);
                
                // 更新UI
                await updateWeatherUI(weatherData, location);
                
                // 初始化雷达图
                refreshRadar();
                
                // 初始化雷达图控制
                initRadarControls();
            } catch (error) {
                showError('应用程序初始化失败: ' + error.message);
                document.getElementById('loading').style.display = 'none';
            }
        }
        
        // 页面加载完成后执行
        document.addEventListener('DOMContentLoaded', initWeatherApp);
        
        // 每30分钟刷新一次数据
        setInterval(initWeatherApp, 30 * 60 * 1000);
        
        // 每10分钟刷新雷达图
        setInterval(refreshRadar, 10 * 60 * 1000);
    </script>
</body>
</html>
