<!DOCTYPE html>
<html lang="hi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>छत्तीसगढ़ी संस्कृति हब</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/three@0.128.0/examples/js/controls/OrbitControls.js"></script>
    <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@400;500;600;700&family=Noto+Sans+Devanagari:wght@400;500;600;700&display=swap" rel="stylesheet">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        :root {
            --primary-blue: #3b82f6;
            --secondary-blue: #1e40af;
            --accent-green: #10b981;
            --accent-purple: #8b5cf6;
            --accent-orange: #f59e0b;
            --accent-red: #ef4444;
            --text-dark: #1f2937;
            --text-light: #6b7280;
            --bg-light: #ffffff;
            --bg-secondary: #f3f4f6;
            --card-bg: rgba(255, 255, 255, 0.95);
            --border-light: rgba(255, 255, 255, 0.2);
            --waterfall-bg: linear-gradient(180deg, #87ceeb 0%, #4682b4 20%, #2e8b57 40%, #228b22 60%, #006400 80%, #013220 100%);
        }
        
        [data-theme="dark"] {
            --primary-blue: #60a5fa;
            --secondary-blue: #3b82f6;
            --accent-green: #34d399;
            --accent-purple: #a78bfa;
            --accent-orange: #fbbf24;
            --accent-red: #f87171;
            --text-dark: #f9fafb;
            --text-light: #d1d5db;
            --bg-light: #111827;
            --bg-secondary: #1f2937;
            --card-bg: rgba(31, 41, 55, 0.95);
            --border-light: rgba(75, 85, 99, 0.3);
            --waterfall-bg: linear-gradient(180deg, #0c4a6e 0%, #075985 20%, #064e3b 40%, #065f46 60%, #064e3b 80%, #022c22 100%);
        }
        
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }
        
        body { 
            font-family: 'Poppins', 'Noto Sans Devanagari', sans-serif;
            background: linear-gradient(135deg, #2d5016 0%, #1a3d0a 50%, #0f2905 100%);
            min-height: 100vh;
            position: relative;
            overflow-x: hidden;
            transition: background-color 0.3s ease;
        }
        
        [data-theme="dark"] body {
            background: linear-gradient(135deg, #0f172a 0%, #1e293b 50%, #0f172a 100%);
        }
        
        .waterfall-background {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            z-index: -2;
            background: var(--waterfall-bg);
            transition: background 0.5s ease;
        }
        
        .waterfall-container {
            position: fixed;
            top: 0;
            left: 50%;
            transform: translateX(-50%);
            width: 200px;
            height: 100vh;
            z-index: -1;
            overflow: hidden;
        }
        
        .waterfall {
            position: absolute;
            top: -50px;
            left: 50%;
            transform: translateX(-50%);
            width: 80px;
            height: 120vh;
            background: linear-gradient(180deg, 
                rgba(255, 255, 255, 0.9) 0%,
                rgba(173, 216, 230, 0.8) 20%,
                rgba(135, 206, 235, 0.7) 40%,
                rgba(100, 149, 237, 0.6) 60%,
                rgba(70, 130, 180, 0.5) 80%,
                rgba(25, 25, 112, 0.3) 100%);
            animation: waterfallFlow 2s linear infinite;
            border-radius: 40px;
            box-shadow: 
                0 0 20px rgba(255, 255, 255, 0.5),
                inset 0 0 20px rgba(255, 255, 255, 0.3);
        }
        
        [data-theme="dark"] .waterfall {
            background: linear-gradient(180deg, 
                rgba(173, 216, 230, 0.7) 0%,
                rgba(135, 206, 235, 0.6) 20%,
                rgba(100, 149, 237, 0.5) 40%,
                rgba(70, 130, 180, 0.4) 60%,
                rgba(25, 25, 112, 0.3) 80%,
                rgba(0, 0, 139, 0.2) 100%);
        }
        
        .waterfall::before {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background: repeating-linear-gradient(
                180deg,
                transparent 0px,
                rgba(255, 255, 255, 0.3) 10px,
                transparent 20px,
                rgba(173, 216, 230, 0.2) 30px
            );
            animation: waterfallTexture 1s linear infinite;
            border-radius: 40px;
        }
        
        .waterfall-splash {
            position: absolute;
            bottom: 0;
            left: 50%;
            transform: translateX(-50%);
            width: 150px;
            height: 50px;
            background: radial-gradient(ellipse at center, 
                rgba(255, 255, 255, 0.8) 0%,
                rgba(173, 216, 230, 0.6) 30%,
                rgba(135, 206, 235, 0.4) 60%,
                transparent 100%);
            border-radius: 50%;
            animation: splash 1.5s ease-in-out infinite;
        }
        
        .forest-trees {
            position: fixed;
            bottom: 0;
            left: 0;
            width: 100%;
            height: 40%;
            z-index: -1;
            background: linear-gradient(180deg, 
                transparent 0%,
                rgba(34, 139, 34, 0.3) 30%,
                rgba(0, 100, 0, 0.6) 70%,
                rgba(1, 50, 32, 0.9) 100%);
            transition: background 0.5s ease;
        }
        
        [data-theme="dark"] .forest-trees {
            background: linear-gradient(180deg, 
                transparent 0%,
                rgba(5, 46, 22, 0.4) 30%,
                rgba(5, 46, 22, 0.7) 70%,
                rgba(1, 9, 5, 0.95) 100%);
        }
        
        .tree {
            position: absolute;
            bottom: 0;
            background: linear-gradient(180deg, 
                #228b22 0%,
                #32cd32 30%,
                #00ff00 60%,
                #adff2f 100%);
            border-radius: 50% 50% 50% 50% / 60% 60% 40% 40%;
            animation: treeSwaying 4s ease-in-out infinite;
        }
        
        [data-theme="dark"] .tree {
            background: linear-gradient(180deg, 
                #166534 0%,
                #15803d 30%,
                #16a34a 60%,
                #4ade80 100%);
        }
        
        .tree:nth-child(1) { left: 5%; width: 60px; height: 80px; animation-delay: 0s; }
        .tree:nth-child(2) { left: 15%; width: 80px; height: 100px; animation-delay: 0.5s; }
        .tree:nth-child(3) { left: 25%; width: 70px; height: 90px; animation-delay: 1s; }
        .tree:nth-child(4) { right: 25%; width: 75px; height: 95px; animation-delay: 1.5s; }
        .tree:nth-child(5) { right: 15%; width: 65px; height: 85px; animation-delay: 2s; }
        .tree:nth-child(6) { right: 5%; width: 85px; height: 105px; animation-delay: 2.5s; }
        
        .birds {
            position: fixed;
            top: 20%;
            left: 0;
            width: 100%;
            height: 60%;
            z-index: -1;
            pointer-events: none;
        }
        
        .bird {
            position: absolute;
            font-size: 20px;
            color: rgba(255, 255, 255, 0.8);
            animation: birdFly 15s linear infinite;
        }
        
        .bird:nth-child(1) { top: 10%; animation-delay: 0s; }
        .bird:nth-child(2) { top: 30%; animation-delay: 3s; }
        .bird:nth-child(3) { top: 50%; animation-delay: 6s; }
        .bird:nth-child(4) { top: 20%; animation-delay: 9s; }
        .bird:nth-child(5) { top: 40%; animation-delay: 12s; }
        
        .water-drops {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            z-index: -1;
            pointer-events: none;
        }
        
        .water-drop {
            position: absolute;
            width: 3px;
            height: 15px;
            background: linear-gradient(180deg, 
                rgba(255, 255, 255, 0.8) 0%,
                rgba(173, 216, 230, 0.6) 50%,
                rgba(135, 206, 235, 0.4) 100%);
            border-radius: 50% 50% 50% 50% / 60% 60% 40% 40%;
            animation: rainDrop 2s linear infinite;
        }
        
        @keyframes waterfallFlow {
            0% { transform: translateX(-50%) translateY(-100px); }
            100% { transform: translateX(-50%) translateY(0px); }
        }
        
        @keyframes waterfallTexture {
            0% { transform: translateY(-30px); }
            100% { transform: translateY(0px); }
        }
        
        @keyframes splash {
            0%, 100% { 
                transform: translateX(-50%) scale(1);
                opacity: 0.8;
            }
            50% { 
                transform: translateX(-50%) scale(1.2);
                opacity: 1;
            }
        }
        
        @keyframes treeSwaying {
            0%, 100% { transform: rotate(-2deg); }
            50% { transform: rotate(2deg); }
        }
        
        @keyframes birdFly {
            0% { 
                left: -50px; 
                transform: translateY(0px);
            }
            25% { 
                transform: translateY(-20px);
            }
            50% { 
                transform: translateY(10px);
            }
            75% { 
                transform: translateY(-15px);
            }
            100% { 
                left: calc(100% + 50px);
                transform: translateY(5px);
            }
        }
        
        @keyframes rainDrop {
            0% { 
                transform: translateY(-20px);
                opacity: 0;
            }
            10% { opacity: 1; }
            90% { opacity: 1; }
            100% { 
                transform: translateY(100vh);
                opacity: 0;
            }
        }
        
        .sound-waves {
            position: fixed;
            bottom: 20px;
            right: 20px;
            z-index: 1000;
            display: flex;
            align-items: center;
            gap: 5px;
            background: rgba(255, 255, 255, 0.1);
            padding: 10px 15px;
            border-radius: 25px;
            backdrop-filter: blur(10px);
            transition: all 0.3s ease;
        }
        
        [data-theme="dark"] .sound-waves {
            background: rgba(0, 0, 0, 0.2);
        }
        
        .sound-wave {
            width: 3px;
            height: 20px;
            background: rgba(255, 255, 255, 0.8);
            border-radius: 2px;
            animation: soundWave 1.5s ease-in-out infinite;
        }
        
        .sound-wave:nth-child(1) { animation-delay: 0s; }
        .sound-wave:nth-child(2) { animation-delay: 0.1s; }
        .sound-wave:nth-child(3) { animation-delay: 0.2s; }
        .sound-wave:nth-child(4) { animation-delay: 0.3s; }
        .sound-wave:nth-child(5) { animation-delay: 0.4s; }
        
        @keyframes soundWave {
            0%, 100% { 
                height: 20px;
                opacity: 0.5;
            }
            50% { 
                height: 35px;
                opacity: 1;
            }
        }
        
        .hindi-text {
            font-family: 'Noto Sans Devanagari', sans-serif;
        }
        
        .chhattisgarhi-text {
            font-family: 'Noto Sans Devanagari', sans-serif;
        }
        
        .tool-card {
            background: var(--card-bg);
            backdrop-filter: blur(10px);
            border: 1px solid var(--border-light);
            transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
            transform-style: preserve-3d;
        }
        
        .tool-card:hover {
            transform: translateY(-8px) scale(1.02);
            box-shadow: 0 20px 40px rgba(0, 0, 0, 0.15);
        }
        
        .tool-icon {
            background: linear-gradient(135deg, var(--primary-blue), var(--secondary-blue));
            color: white;
            width: 60px;
            height: 60px;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 24px;
            margin: 0 auto 1rem;
            transition: all 0.3s ease;
        }
        
        .tool-card:hover .tool-icon {
            transform: rotateY(360deg) scale(1.1);
        }
        
        .btn-primary {
            background: linear-gradient(135deg, var(--primary-blue), var(--secondary-blue));
            color: white;
            border: none;
            padding: 12px 24px;
            border-radius: 8px;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.3s ease;
            transform: translateZ(0);
        }
        
        .btn-primary:hover {
            transform: translateY(-2px);
            box-shadow: 0 8px 20px rgba(59, 130, 246, 0.4);
        }
        
        .btn-secondary {
            background: linear-gradient(135deg, var(--accent-green), #059669);
            color: white;
            border: none;
            padding: 8px 16px;
            border-radius: 6px;
            font-weight: 500;
            cursor: pointer;
            transition: all 0.3s ease;
        }
        
        .btn-secondary:hover {
            transform: translateY(-1px);
            box-shadow: 0 4px 12px rgba(16, 185, 129, 0.4);
        }
        
        .btn-danger {
            background: linear-gradient(135deg, #ef4444, #dc2626);
            color: white;
            border: none;
            padding: 8px 16px;
            border-radius: 6px;
            font-weight: 500;
            cursor: pointer;
            transition: all 0.3s ease;
        }
        
        .btn-danger:hover {
            transform: translateY(-1px);
            box-shadow: 0 4px 12px rgba(239, 68, 68, 0.4);
        }
        
        .input-field {
            width: 100%;
            padding: 12px 16px;
            border: 2px solid #e5e7eb;
            border-radius: 8px;
            font-size: 16px;
            transition: all 0.3s ease;
            background: rgba(255, 255, 255, 0.9);
        }
        
        .input-field:focus {
            outline: none;
            border-color: var(--primary-blue);
            box-shadow: 0 0 0 3px rgba(59, 130, 246, 0.1);
            transform: translateY(-1px);
        }
        
        [data-theme="dark"] .input-field {
            background: rgba(31, 41, 55, 0.9);
            border-color: #4b5563;
            color: #f9fafb;
        }
        
        .result-box {
            background: linear-gradient(135deg, #f0f9ff, #e0f2fe);
            border: 2px solid #0ea5e9;
            border-radius: 12px;
            padding: 20px;
            text-align: center;
            margin-top: 20px;
            animation: slideIn 0.5s ease;
        }
        
        [data-theme="dark"] .result-box {
            background: linear-gradient(135deg, #0c4a6e, #075985);
            border-color: #0284c7;
            color: #f0f9ff;
        }
        
        @keyframes slideIn {
            from {
                opacity: 0;
                transform: translateY(20px);
            }
            to {
                opacity: 1;
                transform: translateY(0);
            }
        }
        
        .todo-item {
            background: rgba(255, 255, 255, 0.9);
            border: 1px solid #e5e7eb;
            border-radius: 8px;
            padding: 12px 16px;
            margin-bottom: 8px;
            display: flex;
            align-items: center;
            justify-content: space-between;
            transition: all 0.3s ease;
        }
        
        [data-theme="dark"] .todo-item {
            background: rgba(31, 41, 55, 0.9);
            border-color: #4b5563;
            color: #f9fafb;
        }
        
        .todo-item:hover {
            transform: translateX(4px);
            box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
        }
        
        .todo-item.completed {
            opacity: 0.6;
            text-decoration: line-through;
            background: rgba(16, 185, 129, 0.1);
        }
        
        .password-strength {
            height: 4px;
            border-radius: 2px;
            margin-top: 8px;
            transition: all 0.3s ease;
        }
        
        .strength-weak { background: #ef4444; }
        .strength-medium { background: #f59e0b; }
        .strength-strong { background: #10b981; }
        
        .recipe-card {
            background: var(--card-bg);
            backdrop-filter: blur(10px);
            border-radius: 16px;
            overflow: hidden;
            transition: all 0.3s ease;
            border: 1px solid var(--border-light);
        }
        
        .recipe-card:hover {
            transform: translateY(-5px);
            box-shadow: 0 15px 35px rgba(0, 0, 0, 0.1);
        }
        
        .heritage-card {
            background: linear-gradient(145deg, var(--card-bg), rgba(240, 248, 255, 0.9));
            backdrop-filter: blur(15px);
            border-radius: 20px;
            overflow: hidden;
            transition: all 0.4s cubic-bezier(0.175, 0.885, 0.32, 1.275);
            border: 2px solid var(--border-light);
            box-shadow: 0 8px 32px rgba(0, 0, 0, 0.1);
            transform-style: preserve-3d;
            perspective: 1000px;
            position: relative;
        }
        
        [data-theme="dark"] .heritage-card {
            background: linear-gradient(145deg, var(--card-bg), rgba(30, 41, 59, 0.9));
            border-color: rgba(75, 85, 99, 0.3);
        }
        
        .heritage-card::before {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background: linear-gradient(135deg, rgba(59, 130, 246, 0.1), rgba(147, 51, 234, 0.1));
            opacity: 0;
            transition: opacity 0.3s ease;
            border-radius: 20px;
        }
        
        .heritage-card:hover::before {
            opacity: 1;
        }
        
        .heritage-card:hover {
            transform: translateY(-15px) rotateX(5deg) rotateY(5deg) scale(1.05);
            box-shadow: 0 25px 60px rgba(0, 0, 0, 0.2), 0 0 0 1px rgba(255, 255, 255, 0.5);
        }
        
        .heritage-icon {
            font-size: 4rem;
            text-align: center;
            margin-bottom: 1rem;
            transition: all 0.4s ease;
            transform-style: preserve-3d;
            text-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
        }
        
        .heritage-card:hover .heritage-icon {
            transform: translateZ(20px) rotateY(360deg) scale(1.2);
            text-shadow: 0 8px 16px rgba(0, 0, 0, 0.2);
        }
        
        .heritage-title {
            font-size: 1.25rem;
            font-weight: bold;
            margin-bottom: 0.75rem;
            color: var(--text-dark);
            transition: all 0.3s ease;
            transform-style: preserve-3d;
        }
        
        .heritage-card:hover .heritage-title {
            transform: translateZ(10px);
            color: var(--primary-blue);
        }
        
        .heritage-content {
            transition: all 0.3s ease;
            transform-style: preserve-3d;
            color: var(--text-light);
        }
        
        .heritage-card:hover .heritage-content {
            transform: translateZ(5px);
        }
        
        .heritage-list-item {
            transition: all 0.2s ease;
            padding: 2px 0;
            border-radius: 4px;
        }
        
        .heritage-card:hover .heritage-list-item:hover {
            background: rgba(59, 130, 246, 0.1);
            transform: translateX(5px);
            padding-left: 8px;
        }
        
        .cultural-quote {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            border-radius: 20px;
            padding: 2rem;
            margin-top: 2rem;
            color: white;
            text-align: center;
            position: relative;
            overflow: hidden;
            transform-style: preserve-3d;
            transition: all 0.4s ease;
        }
        
        .cultural-quote::before {
            content: '';
            position: absolute;
            top: -50%;
            left: -50%;
            width: 200%;
            height: 200%;
            background: linear-gradient(45deg, transparent, rgba(255, 255, 255, 0.1), transparent);
            transform: rotate(45deg);
            transition: all 0.6s ease;
            opacity: 0;
        }
        
        .cultural-quote:hover::before {
            animation: shimmer 1.5s ease-in-out;
            opacity: 1;
        }
        
        .cultural-quote:hover {
            transform: translateY(-5px) scale(1.02);
            box-shadow: 0 20px 40px rgba(0, 0, 0, 0.2);
        }
        
        @keyframes shimmer {
            0% { transform: translateX(-100%) translateY(-100%) rotate(45deg); }
            100% { transform: translateX(100%) translateY(100%) rotate(45deg); }
        }
        
        .heritage-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
            gap: 2rem;
            perspective: 1000px;
        }
        
        @media (max-width: 768px) {
            .heritage-card:hover {
                transform: translateY(-10px) scale(1.02);
            }
            
            .heritage-icon {
                font-size: 3rem;
            }
        }
        
        .recipe-image {
            width: 100%;
            height: 200px;
            background: linear-gradient(135deg, #ff9a9e 0%, #fecfef 50%, #fecfef 100%);
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 4rem;
        }
        
        .floating-particles {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            pointer-events: none;
            z-index: -1;
        }
        
        .particle {
            position: absolute;
            width: 4px;
            height: 4px;
            background: rgba(255, 255, 255, 0.3);
            border-radius: 50%;
            animation: float 15s infinite linear;
        }
        
        [data-theme="dark"] .particle {
            background: rgba(255, 255, 255, 0.2);
        }
        
        @keyframes float {
            0% {
                transform: translateY(100vh) rotate(0deg);
                opacity: 0;
            }
            10% {
                opacity: 1;
            }
            90% {
                opacity: 1;
            }
            100% {
                transform: translateY(-100px) rotate(360deg);
                opacity: 0;
            }
        }
        
        .nav-tab {
            padding: 12px 20px;
            background: rgba(255, 255, 255, 0.1);
            border: none;
            color: white;
            border-radius: 8px;
            cursor: pointer;
            transition: all 0.3s ease;
            margin: 4px;
            font-size: 14px;
        }
        
        [data-theme="dark"] .nav-tab {
            background: rgba(0, 0, 0, 0.2);
        }
        
        .nav-tab:hover {
            background: rgba(255, 255, 255, 0.2);
            transform: translateY(-2px);
        }
        
        .nav-tab.active {
            background: rgba(255, 255, 255, 0.9);
            color: var(--primary-blue);
            transform: translateY(-2px);
        }
        
        [data-theme="dark"] .nav-tab.active {
            background: rgba(59, 130, 246, 0.9);
            color: white;
        }
        
        .tool-section {
            display: none;
            animation: fadeIn 0.5s ease;
        }
        
        .tool-section.active {
            display: block;
        }
        
        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(20px); }
            to { opacity: 1; transform: translateY(0); }
        }
        
        .serving-controls {
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 16px;
            margin: 16px 0;
            padding: 16px;
            background: rgba(59, 130, 246, 0.1);
            border-radius: 12px;
        }
        
        [data-theme="dark"] .serving-controls {
            background: rgba(59, 130, 246, 0.2);
        }
        
        .serving-btn {
            background: var(--primary-blue);
            color: white;
            border: none;
            width: 40px;
            height: 40px;
            border-radius: 50%;
            font-size: 20px;
            font-weight: bold;
            cursor: pointer;
            transition: all 0.3s ease;
        }
        
        .serving-btn:hover {
            background: var(--secondary-blue);
            transform: scale(1.1);
        }
        
        .serving-display {
            font-size: 18px;
            font-weight: 600;
            color: var(--primary-blue);
            min-width: 120px;
            text-align: center;
        }
        
        .festival-card {
            background: var(--card-bg);
            backdrop-filter: blur(10px);
            border-radius: 16px;
            overflow: hidden;
            transition: all 0.3s ease;
            border: 1px solid var(--border-light);
            height: 100%;
        }
        
        .festival-card:hover {
            transform: translateY(-5px);
            box-shadow: 0 15px 35px rgba(0, 0, 0, 0.1);
        }
        
        .festival-image {
            width: 100%;
            height: 180px;
            background: linear-gradient(135deg, #ff9a9e 0%, #fad0c4 100%);
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 3rem;
        }
        
        .folktale-card {
            background: linear-gradient(145deg, var(--card-bg), rgba(240, 248, 255, 0.9));
            backdrop-filter: blur(15px);
            border-radius: 20px;
            overflow: hidden;
            transition: all 0.4s ease;
            border: 2px solid var(--border-light);
            box-shadow: 0 8px 32px rgba(0, 0, 0, 0.1);
            height: 100%;
        }
        
        [data-theme="dark"] .folktale-card {
            background: linear-gradient(145deg, var(--card-bg), rgba(30, 41, 59, 0.9));
            border-color: rgba(75, 85, 99, 0.3);
        }
        
        .folktale-card:hover {
            transform: translateY(-10px);
            box-shadow: 0 20px 40px rgba(0, 0, 0, 0.2);
        }
        
        .folktale-image {
            width: 100%;
            height: 200px;
            background: linear-gradient(135deg, #a1c4fd 0%, #c2e9fb 100%);
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 3rem;
        }
        
        .district-card {
            background: var(--card-bg);
            backdrop-filter: blur(10px);
            border-radius: 16px;
            overflow: hidden;
            transition: all 0.3s ease;
            border: 1px solid var(--border-light);
            height: 100%;
        }
        
        .district-card:hover {
            transform: translateY(-5px);
            box-shadow: 0 15px 35px rgba(0, 0, 0, 0.1);
        }
        
        .district-image {
            width: 100%;
            height: 150px;
            background: linear-gradient(135deg, #84fab0 0%, #8fd3f4 100%);
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 2.5rem;
        }
        
        #canvas-3d {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            z-index: -3;
        }
        
        @media (max-width: 768px) {
            .nav-tab {
                padding: 8px 12px;
                font-size: 12px;
                margin: 2px;
            }
            
            .tool-card {
                margin-bottom: 20px;
            }
            
            .recipe-image {
                height: 150px;
                font-size: 3rem;
            }
        }
        
        /* Weather Widget Styles */
        .weather-widget {
            position: fixed;
            top: 20px;
            right: 20px;
            z-index: 1000;
            background: var(--card-bg);
            backdrop-filter: blur(10px);
            border-radius: 16px;
            padding: 15px;
            box-shadow: 0 8px 32px rgba(0, 0, 0, 0.1);
            border: 1px solid var(--border-light);
            width: 220px;
            transition: all 0.3s ease;
        }
        
        .weather-widget:hover {
            transform: translateY(-5px);
            box-shadow: 0 15px 35px rgba(0, 0, 0, 0.15);
        }
        
        .weather-header {
            display: flex;
            align-items: center;
            justify-content: space-between;
            margin-bottom: 10px;
        }
        
        .weather-location {
            font-weight: 600;
            color: var(--text-dark);
        }
        
        .weather-icon {
            font-size: 24px;
        }
        
        .weather-temp {
            font-size: 28px;
            font-weight: bold;
            color: var(--text-dark);
            margin: 10px 0;
        }
        
        .weather-desc {
            color: var(--text-light);
            font-size: 14px;
            margin-bottom: 10px;
        }
        
        .weather-details {
            display: flex;
            justify-content: space-between;
            font-size: 12px;
            color: var(--text-light);
        }
        
        .weather-detail-item {
            display: flex;
            flex-direction: column;
            align-items: center;
        }
        
        .weather-detail-value {
            font-weight: 600;
            color: var(--text-dark);
            margin-top: 5px;
        }
        
        /* Theme Toggle Styles */
        .theme-toggle {
            position: fixed;
            top: 20px;
            left: 20px;
            z-index: 1000;
            display: flex;
            align-items: center;
            gap: 10px;
            background: var(--card-bg);
            backdrop-filter: blur(10px);
            border-radius: 30px;
            padding: 8px 15px;
            box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
            border: 1px solid var(--border-light);
        }
        
        .theme-toggle-btn {
            background: none;
            border: none;
            font-size: 18px;
            cursor: pointer;
            color: var(--text-dark);
            transition: all 0.3s ease;
            padding: 5px;
            border-radius: 50%;
        }
        
        .theme-toggle-btn:hover {
            background: rgba(59, 130, 246, 0.1);
            transform: scale(1.1);
        }
        
        .theme-toggle-btn.active {
            background: var(--primary-blue);
            color: white;
        }
        
        /* Language Toggle Styles */
        .language-toggle {
            position: fixed;
            top: 80px;
            left: 20px;
            z-index: 1000;
            background: var(--card-bg);
            backdrop-filter: blur(10px);
            border-radius: 30px;
            padding: 5px;
            box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
            border: 1px solid var(--border-light);
            display: flex;
        }
        
        .language-option {
            background: none;
            border: none;
            padding: 8px 15px;
            border-radius: 25px;
            font-size: 14px;
            font-weight: 500;
            cursor: pointer;
            color: var(--text-dark);
            transition: all 0.3s ease;
        }
        
        .language-option:hover {
            background: rgba(59, 130, 246, 0.1);
        }
        
        .language-option.active {
            background: var(--primary-blue);
            color: white;
        }
        
        /* Loading Animation */
        .loader {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(255, 255, 255, 0.9);
            display: flex;
            justify-content: center;
            align-items: center;
            z-index: 9999;
            transition: opacity 0.5s ease;
        }
        
        [data-theme="dark"] .loader {
            background: rgba(17, 24, 39, 0.9);
        }
        
        .loader.hidden {
            opacity: 0;
            pointer-events: none;
        }
        
        .loader-circle {
            width: 50px;
            height: 50px;
            border: 5px solid #f3f3f3;
            border-top: 5px solid var(--primary-blue);
            border-radius: 50%;
            animation: spin 1s linear infinite;
        }
        
        @keyframes spin {
            0% { transform: rotate(0deg); }
            100% { transform: rotate(360deg); }
        }
        
        /* Notification Toast */
        .toast {
            position: fixed;
            bottom: 80px;
            left: 50%;
            transform: translateX(-50%) translateY(100px);
            background: var(--card-bg);
            backdrop-filter: blur(10px);
            border-radius: 10px;
            padding: 15px 25px;
            box-shadow: 0 4px 12px rgba(0, 0, 0, 0.15);
            border: 1px solid var(--border-light);
            display: flex;
            align-items: center;
            gap: 10px;
            z-index: 1000;
            opacity: 0;
            transition: all 0.3s ease;
        }
        
        .toast.show {
            transform: translateX(-50%) translateY(0);
            opacity: 1;
        }
        
        .toast-icon {
            font-size: 20px;
        }
        
        .toast-message {
            color: var(--text-dark);
            font-weight: 500;
        }
        
        .toast.success .toast-icon {
            color: var(--accent-green);
        }
        
        .toast.error .toast-icon {
            color: var(--accent-red);
        }
        
        .toast.info .toast-icon {
            color: var(--primary-blue);
        }
        
        /* Scroll to Top Button */
        .scroll-top {
            position: fixed;
            bottom: 30px;
            right: 30px;
            width: 50px;
            height: 50px;
            border-radius: 50%;
            background: var(--primary-blue);
            color: white;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 20px;
            cursor: pointer;
            box-shadow: 0 4px 12px rgba(0, 0, 0, 0.15);
            opacity: 0;
            visibility: hidden;
            transition: all 0.3s ease;
            z-index: 1000;
        }
        
        .scroll-top.show {
            opacity: 1;
            visibility: visible;
        }
        
        .scroll-top:hover {
            transform: translateY(-5px);
            box-shadow: 0 8px 20px rgba(0, 0, 0, 0.2);
        }
        
        /* Interactive Map Styles */
        .map-container {
            width: 100%;
            height: 400px;
            border-radius: 16px;
            overflow: hidden;
            position: relative;
            margin-top: 20px;
            box-shadow: 0 8px 32px rgba(0, 0, 0, 0.1);
        }
        
        .map-overlay {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0, 0, 0, 0.1);
            display: flex;
            align-items: center;
            justify-content: center;
            z-index: 10;
        }
        
        .map-info {
            background: var(--card-bg);
            backdrop-filter: blur(10px);
            border-radius: 10px;
            padding: 15px;
            max-width: 80%;
            text-align: center;
            box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
        }
        
        .map-info h3 {
            color: var(--text-dark);
            margin-bottom: 10px;
        }
        
        .map-info p {
            color: var(--text-light);
            font-size: 14px;
        }
        
        /* Audio Player Styles */
        .audio-player {
            background: var(--card-bg);
            backdrop-filter: blur(10px);
            border-radius: 16px;
            padding: 20px;
            margin-top: 20px;
            box-shadow: 0 8px 32px rgba(0, 0, 0, 0.1);
            border: 1px solid var(--border-light);
        }
        
        .audio-controls {
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 15px;
            margin-top: 15px;
        }
        
        .audio-btn {
            background: var(--primary-blue);
            color: white;
            border: none;
            width: 40px;
            height: 40px;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            cursor: pointer;
            transition: all 0.3s ease;
        }
        
        .audio-btn:hover {
            transform: scale(1.1);
            background: var(--secondary-blue);
        }
        
        .audio-btn.play-pause {
            width: 50px;
            height: 50px;
            font-size: 20px;
        }
        
        .audio-progress {
            width: 100%;
            height: 6px;
            background: rgba(0, 0, 0, 0.1);
            border-radius: 3px;
            margin-top: 15px;
            position: relative;
            cursor: pointer;
        }
        
        [data-theme="dark"] .audio-progress {
            background: rgba(255, 255, 255, 0.1);
        }
        
        .audio-progress-bar {
            height: 100%;
            background: var(--primary-blue);
            border-radius: 3px;
            width: 0%;
            transition: width 0.1s linear;
        }
        
        .audio-time {
            display: flex;
            justify-content: space-between;
            margin-top: 5px;
            font-size: 12px;
            color: var(--text-light);
        }
        
        .audio-title {
            text-align: center;
            color: var(--text-dark);
            font-weight: 600;
            margin-bottom: 5px;
        }
        
        .audio-artist {
            text-align: center;
            color: var(--text-light);
            font-size: 14px;
        }
        
        /* Image Gallery Styles */
        .gallery-container {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
            gap: 15px;
            margin-top: 20px;
        }
        
        .gallery-item {
            position: relative;
            border-radius: 10px;
            overflow: hidden;
            height: 200px;
            cursor: pointer;
            transition: all 0.3s ease;
        }
        
        .gallery-item:hover {
            transform: scale(1.05);
            box-shadow: 0 8px 20px rgba(0, 0, 0, 0.2);
        }
        
        .gallery-item img {
            width: 100%;
            height: 100%;
            object-fit: cover;
        }
        
        .gallery-overlay {
            position: absolute;
            bottom: 0;
            left: 0;
            width: 100%;
            background: linear-gradient(to top, rgba(0, 0, 0, 0.8), transparent);
            padding: 15px;
            color: white;
            transform: translateY(100%);
            transition: transform 0.3s ease;
        }
        
        .gallery-item:hover .gallery-overlay {
            transform: translateY(0);
        }
        
        .gallery-title {
            font-weight: 600;
            margin-bottom: 5px;
        }
        
        .gallery-desc {
            font-size: 12px;
            opacity: 0.8;
        }
        
        /* Lightbox Styles */
        .lightbox {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0, 0, 0, 0.9);
            display: flex;
            align-items: center;
            justify-content: center;
            z-index: 2000;
            opacity: 0;
            visibility: hidden;
            transition: all 0.3s ease;
        }
        
        .lightbox.show {
            opacity: 1;
            visibility: visible;
        }
        
        .lightbox-content {
            max-width: 90%;
            max-height: 90%;
            position: relative;
        }
        
        .lightbox-img {
            max-width: 100%;
            max-height: 90vh;
            border-radius: 5px;
            box-shadow: 0 0 20px rgba(0, 0, 0, 0.5);
        }
        
        .lightbox-close {
            position: absolute;
            top: -40px;
            right: 0;
            color: white;
            font-size: 30px;
            cursor: pointer;
            transition: all 0.3s ease;
        }
        
        .lightbox-close:hover {
            transform: scale(1.2);
        }
        
        .lightbox-caption {
            position: absolute;
            bottom: -40px;
            left: 0;
            width: 100%;
            text-align: center;
            color: white;
            font-size: 16px;
        }
        
        /* 3D Model Viewer Styles */
        .model-viewer {
            width: 100%;
            height: 400px;
            border-radius: 16px;
            overflow: hidden;
            position: relative;
            margin-top: 20px;
            box-shadow: 0 8px 32px rgba(0, 0, 0, 0.1);
        }
        
        .model-controls {
            position: absolute;
            bottom: 20px;
            left: 50%;
            transform: translateX(-50%);
            display: flex;
            gap: 10px;
            background: var(--card-bg);
            backdrop-filter: blur(10px);
            border-radius: 30px;
            padding: 5px;
            box-shadow: 0 4px 12px rgba(0, 0, 0, 0.15);
        }
        
        .model-btn {
            background: none;
            border: none;
            width: 40px;
            height: 40px;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            cursor: pointer;
            color: var(--text-dark);
            transition: all 0.3s ease;
        }
        
        .model-btn:hover {
            background: rgba(59, 130, 246, 0.1);
            transform: scale(1.1);
        }
        
        /* Countdown Timer Styles */
        .countdown-container {
            background: var(--card-bg);
            backdrop-filter: blur(10px);
            border-radius: 16px;
            padding: 20px;
            margin-top: 20px;
            box-shadow: 0 8px 32px rgba(0, 0, 0, 0.1);
            border: 1px solid var(--border-light);
            text-align: center;
        }
        
        .countdown-title {
            color: var(--text-dark);
            font-weight: 600;
            margin-bottom: 15px;
        }
        
        .countdown-timer {
            display: flex;
            justify-content: center;
            gap: 15px;
        }
        
        .countdown-item {
            display: flex;
            flex-direction: column;
            align-items: center;
        }
        
        .countdown-value {
            background: var(--primary-blue);
            color: white;
            width: 60px;
            height: 60px;
            border-radius: 10px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 24px;
            font-weight: bold;
            margin-bottom: 5px;
        }
        
        .countdown-label {
            color: var(--text-light);
            font-size: 12px;
        }
        
        /* Virtual Tour Styles */
        .tour-container {
            position: relative;
            width: 100%;
            height: 500px;
            border-radius: 16px;
            overflow: hidden;
            margin-top: 20px;
            box-shadow: 0 8px 32px rgba(0, 0, 0, 0.1);
        }
        
        .tour-scene {
            width: 100%;
            height: 100%;
            position: relative;
            background: url('https://images.unsplash.com/photo-1543429776-2782fc40f817?ixlib=rb-4.0.3&auto=format&fit=crop&w=1200&q=80') center/cover no-repeat;
        }
        
        .tour-hotspot {
            position: absolute;
            width: 30px;
            height: 30px;
            border-radius: 50%;
            background: rgba(255, 255, 255, 0.8);
            display: flex;
            align-items: center;
            justify-content: center;
            cursor: pointer;
            box-shadow: 0 2px 8px rgba(0, 0, 0, 0.3);
            animation: pulse 2s infinite;
        }
        
        @keyframes pulse {
            0% { box-shadow: 0 0 0 0 rgba(255, 255, 255, 0.7); }
            70% { box-shadow: 0 0 0 10px rgba(255, 255, 255, 0); }
            100% { box-shadow: 0 0 0 0 rgba(255, 255, 255, 0); }
        }
        
        .tour-tooltip {
            position: absolute;
            bottom: 40px;
            left: 50%;
            transform: translateX(-50%);
            background: var(--card-bg);
            backdrop-filter: blur(10px);
            border-radius: 10px;
            padding: 10px 15px;
            white-space: nowrap;
            box-shadow: 0 4px 12px rgba(0, 0, 0, 0.15);
            opacity: 0;
            visibility: hidden;
            transition: all 0.3s ease;
        }
        
        .tour-hotspot:hover .tour-tooltip {
            opacity: 1;
            visibility: visible;
        }
        
        .tour-controls {
            position: absolute;
            bottom: 20px;
            left: 50%;
            transform: translateX(-50%);
            display: flex;
            gap: 10px;
            background: var(--card-bg);
            backdrop-filter: blur(10px);
            border-radius: 30px;
            padding: 5px;
            box-shadow: 0 4px 12px rgba(0, 0, 0, 0.15);
        }
        
        /* Interactive Timeline Styles */
        .timeline-container {
            position: relative;
            margin: 40px 0;
        }
        
        .timeline-line {
            position: absolute;
            top: 0;
            left: 50%;
            transform: translateX(-50%);
            width: 4px;
            height: 100%;
            background: var(--primary-blue);
        }
        
        .timeline-item {
            position: relative;
            margin-bottom: 50px;
        }
        
        .timeline-item:nth-child(odd) {
            padding-right: calc(50% + 30px);
            text-align: right;
        }
        
        .timeline-item:nth-child(even) {
            padding-left: calc(50% + 30px);
            text-align: left;
        }
        
        .timeline-dot {
            position: absolute;
            top: 0;
            width: 20px;
            height: 20px;
            border-radius: 50%;
            background: var(--primary-blue);
            left: 50%;
            transform: translateX(-50%);
            box-shadow: 0 0 0 4px rgba(59, 130, 246, 0.2);
        }
        
        .timeline-content {
            background: var(--card-bg);
            backdrop-filter: blur(10px);
            border-radius: 10px;
            padding: 20px;
            box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
            border: 1px solid var(--border-light);
        }
        
        .timeline-date {
            color: var(--primary-blue);
            font-weight: 600;
            margin-bottom: 5px;
        }
        
        .timeline-title {
            color: var(--text-dark);
            font-weight: 600;
            margin-bottom: 10px;
        }
        
        .timeline-desc {
            color: var(--text-light);
            font-size: 14px;
        }
        
        @media (max-width: 768px) {
            .timeline-line {
                left: 20px;
            }
            
            .timeline-item {
                padding-left: 50px !important;
                padding-right: 0 !important;
                text-align: left !important;
            }
            
            .timeline-dot {
                left: 20px;
            }
        }
        
        /* AR/VR Viewer Styles */
        .arvr-container {
            width: 100%;
            height: 500px;
            border-radius: 16px;
            overflow: hidden;
            position: relative;
            margin-top: 20px;
            background: #000;
        }
        
        .arvr-controls {
            position: absolute;
            bottom: 20px;
            left: 50%;
            transform: translateX(-50%);
            display: flex;
            gap: 10px;
            background: var(--card-bg);
            backdrop-filter: blur(10px);
            border-radius: 30px;
            padding: 5px;
            box-shadow: 0 4px 12px rgba(0, 0, 0, 0.15);
            z-index: 10;
        }
        
        .arvr-btn {
            background: none;
            border: none;
            width: 40px;
            height: 40px;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            cursor: pointer;
            color: var(--text-dark);
            transition: all 0.3s ease;
        }
        
        .arvr-btn:hover {
            background: rgba(59, 130, 246, 0.1);
            transform: scale(1.1);
        }
        
        .arvr-info {
            position: absolute;
            top: 20px;
            left: 20px;
            background: var(--card-bg);
            backdrop-filter: blur(10px);
            border-radius: 10px;
            padding: 15px;
            max-width: 300px;
            box-shadow: 0 4px 12px rgba(0, 0, 0, 0.15);
            z-index: 10;
        }
        
        .arvr-title {
            color: var(--text-dark);
            font-weight: 600;
            margin-bottom: 5px;
        }
        
        .arvr-desc {
            color: var(--text-light);
            font-size: 14px;
        }
        
        /* Chatbot Styles */
        .chatbot-container {
            position: fixed;
            bottom: 20px;
            right: 20px;
            width: 350px;
            height: 500px;
            background: var(--card-bg);
            backdrop-filter: blur(10px);
            border-radius: 16px;
            box-shadow: 0 8px 32px rgba(0, 0, 0, 0.15);
            border: 1px solid var(--border-light);
            display: flex;
            flex-direction: column;
            z-index: 1000;
            transform: translateY(calc(100% + 40px));
            transition: transform 0.3s ease;
        }
        
        .chatbot-container.open {
            transform: translateY(0);
        }
        
        .chatbot-header {
            padding: 15px;
            border-bottom: 1px solid var(--border-light);
            display: flex;
            align-items: center;
            justify-content: space-between;
        }
        
        .chatbot-title {
            color: var(--text-dark);
            font-weight: 600;
            display: flex;
            align-items: center;
            gap: 10px;
        }
        
        .chatbot-close {
            background: none;
            border: none;
            font-size: 18px;
            cursor: pointer;
            color: var(--text-light);
            transition: all 0.3s ease;
        }
        
        .chatbot-close:hover {
            color: var(--text-dark);
            transform: scale(1.2);
        }
        
        .chatbot-messages {
            flex: 1;
            padding: 15px;
            overflow-y: auto;
            display: flex;
            flex-direction: column;
            gap: 15px;
        }
        
        .chatbot-message {
            max-width: 80%;
            padding: 10px 15px;
            border-radius: 18px;
            font-size: 14px;
            line-height: 1.4;
        }
        
        .chatbot-message.bot {
            background: rgba(59, 130, 246, 0.1);
            color: var(--text-dark);
            align-self: flex-start;
            border-bottom-left-radius: 4px;
        }
        
        .chatbot-message.user {
            background: var(--primary-blue);
            color: white;
            align-self: flex-end;
            border-bottom-right-radius: 4px;
        }
        
        .chatbot-input-container {
            padding: 15px;
            border-top: 1px solid var(--border-light);
            display: flex;
            gap: 10px;
        }
        
        .chatbot-input {
            flex: 1;
            padding: 10px 15px;
            border: 1px solid var(--border-light);
            border-radius: 25px;
            background: var(--bg-secondary);
            color: var(--text-dark);
            font-size: 14px;
            outline: none;
        }
        
        .chatbot-send {
            background: var(--primary-blue);
            color: white;
            border: none;
            width: 40px;
            height: 40px;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            cursor: pointer;
            transition: all 0.3s ease;
        }
        
        .chatbot-send:hover {
            background: var(--secondary-blue);
            transform: scale(1.1);
        }
        
        .chatbot-trigger {
            position: fixed;
            bottom: 20px;
            right: 20px;
            width: 60px;
            height: 60px;
            border-radius: 50%;
            background: var(--primary-blue);
            color: white;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 24px;
            cursor: pointer;
            box-shadow: 0 4px 12px rgba(0, 0, 0, 0.15);
            transition: all 0.3s ease;
            z-index: 999;
        }
        
        .chatbot-trigger:hover {
            transform: scale(1.1);
            box-shadow: 0 6px 20px rgba(0, 0, 0, 0.2);
        }
        
        .chatbot-trigger.unread::after {
            content: '';
            position: absolute;
            top: 0;
            right: 0;
            width: 20px;
            height: 20px;
            border-radius: 50%;
            background: var(--accent-red);
            border: 2px solid white;
            animation: pulse 2s infinite;
        }
        
        /* Progress Bar Styles */
        .progress-container {
            width: 100%;
            height: 8px;
            background: rgba(0, 0, 0, 0.1);
            border-radius: 4px;
            overflow: hidden;
            margin: 20px 0;
        }
        
        [data-theme="dark"] .progress-container {
            background: rgba(255, 255, 255, 0.1);
        }
        
        .progress-bar {
            height: 100%;
            background: linear-gradient(90deg, var(--primary-blue), var(--accent-purple));
            border-radius: 4px;
            width: 0%;
            transition: width 1s ease;
        }
        
        .progress-text {
            display: flex;
            justify-content: space-between;
            margin-top: 5px;
            font-size: 12px;
            color: var(--text-light);
        }
        
        /* Rating Stars Styles */
        .rating-container {
            display: flex;
            align-items: center;
            gap: 5px;
            margin: 10px 0;
        }
        
        .rating-star {
            font-size: 20px;
            color: #ddd;
            cursor: pointer;
            transition: all 0.2s ease;
        }
        
        .rating-star:hover,
        .rating-star.active {
            color: #fbbf24;
            transform: scale(1.1);
        }
        
        .rating-value {
            margin-left: 10px;
            color: var(--text-light);
            font-size: 14px;
        }
        
        /* Form Styles */
        .form-group {
            margin-bottom: 20px;
        }
        
        .form-label {
            display: block;
            margin-bottom: 8px;
            color: var(--text-dark);
            font-weight: 500;
        }
        
        .form-input,
        .form-textarea,
        .form-select {
            width: 100%;
            padding: 12px 16px;
            border: 2px solid #e5e7eb;
            border-radius: 8px;
            font-size: 16px;
            transition: all 0.3s ease;
            background: var(--bg-light);
            color: var(--text-dark);
        }
        
        [data-theme="dark"] .form-input,
        [data-theme="dark"] .form-textarea,
        [data-theme="dark"] .form-select {
            background: var(--bg-secondary);
            border-color: #4b5563;
            color: var(--text-dark);
        }
        
        .form-input:focus,
        .form-textarea:focus,
        .form-select:focus {
            outline: none;
            border-color: var(--primary-blue);
            box-shadow: 0 0 0 3px rgba(59, 130, 246, 0.1);
        }
        
        .form-textarea {
            min-height: 120px;
            resize: vertical;
        }
        
        .form-checkbox,
        .form-radio {
            display: flex;
            align-items: center;
            gap: 8px;
            margin-bottom: 10px;
        }
        
        .form-checkbox input,
        .form-radio input {
            width: 18px;
            height: 18px;
            accent-color: var(--primary-blue);
        }
        
        .form-checkbox label,
        .form-radio label {
            color: var(--text-dark);
            font-size: 14px;
            cursor: pointer;
        }
        
        .form-error {
            color: var(--accent-red);
            font-size: 12px;
            margin-top: 5px;
        }
        
        /* Tooltip Styles */
        .tooltip {
            position: relative;
            display: inline-block;
        }
        
        .tooltip-content {
            position: absolute;
            bottom: 125%;
            left: 50%;
            transform: translateX(-50%);
            background: var(--card-bg);
            color: var(--text-dark);
            padding: 8px 12px;
            border-radius: 6px;
            font-size: 12px;
            white-space: nowrap;
            box-shadow: 0 4px 12px rgba(0, 0, 0, 0.15);
            opacity: 0;
            visibility: hidden;
            transition: all 0.3s ease;
            z-index: 1000;
        }
        
        .tooltip:hover .tooltip-content {
            opacity: 1;
            visibility: visible;
        }
        
        .tooltip-content::after {
            content: '';
            position: absolute;
            top: 100%;
            left: 50%;
            transform: translateX(-50%);
            border-width: 5px;
            border-style: solid;
            border-color: var(--card-bg) transparent transparent transparent;
        }
        
        /* Badge Styles */
        .badge {
            display: inline-block;
            padding: 4px 8px;
            border-radius: 12px;
            font-size: 12px;
            font-weight: 500;
        }
        
        .badge-primary {
            background: rgba(59, 130, 246, 0.1);
            color: var(--primary-blue);
        }
        
        .badge-success {
            background: rgba(16, 185, 129, 0.1);
            color: var(--accent-green);
        }
        
        .badge-warning {
            background: rgba(245, 158, 11, 0.1);
            color: var(--accent-orange);
        }
        
        .badge-danger {
            background: rgba(239, 68, 68, 0.1);
            color: var(--accent-red);
        }
        
        .badge-purple {
            background: rgba(139, 92, 246, 0.1);
            color: var(--accent-purple);
        }
        
        /* Avatar Styles */
        .avatar {
            width: 40px;
            height: 40px;
            border-radius: 50%;
            overflow: hidden;
            display: flex;
            align-items: center;
            justify-content: center;
            background: var(--primary-blue);
            color: white;
            font-weight: 600;
            font-size: 16px;
        }
        
        .avatar-sm {
            width: 30px;
            height: 30px;
            font-size: 12px;
        }
        
        .avatar-lg {
            width: 60px;
            height: 60px;
            font-size: 24px;
        }
        
        .avatar img {
            width: 100%;
            height: 100%;
            object-fit: cover;
        }
        
        /* Tag Styles */
        .tag {
            display: inline-block;
            padding: 4px 12px;
            border-radius: 20px;
            font-size: 12px;
            font-weight: 500;
            background: var(--bg-secondary);
            color: var(--text-dark);
            margin: 0 5px 5px 0;
            transition: all 0.3s ease;
        }
        
        .tag:hover {
            background: var(--primary-blue);
            color: white;
            transform: translateY(-2px);
        }
        
        /* Social Media Styles */
        .social-links {
            display: flex;
            gap: 10px;
            margin-top: 15px;
        }
        
        .social-link {
            width: 36px;
            height: 36px;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            color: white;
            transition: all 0.3s ease;
        }
        
        .social-link:hover {
            transform: translateY(-3px) scale(1.1);
        }
        
        .social-link.facebook {
            background: #1877f2;
        }
        
        .social-link.twitter {
            background: #1da1f2;
        }
        
        .social-link.instagram {
            background: linear-gradient(45deg, #f09433 0%, #e6683c 25%, #dc2743 50%, #cc2366 75%, #bc1888 100%);
        }
        
        .social-link.youtube {
            background: #ff0000;
        }
        
        .social-link.linkedin {
            background: #0077b5;
        }
        
        .social-link.whatsapp {
            background: #25d366;
        }
        
        /* Card Flip Styles */
        .card-flip {
            background-color: transparent;
            width: 100%;
            height: 300px;
            perspective: 1000px;
        }
        
        .card-flip-inner {
            position: relative;
            width: 100%;
            height: 100%;
            text-align: center;
            transition: transform 0.6s;
            transform-style: preserve-3d;
        }
        
        .card-flip:hover .card-flip-inner {
            transform: rotateY(180deg);
        }
        
        .card-flip-front, .card-flip-back {
            position: absolute;
            width: 100%;
            height: 100%;
            -webkit-backface-visibility: hidden;
            backface-visibility: hidden;
            border-radius: 16px;
            overflow: hidden;
        }
        
        .card-flip-back {
            transform: rotateY(180deg);
        }
        
        /* Parallax Styles */
        .parallax-container {
            height: 400px;
            overflow: hidden;
            position: relative;
            border-radius: 16px;
            margin: 20px 0;
        }
        
        .parallax-element {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 120%;
            background-size: cover;
            background-position: center;
            will-change: transform;
        }
        
        .parallax-content {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            display: flex;
            align-items: center;
            justify-content: center;
            background: rgba(0, 0, 0, 0.4);
            color: white;
            text-align: center;
            padding: 20px;
        }
        
        .parallax-title {
            font-size: 2.5rem;
            font-weight: 700;
            margin-bottom: 15px;
            text-shadow: 0 2px 10px rgba(0, 0, 0, 0.5);
        }
        
        .parallax-subtitle {
            font-size: 1.2rem;
            max-width: 600px;
            text-shadow: 0 1px 5px rgba(0, 0, 0, 0.5);
        }
        
        /* Masonry Grid Styles */
        .masonry-grid {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
            grid-auto-rows: 10px;
            gap: 20px;
            margin: 20px 0;
        }
        
        .masonry-item {
            grid-row-end: span 30;
            border-radius: 10px;
            overflow: hidden;
            box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
            transition: all 0.3s ease;
        }
        
        .masonry-item:hover {
            transform: translateY(-5px);
            box-shadow: 0 8px 24px rgba(0, 0, 0, 0.15);
        }
        
        .masonry-item img {
            width: 100%;
            height: 100%;
            object-fit: cover;
        }
        
        .masonry-item.tall {
            grid-row-end: span 45;
        }
        
        .masonry-item.wide {
            grid-column-end: span 2;
        }
        
        @media (max-width: 768px) {
            .masonry-item.wide {
                grid-column-end: span 1;
            }
        }
        
        /* Split Screen Styles */
        .split-screen {
            display: flex;
            height: 500px;
            border-radius: 16px;
            overflow: hidden;
            margin: 20px 0;
            box-shadow: 0 8px 32px rgba(0, 0, 0, 0.1);
        }
        
        .split-screen-left,
        .split-screen-right {
            flex: 1;
            padding: 30px;
            display: flex;
            flex-direction: column;
            justify-content: center;
        }
        
        .split-screen-left {
            background: var(--bg-light);
            color: var(--text-dark);
        }
        
        .split-screen-right {
            background: var(--primary-blue);
            color: white;
        }
        
        .split-screen-title {
            font-size: 2rem;
            font-weight: 700;
            margin-bottom: 20px;
        }
        
        .split-screen-desc {
            font-size: 1rem;
            line-height: 1.6;
            margin-bottom: 30px;
        }
        
        @media (max-width: 768px) {
            .split-screen {
                flex-direction: column;
                height: auto;
            }
            
            .split-screen-left,
            .split-screen-right {
                padding: 20px;
            }
            
            .split-screen-title {
                font-size: 1.5rem;
            }
        }
        
        /* Accordion Styles */
        .accordion {
            border-radius: 8px;
            overflow: hidden;
            margin: 20px 0;
            box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
        }
        
        .accordion-item {
            border-bottom: 1px solid var(--border-light);
        }
        
        .accordion-item:last-child {
            border-bottom: none;
        }
        
        .accordion-header {
            padding: 15px 20px;
            background: var(--bg-light);
            color: var(--text-dark);
            font-weight: 600;
            cursor: pointer;
            display: flex;
            align-items: center;
            justify-content: space-between;
            transition: all 0.3s ease;
        }
        
        [data-theme="dark"] .accordion-header {
            background: var(--bg-secondary);
        }
        
        .accordion-header:hover {
            background: rgba(59, 130, 246, 0.1);
        }
        
        .accordion-icon {
            transition: transform 0.3s ease;
        }
        
        .accordion-item.active .accordion-icon {
            transform: rotate(180deg);
        }
        
        .accordion-content {
            max-height: 0;
            overflow: hidden;
            transition: max-height 0.3s ease;
            background: var(--bg-light);
        }
        
        [data-theme="dark"] .accordion-content {
            background: var(--bg-secondary);
        }
        
        .accordion-body {
            padding: 20px;
            color: var(--text-light);
        }
        
        .accordion-item.active .accordion-content {
            max-height: 500px;
        }
        
        /* Tabs Styles */
        .tabs {
            margin: 20px 0;
        }
        
        .tabs-header {
            display: flex;
            border-bottom: 2px solid var(--border-light);
            margin-bottom: 20px;
        }
        
        .tabs-tab {
            padding: 10px 20px;
            font-weight: 600;
            color: var(--text-light);
            cursor: pointer;
            border-bottom: 2px solid transparent;
            margin-bottom: -2px;
            transition: all 0.3s ease;
        }
        
        .tabs-tab:hover {
            color: var(--primary-blue);
        }
        
        .tabs-tab.active {
            color: var(--primary-blue);
            border-bottom-color: var(--primary-blue);
        }
        
        .tabs-content {
            padding: 20px 0;
        }
        
        .tabs-pane {
            display: none;
        }
        
        .tabs-pane.active {
            display: block;
            animation: fadeIn 0.5s ease;
        }
        
        /* Modal Styles */
        .modal {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0, 0, 0, 0.7);
            display: flex;
            align-items: center;
            justify-content: center;
            z-index: 2000;
            opacity: 0;
            visibility: hidden;
            transition: all 0.3s ease;
        }
        
        .modal.show {
            opacity: 1;
            visibility: visible;
        }
        
        .modal-content {
            background: var(--card-bg);
            backdrop-filter: blur(10px);
            border-radius: 16px;
            max-width: 90%;
            max-height: 90vh;
            overflow: auto;
            box-shadow: 0 8px 32px rgba(0, 0, 0, 0.2);
            transform: scale(0.8);
            transition: transform 0.3s ease;
        }
        
        .modal.show .modal-content {
            transform: scale(1);
        }
        
        .modal-header {
            padding: 20px;
            border-bottom: 1px solid var(--border-light);
            display: flex;
            align-items: center;
            justify-content: space-between;
        }
        
        .modal-title {
            color: var(--text-dark);
            font-weight: 600;
            font-size: 1.5rem;
        }
        
        .modal-close {
            background: none;
            border: none;
            font-size: 24px;
            cursor: pointer;
            color: var(--text-light);
            transition: all 0.3s ease;
        }
        
        .modal-close:hover {
            color: var(--text-dark);
            transform: scale(1.2);
        }
        
        .modal-body {
            padding: 20px;
            color: var(--text-light);
        }
        
        .modal-footer {
            padding: 15px 20px;
            border-top: 1px solid var(--border-light);
            display: flex;
            justify-content: flex-end;
            gap: 10px;
        }
        
        /* Loading Skeleton Styles */
        .skeleton {
            background: linear-gradient(90deg, rgba(0, 0, 0, 0.06) 25%, rgba(0, 0, 0, 0.15) 37%, rgba(0, 0, 0, 0.06) 63%);
            background-size: 400% 100%;
            animation: skeleton-loading 1.4s ease infinite;
        }
        
        [data-theme="dark"] .skeleton {
            background: linear-gradient(90deg, rgba(255, 255, 255, 0.06) 25%, rgba(255, 255, 255, 0.15) 37%, rgba(255, 255, 255, 0.06) 63%);
        }
        
        @keyframes skeleton-loading {
            0% {
                background-position: 100% 50%;
            }
            100% {
                background-position: 0 50%;
            }
        }
        
        .skeleton-text {
            height: 16px;
            border-radius: 4px;
            margin-bottom: 8px;
        }
        
        .skeleton-text:last-child {
            width: 70%;
        }
        
        .skeleton-title {
            height: 24px;
            border-radius: 4px;
            margin-bottom: 16px;
            width: 60%;
        }
        
        .skeleton-avatar {
            width: 60px;
            height: 60px;
            border-radius: 50%;
            margin-bottom: 16px;
        }
        
        .skeleton-button {
            height: 40px;
            border-radius: 8px;
            width: 120px;
        }
        
        .skeleton-card {
            height: 200px;
            border-radius: 8px;
            margin-bottom: 16px;
        }
        
        /* Animation Keyframes */
        @keyframes fadeIn {
            from {
                opacity: 0;
                transform: translateY(20px);
            }
            to {
                opacity: 1;
                transform: translateY(0);
            }
        }
        
        @keyframes slideInLeft {
            from {
                opacity: 0;
                transform: translateX(-50px);
            }
            to {
                opacity: 1;
                transform: translateX(0);
            }
        }
        
        @keyframes slideInRight {
            from {
                opacity: 0;
                transform: translateX(50px);
            }
            to {
                opacity: 1;
                transform: translateX(0);
            }
        }
        
        @keyframes slideInUp {
            from {
                opacity: 0;
                transform: translateY(50px);
            }
            to {
                opacity: 1;
                transform: translateY(0);
            }
        }
        
        @keyframes slideInDown {
            from {
                opacity: 0;
                transform: translateY(-50px);
            }
            to {
                opacity: 1;
                transform: translateY(0);
            }
        }
        
        @keyframes zoomIn {
            from {
                opacity: 0;
                transform: scale(0.8);
            }
            to {
                opacity: 1;
                transform: scale(1);
            }
        }
        
        @keyframes zoomOut {
            from {
                opacity: 1;
                transform: scale(1);
            }
            to {
                opacity: 0;
                transform: scale(0.8);
            }
        }
        
        @keyframes rotateIn {
            from {
                opacity: 0;
                transform: rotate(-200deg);
            }
            to {
                opacity: 1;
                transform: rotate(0);
            }
        }
        
        @keyframes bounce {
            0%, 100% {
                transform: translateY(0);
            }
            50% {
                transform: translateY(-20px);
            }
        }
        
        @keyframes flash {
            0%, 100% {
                opacity: 1;
            }
            50% {
                opacity: 0;
            }
        }
        
        @keyframes pulse {
            0% {
                transform: scale(1);
            }
            50% {
                transform: scale(1.05);
            }
            100% {
                transform: scale(1);
            }
        }
        
        @keyframes rubberBand {
            0% {
                transform: scale(1);
            }
            30% {
                transform: scaleX(1.25) scaleY(0.75);
            }
            40% {
                transform: scaleX(0.75) scaleY(1.25);
            }
            60% {
                transform: scaleX(1.15) scaleY(0.85);
            }
            100% {
                transform: scale(1);
            }
        }
        
        @keyframes shake {
            0%, 100% {
                transform: translateX(0);
            }
            10%, 30%, 50%, 70%, 90% {
                transform: translateX(-10px);
            }
            20%, 40%, 60%, 80% {
                transform: translateX(10px);
            }
        }
        
        @keyframes swing {
            20% {
                transform: rotate(15deg);
            }
            40% {
                transform: rotate(-10deg);
            }
            60% {
                transform: rotate(5deg);
            }
            80% {
                transform: rotate(-5deg);
            }
            100% {
                transform: rotate(0deg);
            }
        }
        
        @keyframes tada {
            0% {
                transform: scale(1);
            }
            10%, 20% {
                transform: scale(0.9) rotate(-3deg);
            }
            30%, 50%, 70%, 90% {
                transform: scale(1.1) rotate(3deg);
            }
            40%, 60%, 80% {
                transform: scale(1.1) rotate(-3deg);
            }
            100% {
                transform: scale(1) rotate(0);
            }
        }
        
        @keyframes wobble {
            0% {
                transform: translateX(0%);
            }
            15% {
                transform: translateX(-25%) rotate(-5deg);
            }
            30% {
                transform: translateX(20%) rotate(3deg);
            }
            45% {
                transform: translateX(-15%) rotate(-3deg);
            }
            60% {
                transform: translateX(10%) rotate(2deg);
            }
            75% {
                transform: translateX(-5%) rotate(-1deg);
            }
            100% {
                transform: translateX(0%);
            }
        }
        
        @keyframes jello {
            0%, 100% {
                transform: scale(1, 1);
            }
            25% {
                transform: scale(0.9, 1.1);
            }
            50% {
                transform: scale(1.1, 0.9);
            }
            75% {
                transform: scale(0.95, 1.05);
            }
        }
        
        @keyframes heartBeat {
            0% {
                transform: scale(1);
            }
            14% {
                transform: scale(1.3);
            }
            28% {
                transform: scale(1);
            }
            42% {
                transform: scale(1.3);
            }
            70% {
                transform: scale(1);
            }
        }
        
        @keyframes flipInX {
            from {
                transform: perspective(400px) rotate3d(1, 0, 0, 90deg);
                opacity: 0;
            }
            40% {
                transform: perspective(400px) rotate3d(1, 0, 0, -20deg);
            }
            60% {
                transform: perspective(400px) rotate3d(1, 0, 0, 10deg);
                opacity: 1;
            }
            80% {
                transform: perspective(400px) rotate3d(1, 0, 0, -5deg);
            }
            to {
                transform: perspective(400px) rotate3d(1, 0, 0, 0);
            }
        }
        
        @keyframes flipInY {
            from {
                transform: perspective(400px) rotate3d(0, 1, 0, 90deg);
                opacity: 0;
            }
            40% {
                transform: perspective(400px) rotate3d(0, 1, 0, -20deg);
            }
            60% {
                transform: perspective(400px) rotate3d(0, 1, 0, 10deg);
                opacity: 1;
            }
            80% {
                transform: perspective(400px) rotate3d(0, 1, 0, -5deg);
            }
            to {
                transform: perspective(400px) rotate3d(0, 1, 0, 0);
            }
        }
        
        @keyframes flipOutX {
            from {
                transform: perspective(400px);
            }
            30% {
                transform: perspective(400px) rotate3d(1, 0, 0, -20deg);
                opacity: 1;
            }
            to {
                transform: perspective(400px) rotate3d(1, 0, 0, 90deg);
                opacity: 0;
            }
        }
        
        @keyframes flipOutY {
            from {
                transform: perspective(400px);
            }
            30% {
                transform: perspective(400px) rotate3d(0, 1, 0, -15deg);
                opacity: 1;
            }
            to {
                transform: perspective(400px) rotate3d(0, 1, 0, 90deg);
                opacity: 0;
            }
        }
        
        @keyframes lightSpeedIn {
            from {
                transform: translate3d(100%, 0, 0) skewX(-30deg);
                opacity: 0;
            }
            60% {
                transform: skewX(20deg);
                opacity: 1;
            }
            80% {
                transform: skewX(-5deg);
            }
            to {
                transform: translate3d(0, 0, 0);
            }
        }
        
        @keyframes lightSpeedOut {
            from {
                opacity: 1;
            }
            to {
                transform: translate3d(100%, 0, 0) skewX(30deg);
                opacity: 0;
            }
        }
        
        @keyframes rotateIn {
            from {
                transform-origin: center;
                transform: rotate3d(0, 0, 1, -200deg);
                opacity: 0;
            }
            to {
                transform-origin: center;
                transform: translate3d(0, 0, 0);
                opacity: 1;
            }
        }
        
        @keyframes rotateInDownLeft {
            from {
                transform-origin: left bottom;
                transform: rotate3d(0, 0, 1, -45deg);
                opacity: 0;
            }
            to {
                transform-origin: left bottom;
                transform: translate3d(0, 0, 0);
                opacity: 1;
            }
        }
        
        @keyframes rotateInDownRight {
            from {
                transform-origin: right bottom;
                transform: rotate3d(0, 0, 1, 45deg);
                opacity: 0;
            }
            to {
                transform-origin: right bottom;
                transform: translate3d(0, 0, 0);
                opacity: 1;
            }
        }
        
        @keyframes rotateInUpLeft {
            from {
                transform-origin: left bottom;
                transform: rotate3d(0, 0, 1, 45deg);
                opacity: 0;
            }
            to {
                transform-origin: left bottom;
                transform: translate3d(0, 0, 0);
                opacity: 1;
            }
        }
        
        @keyframes rotateInUpRight {
            from {
                transform-origin: right bottom;
                transform: rotate3d(0, 0, 1, -90deg);
                opacity: 0;
            }
            to {
                transform-origin: right bottom;
                transform: translate3d(0, 0, 0);
                opacity: 1;
            }
        }
        
        @keyframes rotateOut {
            from {
                transform-origin: center;
                opacity: 1;
            }
            to {
                transform-origin: center;
                transform: rotate3d(0, 0, 1, 200deg);
                opacity: 0;
            }
        }
        
        @keyframes rotateOutDownLeft {
            from {
                transform-origin: left bottom;
                opacity: 1;
            }
            to {
                transform-origin: left bottom;
                transform: rotate3d(0, 0, 1, 45deg);
                opacity: 0;
            }
        }
        
        @keyframes rotateOutDownRight {
            from {
                transform-origin: right bottom;
                opacity: 1;
            }
            to {
                transform-origin: right bottom;
                transform: rotate3d(0, 0, 1, -45deg);
                opacity: 0;
            }
        }
        
        @keyframes rotateOutUpLeft {
            from {
                transform-origin: left bottom;
                opacity: 1;
            }
            to {
                transform-origin: left bottom;
                transform: rotate3d(0, 0, 1, -45deg);
                opacity: 0;
            }
        }
        
        @keyframes rotateOutUpRight {
            from {
                transform-origin: right bottom;
                opacity: 1;
            }
            to {
                transform-origin: right bottom;
                transform: rotate3d(0, 0, 1, 90deg);
                opacity: 0;
            }
        }
        
        @keyframes hinge {
            0% {
                transform-origin: top left;
            }
            20%, 60% {
                transform: rotate3d(0, 0, 1, 80deg);
                transform-origin: top left;
            }
            40%, 80% {
                transform: rotate3d(0, 0, 1, 60deg);
                transform-origin: top left;
                opacity: 1;
            }
            to {
                transform: translate3d(0, 700px, 0);
                opacity: 0;
            }
        }
        
        @keyframes jackInTheBox {
            from {
                opacity: 0;
                transform: scale(0.1) rotate(30deg);
                transform-origin: center bottom;
            }
            50% {
                transform: rotate(-10deg);
            }
            70% {
                transform: rotate(3deg);
            }
            to {
                opacity: 1;
                transform: scale(1);
            }
        }
        
        @keyframes rollIn {
            from {
                opacity: 0;
                transform: translate3d(-100%, 0, 0) rotate3d(0, 0, 1, -120deg);
            }
            to {
                opacity: 1;
                transform: translate3d(0, 0, 0);
            }
        }
        
        @keyframes rollOut {
            from {
                opacity: 1;
            }
            to {
                opacity: 0;
                transform: translate3d(100%, 0, 0) rotate3d(0, 0, 1, 120deg);
            }
        }
        
        /* Utility Classes */
        .text-center { text-align: center; }
        .text-left { text-align: left; }
        .text-right { text-align: right; }
        .text-justify { text-align: justify; }
        
        .font-light { font-weight: 300; }
        .font-normal { font-weight: 400; }
        .font-medium { font-weight: 500; }
        .font-semibold { font-weight: 600; }
        .font-bold { font-weight: 700; }
        .font-extrabold { font-weight: 800; }
        .font-black { font-weight: 900; }
        
        .text-xs { font-size: 0.75rem; }
        .text-sm { font-size: 0.875rem; }
        .text-base { font-size: 1rem; }
        .text-lg { font-size: 1.125rem; }
        .text-xl { font-size: 1.25rem; }
        .text-2xl { font-size: 1.5rem; }
        .text-3xl { font-size: 1.875rem; }
        .text-4xl { font-size: 2.25rem; }
        .text-5xl { font-size: 3rem; }
        .text-6xl { font-size: 3.75rem; }
        
        .uppercase { text-transform: uppercase; }
        .lowercase { text-transform: lowercase; }
        .capitalize { text-transform: capitalize; }
        
        .italic { font-style: italic; }
        .not-italic { font-style: normal; }
        
        .underline { text-decoration: underline; }
        .line-through { text-decoration: line-through; }
        .no-underline { text-decoration: none; }
        
        .truncate {
            overflow: hidden;
            text-overflow: ellipsis;
            white-space: nowrap;
        }
        
        .break-words {
            word-wrap: break-word;
        }
        
        .break-all {
            word-break: break-all;
        }
        
        .whitespace-normal { white-space: normal; }
        .whitespace-nowrap { white-space: nowrap; }
        .whitespace-pre { white-space: pre; }
        .whitespace-pre-line { white-space: pre-line; }
        .whitespace-pre-wrap { white-space: pre-wrap; }
        
        .w-auto { width: auto; }
        .w-full { width: 100%; }
        .w-screen { width: 100vw; }
        .w-min { width: min-content; }
        .w-max { width: max-content; }
        .w-fit { width: fit-content; }
        
        .h-auto { height: auto; }
        .h-full { height: 100%; }
        .h-screen { height: 100vh; }
        .h-min { height: min-content; }
        .h-max { height: max-content; }
        .h-fit { height: fit-content; }
        
        .max-w-xs { max-width: 20rem; }
        .max-w-sm { max-width: 24rem; }
        .max-w-md { max-width: 28rem; }
        .max-w-lg { max-width: 32rem; }
        .max-w-xl { max-width: 36rem; }
        .max-w-2xl { max-width: 42rem; }
        .max-w-3xl { max-width: 48rem; }
        .max-w-4xl { max-width: 56rem; }
        .max-w-5xl { max-width: 64rem; }
        .max-w-6xl { max-width: 72rem; }
        .max-w-7xl { max-width: 80rem; }
        .max-w-full { max-width: 100%; }
        .max-w-screen-sm { max-width: 640px; }
        .max-w-screen-md { max-width: 768px; }
        .max-w-screen-lg { max-width: 1024px; }
        .max-w-screen-xl { max-width: 1280px; }
        .max-w-screen-2xl { max-width: 1536px; }
        
        .min-w-0 { min-width: 0; }
        .min-w-full { min-width: 100%; }
        .min-w-min { min-width: min-content; }
        .min-w-max { min-width: max-content; }
        .min-w-fit { min-width: fit-content; }
        
        .max-h-0 { max-height: 0; }
        .max-h-none { max-height: none; }
        .max-h-full { max-height: 100%; }
        .max-h-screen { max-height: 100vh; }
        .max-h-min { max-height: min-content; }
        .max-h-max { max-height: max-content; }
        .max-h-fit { max-height: fit-content; }
        
        .min-h-0 { min-height: 0; }
        .min-h-full { min-height: 100%; }
        .min-h-screen { min-height: 100vh; }
        .min-h-min { min-height: min-content; }
        .min-h-max { min-height: max-content; }
        .min-h-fit { min-height: fit-content; }
        
        .overflow-auto { overflow: auto; }
        .overflow-hidden { overflow: hidden; }
        .overflow-clip { overflow: clip; }
        .overflow-visible { overflow: visible; }
        .overflow-scroll { overflow: scroll; }
        .overflow-x-auto { overflow-x: auto; }
        .overflow-y-auto { overflow-y: auto; }
        .overflow-x-hidden { overflow-x: hidden; }
        .overflow-y-hidden { overflow-y: hidden; }
        .overflow-x-clip { overflow-x: clip; }
        .overflow-y-clip { overflow-y: clip; }
        .overflow-x-visible { overflow-x: visible; }
        .overflow-y-visible { overflow-y: visible; }
        .overflow-x-scroll { overflow-x: scroll; }
        .overflow-y-scroll { overflow-y: scroll; }
        
        .overscroll-auto { overscroll-behavior: auto; }
        .overscroll-contain { overscroll-behavior: contain; }
        .overscroll-none { overscroll-behavior: none; }
        .overscroll-y-auto { overscroll-behavior-y: auto; }
        .overscroll-y-contain { overscroll-behavior-y: contain; }
        .overscroll-y-none { overscroll-behavior-y: none; }
        .overscroll-x-auto { overscroll-behavior-x: auto; }
        .overscroll-x-contain { overscroll-behavior-x: contain; }
        .overscroll-x-none { overscroll-behavior-x: none; }
        
        .static { position: static; }
        .fixed { position: fixed; }
        .absolute { position: absolute; }
        .relative { position: relative; }
        .sticky { position: sticky; }
        
        .inset-0 { top: 0; right: 0; bottom: 0; left: 0; }
        .inset-x-0 { right: 0; left: 0; }
        .inset-y-0 { top: 0; bottom: 0; }
        
        .top-0 { top: 0; }
        .right-0 { right: 0; }
        .bottom-0 { bottom: 0; }
        .left-0 { left: 0; }
        
        .z-0 { z-index: 0; }
        .z-10 { z-index: 10; }
        .z-20 { z-index: 20; }
        .z-30 { z-index: 30; }
        .z-40 { z-index: 40; }
        .z-50 { z-index: 50; }
        .z-auto { z-index: auto; }
        
        .m-0 { margin: 0; }
        .m-1 { margin: 0.25rem; }
        .m-2 { margin: 0.5rem; }
        .m-3 { margin: 0.75rem; }
        .m-4 { margin: 1rem; }
        .m-5 { margin: 1.25rem; }
        .m-6 { margin: 1.5rem; }
        .m-8 { margin: 2rem; }
        .m-10 { margin: 2.5rem; }
        .m-12 { margin: 3rem; }
        .m-16 { margin: 4rem; }
        .m-20 { margin: 5rem; }
        .m-24 { margin: 6rem; }
        .m-32 { margin: 8rem; }
        .m-40 { margin: 10rem; }
        .m-48 { margin: 12rem; }
        .m-56 { margin: 14rem; }
        .m-64 { margin: 16rem; }
        .m-auto { margin: auto; }
        
        .mx-0 { margin-left: 0; margin-right: 0; }
        .mx-1 { margin-left: 0.25rem; margin-right: 0.25rem; }
        .mx-2 { margin-left: 0.5rem; margin-right: 0.5rem; }
        .mx-3 { margin-left: 0.75rem; margin-right: 0.75rem; }
        .mx-4 { margin-left: 1rem; margin-right: 1rem; }
        .mx-5 { margin-left: 1.25rem; margin-right: 1.25rem; }
        .mx-6 { margin-left: 1.5rem; margin-right: 1.5rem; }
        .mx-8 { margin-left: 2rem; margin-right: 2rem; }
        .mx-10 { margin-left: 2.5rem; margin-right: 2.5rem; }
        .mx-12 { margin-left: 3rem; margin-right: 3rem; }
        .mx-16 { margin-left: 4rem; margin-right: 4rem; }
        .mx-20 { margin-left: 5rem; margin-right: 5rem; }
        .mx-24 { margin-left: 6rem; margin-right: 6rem; }
        .mx-32 { margin-left: 8rem; margin-right: 8rem; }
        .mx-40 { margin-left: 10rem; margin-right: 10rem; }
        .mx-48 { margin-left: 12rem; margin-right: 12rem; }
        .mx-56 { margin-left: 14rem; margin-right: 14rem; }
        .mx-64 { margin-left: 16rem; margin-right: 16rem; }
        .mx-auto { margin-left: auto; margin-right: auto; }
        
        .my-0 { margin-top: 0; margin-bottom: 0; }
        .my-1 { margin-top: 0.25rem; margin-bottom: 0.25rem; }
        .my-2 { margin-top: 0.5rem; margin-bottom: 0.5rem; }
        .my-3 { margin-top: 0.75rem; margin-bottom: 0.75rem; }
        .my-4 { margin-top: 1rem; margin-bottom: 1rem; }
        .my-5 { margin-top: 1.25rem; margin-bottom: 1.25rem; }
        .my-6 { margin-top: 1.5rem; margin-bottom: 1.5rem; }
        .my-8 { margin-top: 2rem; margin-bottom: 2rem; }
        .my-10 { margin-top: 2.5rem; margin-bottom: 2.5rem; }
        .my-12 { margin-top: 3rem; margin-bottom: 3rem; }
        .my-16 { margin-top: 4rem; margin-bottom: 4rem; }
        .my-20 { margin-top: 5rem; margin-bottom: 5rem; }
        .my-24 { margin-top: 6rem; margin-bottom: 6rem; }
        .my-32 { margin-top: 8rem; margin-bottom: 8rem; }
        .my-40 { margin-top: 10rem; margin-bottom: 10rem; }
        .my-48 { margin-top: 12rem; margin-bottom: 12rem; }
        .my-56 { margin-top: 14rem; margin-bottom: 14rem; }
        .my-64 { margin-top: 16rem; margin-bottom: 16rem; }
        .my-auto { margin-top: auto; margin-bottom: auto; }
        
        .mt-0 { margin-top: 0; }
        .mt-1 { margin-top: 0.25rem; }
        .mt-2 { margin-top: 0.5rem; }
        .mt-3 { margin-top: 0.75rem; }
        .mt-4 { margin-top: 1rem; }
        .mt-5 { margin-top: 1.25rem; }
        .mt-6 { margin-top: 1.5rem; }
        .mt-8 { margin-top: 2rem; }
        .mt-10 { margin-top: 2.5rem; }
        .mt-12 { margin-top: 3rem; }
        .mt-16 { margin-top: 4rem; }
        .mt-20 { margin-top: 5rem; }
        .mt-24 { margin-top: 6rem; }
        .mt-32 { margin-top: 8rem; }
        .mt-40 { margin-top: 10rem; }
        .mt-48 { margin-top: 12rem; }
        .mt-56 { margin-top: 14rem; }
        .mt-64 { margin-top: 16rem; }
        .mt-auto { margin-top: auto; }
        
        .mr-0 { margin-right: 0; }
        .mr-1 { margin-right: 0.25rem; }
        .mr-2 { margin-right: 0.5rem; }
        .mr-3 { margin-right: 0.75rem; }
        .mr-4 { margin-right: 1rem; }
        .mr-5 { margin-right: 1.25rem; }
        .mr-6 { margin-right: 1.5rem; }
        .mr-8 { margin-right: 2rem; }
        .mr-10 { margin-right: 2.5rem; }
        .mr-12 { margin-right: 3rem; }
        .mr-16 { margin-right: 4rem; }
        .mr-20 { margin-right: 5rem; }
        .mr-24 { margin-right: 6rem; }
        .mr-32 { margin-right: 8rem; }
        .mr-40 { margin-right: 10rem; }
        .mr-48 { margin-right: 12rem; }
        .mr-56 { margin-right: 14rem; }
        .mr-64 { margin-right: 16rem; }
        .mr-auto { margin-right: auto; }
        
        .mb-0 { margin-bottom: 0; }
        .mb-1 { margin-bottom: 0.25rem; }
        .mb-2 { margin-bottom: 0.5rem; }
        .mb-3 { margin-bottom: 0.75rem; }
        .mb-4 { margin-bottom: 1rem; }
        .mb-5 { margin-bottom: 1.25rem; }
        .mb-6 { margin-bottom: 1.5rem; }
        .mb-8 { margin-bottom: 2rem; }
        .mb-10 { margin-bottom: 2.5rem; }
        .mb-12 { margin-bottom: 3rem; }
        .mb-16 { margin-bottom: 4rem; }
        .mb-20 { margin-bottom: 5rem; }
        .mb-24 { margin-bottom: 6rem; }
        .mb-32 { margin-bottom: 8rem; }
        .mb-40 { margin-bottom: 10rem; }
        .mb-48 { margin-bottom: 12rem; }
        .mb-56 { margin-bottom: 14rem; }
        .mb-64 { margin-bottom: 16rem; }
        .mb-auto { margin-bottom: auto; }
        
        .ml-0 { margin-left: 0; }
        .ml-1 { margin-left: 0.25rem; }
        .ml-2 { margin-left: 0.5rem; }
        .ml-3 { margin-left: 0.75rem; }
        .ml-4 { margin-left: 1rem; }
        .ml-5 { margin-left: 1.25rem; }
        .ml-6 { margin-left: 1.5rem; }
        .ml-8 { margin-left: 2rem; }
        .ml-10 { margin-left: 2.5rem; }
        .ml-12 { margin-left: 3rem; }
        .ml-16 { margin-left: 4rem; }
        .ml-20 { margin-left: 5rem; }
        .ml-24 { margin-left: 6rem; }
        .ml-32 { margin-left: 8rem; }
        .ml-40 { margin-left: 10rem; }
        .ml-48 { margin-left: 12rem; }
        .ml-56 { margin-left: 14rem; }
        .ml-64 { margin-left: 16rem; }
        .ml-auto { margin-left: auto; }
        
        .space-x-0 > * + * { margin-left: 0; }
        .space-x-1 > * + * { margin-left: 0.25rem; }
        .space-x-2 > * + * { margin-left: 0.5rem; }
        .space-x-3 > * + * { margin-left: 0.75rem; }
        .space-x-4 > * + * { margin-left: 1rem; }
        .space-x-5 > * + * { margin-left: 1.25rem; }
        .space-x-6 > * + * { margin-left: 1.5rem; }
        .space-x-8 > * + * { margin-left: 2rem; }
        .space-x-10 > * + * { margin-left: 2.5rem; }
        .space-x-12 > * + * { margin-left: 3rem; }
        .space-x-16 > * + * { margin-left: 4rem; }
        .space-x-20 > * + * { margin-left: 5rem; }
        .space-x-24 > * + * { margin-left: 6rem; }
        .space-x-32 > * + * { margin-left: 8rem; }
        .space-x-40 > * + * { margin-left: 10rem; }
        .space-x-48 > * + * { margin-left: 12rem; }
        .space-x-56 > * + * { margin-left: 14rem; }
        .space-x-64 > * + * { margin-left: 16rem; }
        
        .space-y-0 > * + * { margin-top: 0; }
        .space-y-1 > * + * { margin-top: 0.25rem; }
        .space-y-2 > * + * { margin-top: 0.5rem; }
        .space-y-3 > * + * { margin-top: 0.75rem; }
        .space-y-4 > * + * { margin-top: 1rem; }
        .space-y-5 > * + * { margin-top: 1.25rem; }
        .space-y-6 > * + * { margin-top: 1.5rem; }
        .space-y-8 > * + * { margin-top: 2rem; }
        .space-y-10 > * + * { margin-top: 2.5rem; }
        .space-y-12 > * + * { margin-top: 3rem; }
        .space-y-16 > * + * { margin-top: 4rem; }
        .space-y-20 > * + * { margin-top: 5rem; }
        .space-y-24 > * + * { margin-top: 6rem; }
        .space-y-32 > * + * { margin-top: 8rem; }
        .space-y-40 > * + * { margin-top: 10rem; }
        .space-y-48 > * + * { margin-top: 12rem; }
        .space-y-56 > * + * { margin-top: 14rem; }
        .space-y-64 > * + * { margin-top: 16rem; }
        
        .p-0 { padding: 0; }
        .p-1 { padding: 0.25rem; }
        .p-2 { padding: 0.5rem; }
        .p-3 { padding: 0.75rem; }
        .p-4 { padding: 1rem; }
        .p-5 { padding: 1.25rem; }
        .p-6 { padding: 1.5rem; }
        .p-8 { padding: 2rem; }
        .p-10 { padding: 2.5rem; }
        .p-12 { padding: 3rem; }
        .p-16 { padding: 4rem; }
        .p-20 { padding: 5rem; }
        .p-24 { padding: 6rem; }
        .p-32 { padding: 8rem; }
        .p-40 { padding: 10rem; }
        .p-48 { padding: 12rem; }
        .p-56 { padding: 14rem; }
        .p-64 { padding: 16rem; }
        
        .px-0 { padding-left: 0; padding-right: 0; }
        .px-1 { padding-left: 0.25rem; padding-right: 0.25rem; }
        .px-2 { padding-left: 0.5rem; padding-right: 0.5rem; }
        .px-3 { padding-left: 0.75rem; padding-right: 0.75rem; }
        .px-4 { padding-left: 1rem; padding-right: 1rem; }
        .px-5 { padding-left: 1.25rem; padding-right: 1.25rem; }
        .px-6 { padding-left: 1.5rem; padding-right: 1.5rem; }
        .px-8 { padding-left: 2rem; padding-right: 2rem; }
        .px-10 { padding-left: 2.5rem; padding-right: 2.5rem; }
        .px-12 { padding-left: 3rem; padding-right: 3rem; }
        .px-16 { padding-left: 4rem; padding-right: 4rem; }
        .px-20 { padding-left: 5rem; padding-right: 5rem; }
        .px-24 { padding-left: 6rem; padding-right: 6rem; }
        .px-32 { padding-left: 8rem; padding-right: 8rem; }
        .px-40 { padding-left: 10rem; padding-right: 10rem; }
        .px-48 { padding-left: 12rem; padding-right: 12rem; }
        .px-56 { padding-left: 14rem; padding-right: 14rem; }
        .px-64 { padding-left: 16rem; padding-right: 16rem; }
        
        .py-0 { padding-top: 0; padding-bottom: 0; }
        .py-1 { padding-top: 0.25rem; padding-bottom: 0.25rem; }
        .py-2 { padding-top: 0.5rem; padding-bottom: 0.5rem; }
        .py-3 { padding-top: 0.75rem; padding-bottom: 0.75rem; }
        .py-4 { padding-top: 1rem; padding-bottom: 1rem; }
        .py-5 { padding-top: 1.25rem; padding-bottom: 1.25rem; }
        .py-6 { padding-top: 1.5rem; padding-bottom: 1.5rem; }
        .py-8 { padding-top: 2rem; padding-bottom: 2rem; }
        .py-10 { padding-top: 2.5rem; padding-bottom: 2.5rem; }
        .py-12 { padding-top: 3rem; padding-bottom: 3rem; }
        .py-16 { padding-top: 4rem; padding-bottom: 4rem; }
        .py-20 { padding-top: 5rem; padding-bottom: 5rem; }
        .py-24 { padding-top: 6rem; padding-bottom: 6rem; }
        .py-32 { padding-top: 8rem; padding-bottom: 8rem; }
        .py-40 { padding-top: 10rem; padding-bottom: 10rem; }
        .py-48 { padding-top: 12rem; padding-bottom: 12rem; }
        .py-56 { padding-top: 14rem; padding-bottom: 14rem; }
        .py-64 { padding-top: 16rem; padding-bottom: 16rem; }
        
        .pt-0 { padding-top: 0; }
        .pt-1 { padding-top: 0.25rem; }
        .pt-2 { padding-top: 0.5rem; }
        .pt-3 { padding-top: 0.75rem; }
        .pt-4 { padding-top: 1rem; }
        .pt-5 { padding-top: 1.25rem; }
        .pt-6 { padding-top: 1.5rem; }
        .pt-8 { padding-top: 2rem; }
        .pt-10 { padding-top: 2.5rem; }
        .pt-12 { padding-top: 3rem; }
        .pt-16 { padding-top: 4rem; }
        .pt-20 { padding-top: 5rem; }
        .pt-24 { padding-top: 6rem; }
        .pt-32 { padding-top: 8rem; }
        .pt-40 { padding-top: 10rem; }
        .pt-48 { padding-top: 12rem; }
        .pt-56 { padding-top: 14rem; }
        .pt-64 { padding-top: 16rem; }
        
        .pr-0 { padding-right: 0; }
        .pr-1 { padding-right: 0.25rem; }
        .pr-2 { padding-right: 0.5rem; }
        .pr-3 { padding-right: 0.75rem; }
        .pr-4 { padding-right: 1rem; }
        .pr-5 { padding-right: 1.25rem; }
        .pr-6 { padding-right: 1.5rem; }
        .pr-8 { padding-right: 2rem; }
        .pr-10 { padding-right: 2.5rem; }
        .pr-12 { padding-right: 3rem; }
        .pr-16 { padding-right: 4rem; }
        .pr-20 { padding-right: 5rem; }
        .pr-24 { padding-right: 6rem; }
        .pr-32 { padding-right: 8rem; }
        .pr-40 { padding-right: 10rem; }
        .pr-48 { padding-right: 12rem; }
        .pr-56 { padding-right: 14rem; }
        .pr-64 { padding-right: 16rem; }
        
        .pb-0 { padding-bottom: 0; }
        .pb-1 { padding-bottom: 0.25rem; }
        .pb-2 { padding-bottom: 0.5rem; }
        .pb-3 { padding-bottom: 0.75rem; }
        .pb-4 { padding-bottom: 1rem; }
        .pb-5 { padding-bottom: 1.25rem; }
        .pb-6 { padding-bottom: 1.5rem; }
        .pb-8 { padding-bottom: 2rem; }
        .pb-10 { padding-bottom: 2.5rem; }
        .pb-12 { padding-bottom: 3rem; }
        .pb-16 { padding-bottom: 4rem; }
        .pb-20 { padding-bottom: 5rem; }
        .pb-24 { padding-bottom: 6rem; }
        .pb-32 { padding-bottom: 8rem; }
        .pb-40 { padding-bottom: 10rem; }
        .pb-48 { padding-bottom: 12rem; }
        .pb-56 { padding-bottom: 14rem; }
        .pb-64 { padding-bottom: 16rem; }
        
        .pl-0 { padding-left: 0; }
        .pl-1 { padding-left: 0.25rem; }
        .pl-2 { padding-left: 0.5rem; }
        .pl-3 { padding-left: 0.75rem; }
        .pl-4 { padding-left: 1rem; }
        .pl-5 { padding-left: 1.25rem; }
        .pl-6 { padding-left: 1.5rem; }
        .pl-8 { padding-left: 2rem; }
        .pl-10 { padding-left: 2.5rem; }
        .pl-12 { padding-left: 3rem; }
        .pl-16 { padding-left: 4rem; }
        .pl-20 { padding-left: 5rem; }
        .pl-24 { padding-left: 6rem; }
        .pl-32 { padding-left: 8rem; }
        .pl-40 { padding-left: 10rem; }
        .pl-48 { padding-left: 12rem; }
        .pl-56 { padding-left: 14rem; }
        .pl-64 { padding-left: 16rem; }
        
        .rounded-none { border-radius: 0; }
        .rounded-sm { border-radius: 0.125rem; }
        .rounded { border-radius: 0.25rem; }
        .rounded-md { border-radius: 0.375rem; }
        .rounded-lg { border-radius: 0.5rem; }
        .rounded-xl { border-radius: 0.75rem; }
        .rounded-2xl { border-radius: 1rem; }
        .rounded-3xl { border-radius: 1.5rem; }
        .rounded-full { border-radius: 9999px; }
        
        .rounded-t-none { border-top-left-radius: 0; border-top-right-radius: 0; }
        .rounded-t-sm { border-top-left-radius: 0.125rem; border-top-right-radius: 0.125rem; }
        .rounded-t { border-top-left-radius: 0.25rem; border-top-right-radius: 0.25rem; }
        .rounded-t-md { border-top-left-radius: 0.375rem; border-top-right-radius: 0.375rem; }
        .rounded-t-lg { border-top-left-radius: 0.5rem; border-top-right-radius: 0.5rem; }
        .rounded-t-xl { border-top-left-radius: 0.75rem; border-top-right-radius: 0.75rem; }
        .rounded-t-2xl { border-top-left-radius: 1rem; border-top-right-radius: 1rem; }
        .rounded-t-3xl { border-top-left-radius: 1.5rem; border-top-right-radius: 1.5rem; }
        .rounded-t-full { border-top-left-radius: 9999px; border-top-right-radius: 9999px; }
        
        .rounded-r-none { border-top-right-radius: 0; border-bottom-right-radius: 0; }
        .rounded-r-sm { border-top-right-radius: 0.125rem; border-bottom-right-radius: 0.125rem; }
        .rounded-r { border-top-right-radius: 0.25rem; border-bottom-right-radius: 0.25rem; }
        .rounded-r-md { border-top-right-radius: 0.375rem; border-bottom-right-radius: 0.375rem; }
        .rounded-r-lg { border-top-right-radius: 0.5rem; border-bottom-right-radius: 0.5rem; }
        .rounded-r-xl { border-top-right-radius: 0.75rem; border-bottom-right-radius: 0.75rem; }
        .rounded-r-2xl { border-top-right-radius: 1rem; border-bottom-right-radius: 1rem; }
        .rounded-r-3xl { border-top-right-radius: 1.5rem; border-bottom-right-radius: 1.5rem; }
        .rounded-r-full { border-top-right-radius: 9999px; border-bottom-right-radius: 9999px; }
        
        .rounded-b-none { border-bottom-left-radius: 0; border-bottom-right-radius: 0; }
        .rounded-b-sm { border-bottom-left-radius: 0.125rem; border-bottom-right-radius: 0.125rem; }
        .rounded-b { border-bottom-left-radius: 0.25rem; border-bottom-right-radius: 0.25rem; }
        .rounded-b-md { border-bottom-left-radius: 0.375rem; border-bottom-right-radius: 0.375rem; }
        .rounded-b-lg { border-bottom-left-radius: 0.5rem; border-bottom-right-radius: 0.5rem; }
        .rounded-b-xl { border-bottom-left-radius: 0.75rem; border-bottom-right-radius: 0.75rem; }
        .rounded-b-2xl { border-bottom-left-radius: 1rem; border-bottom-right-radius: 1rem; }
        .rounded-b-3xl { border-bottom-left-radius: 1.5rem; border-bottom-right-radius: 1.5rem; }
        .rounded-b-full { border-bottom-left-radius: 9999px; border-bottom-right-radius: 9999px; }
        
        .rounded-l-none { border-top-left-radius: 0; border-bottom-left-radius: 0; }
        .rounded-l-sm { border-top-left-radius: 0.125rem; border-bottom-left-radius: 0.125rem; }
        .rounded-l { border-top-left-radius: 0.25rem; border-bottom-left-radius: 0.25rem; }
        .rounded-l-md { border-top-left-radius: 0.375rem; border-bottom-left-radius: 0.375rem; }
        .rounded-l-lg { border-top-left-radius: 0.5rem; border-bottom-left-radius: 0.5rem; }
        .rounded-l-xl { border-top-left-radius: 0.75rem; border-bottom-left-radius: 0.75rem; }
        .rounded-l-2xl { border-top-left-radius: 1rem; border-bottom-left-radius: 1rem; }
        .rounded-l-3xl { border-top-left-radius: 1.5rem; border-bottom-left-radius: 1.5rem; }
        .rounded-l-full { border-top-left-radius: 9999px; border-bottom-left-radius: 9999px; }
        
        .rounded-tl-none { border-top-left-radius: 0; }
        .rounded-tl-sm { border-top-left-radius: 0.125rem; }
        .rounded-tl { border-top-left-radius: 0.25rem; }
        .rounded-tl-md { border-top-left-radius: 0.375rem; }
        .rounded-tl-lg { border-top-left-radius: 0.5rem; }
        .rounded-tl-xl { border-top-left-radius: 0.75rem; }
        .rounded-tl-2xl { border-top-left-radius: 1rem; }
        .rounded-tl-3xl { border-top-left-radius: 1.5rem; }
        .rounded-tl-full { border-top-left-radius: 9999px; }
        
        .rounded-tr-none { border-top-right-radius: 0; }
        .rounded-tr-sm { border-top-right-radius: 0.125rem; }
        .rounded-tr { border-top-right-radius: 0.25rem; }
        .rounded-tr-md { border-top-right-radius: 0.375rem; }
        .rounded-tr-lg { border-top-right-radius: 0.5rem; }
        .rounded-tr-xl { border-top-right-radius: 0.75rem; }
        .rounded-tr-2xl { border-top-right-radius: 1rem; }
        .rounded-tr-3xl { border-top-right-radius: 1.5rem; }
        .rounded-tr-full { border-top-right-radius: 9999px; }
        
        .rounded-bl-none { border-bottom-left-radius: 0; }
        .rounded-bl-sm { border-bottom-left-radius: 0.125rem; }
        .rounded-bl { border-bottom-left-radius: 0.25rem; }
        .rounded-bl-md { border-bottom-left-radius: 0.375rem; }
        .rounded-bl-lg { border-bottom-left-radius: 0.5rem; }
        .rounded-bl-xl { border-bottom-left-radius: 0.75rem; }
        .rounded-bl-2xl { border-bottom-left-radius: 1rem; }
        .rounded-bl-3xl { border-bottom-left-radius: 1.5rem; }
        .rounded-bl-full { border-bottom-left-radius: 9999px; }
        
        .rounded-br-none { border-bottom-right-radius: 0; }
        .rounded-br-sm { border-bottom-right-radius: 0.125rem; }
        .rounded-br { border-bottom-right-radius: 0.25rem; }
        .rounded-br-md { border-bottom-right-radius: 0.375rem; }
        .rounded-br-lg { border-bottom-right-radius: 0.5rem; }
        .rounded-br-xl { border-bottom-right-radius: 0.75rem; }
        .rounded-br-2xl { border-bottom-right-radius: 1rem; }
        .rounded-br-3xl { border-bottom-right-radius: 1.5rem; }
        .rounded-br-full { border-bottom-right-radius: 9999px; }
        
        .border-0 { border-width: 0; }
        .border { border-width: 1px; }
        .border-2 { border-width: 2px; }
        .border-4 { border-width: 4px; }
        .border-8 { border-width: 8px; }
        
        .border-t-0 { border-top-width: 0; }
        .border-t { border-top-width: 1px; }
        .border-t-2 { border-top-width: 2px; }
        .border-t-4 { border-top-width: 4px; }
        .border-t-8 { border-top-width: 8px; }
        
        .border-r-0 { border-right-width: 0; }
        .border-r { border-right-width: 1px; }
        .border-r-2 { border-right-width: 2px; }
        .border-r-4 { border-right-width: 4px; }
        .border-r-8 { border-right-width: 8px; }
        
        .border-b-0 { border-bottom-width: 0; }
        .border-b { border-bottom-width: 1px; }
        .border-b-2 { border-bottom-width: 2px; }
        .border-b-4 { border-bottom-width: 4px; }
        .border-b-8 { border-bottom-width: 8px; }
        
        .border-l-0 { border-left-width: 0; }
        .border-l { border-left-width: 1px; }
        .border-l-2 { border-left-width: 2px; }
        .border-l-4 { border-left-width: 4px; }
        .border-l-8 { border-left-width: 8px; }
        
        .border-solid { border-style: solid; }
        .border-dashed { border-style: dashed; }
        .border-dotted { border-style: dotted; }
        .border-double { border-style: double; }
        .border-hidden { border-style: hidden; }
        .border-none { border-style: none; }
        
        .border-transparent { border-color: transparent; }
        .border-current { border-color: currentColor; }
        .border-black { border-color: #000; }
        .border-white { border-color: #fff; }
        .border-gray-50 { border-color: #f9fafb; }
        .border-gray-100 { border-color: #f3f4f6; }
        .border-gray-200 { border-color: #e5e7eb; }
        .border-gray-300 { border-color: #d1d5db; }
        .border-gray-400 { border-color: #9ca3af; }
        .border-gray-500 { border-color: #6b7280; }
        .border-gray-600 { border-color: #4b5563; }
        .border-gray-700 { border-color: #374151; }
        .border-gray-800 { border-color: #1f2937; }
        .border-gray-900 { border-color: #111827; }
        
        .float-left { float: left; }
        .float-right { float: right; }
        .float-none { float: none; }
        
        .clear-left { clear: left; }
        .clear-right { clear: right; }
        .clear-both { clear: both; }
        .clear-none { clear: none; }
        
        .block { display: block; }
        .inline-block { display: inline-block; }
        .inline { display: inline; }
        .flex { display: flex; }
        .inline-flex { display: inline-flex; }
        .grid { display: grid; }
        .inline-grid { display: inline-grid; }
        .table { display: table; }
        .inline-table { display: inline-table; }
        .table-caption { display: table-caption; }
        .table-cell { display: table-cell; }
        .table-column { display: table-column; }
        .table-column-group { display: table-column-group; }
        .table-footer-group { display: table-footer-group; }
        .table-header-group { display: table-header-group; }
        .table-row { display: table-row; }
        .table-row-group { display: table-row-group; }
        .flow-root { display: flow-root; }
        .hidden { display: none; }
        
        .appearance-none { appearance: none; }
        
        .bg-fixed { background-attachment: fixed; }
        .bg-local { background-attachment: local; }
        .bg-scroll { background-attachment: scroll; }
        
        .bg-clip-border { background-clip: border-box; }
        .bg-clip-padding { background-clip: padding-box; }
        .bg-clip-content { background-clip: content-box; }
        .bg-clip-text { background-clip: text; }
        
        .bg-repeat { background-repeat: repeat; }
        .bg-no-repeat { background-repeat: no-repeat; }
        .bg-repeat-x { background-repeat: repeat-x; }
        .bg-repeat-y { background-repeat: repeat-y; }
        .bg-repeat-round { background-repeat: round; }
        .bg-repeat-space { background-repeat: space; }
        
        .bg-auto { background-size: auto; }
        .bg-cover { background-size: cover; }
        .bg-contain { background-size: contain; }
        
        .border-collapse { border-collapse: collapse; }
        .border-separate { border-collapse: separate; }
        .border-spacing-0 { border-spacing: 0; }
        
        .table-auto { table-layout: auto; }
        .table-fixed { table-layout: fixed; }
        
        .transform { transform: translate(var(--tw-translate-x), var(--tw-translate-y)) rotate(var(--tw-rotate)) skewX(var(--tw-skew-x)) skewY(var(--tw-skew-y)) scaleX(var(--tw-scale-x)) scaleY(var(--tw-scale-y)); }
        .transform-gpu { transform: translate3d(var(--tw-translate-x), var(--tw-translate-y), 0) rotate(var(--tw-rotate)) skewX(var(--tw-skew-x)) skewY(var(--tw-skew-y)) scaleX(var(--tw-scale-x)) scaleY(var(--tw-scale-y)); }
        .transform-none { transform: none; }
        
        .origin-center { transform-origin: center; }
        .origin-top { transform-origin: top; }
        .origin-top-right { transform-origin: top right; }
        .origin-right { transform-origin: right; }
        .origin-bottom-right { transform-origin: bottom right; }
        .origin-bottom { transform-origin: bottom; }
        .origin-bottom-left { transform-origin: bottom left; }
        .origin-left { transform-origin: left; }
        .origin-top-left { transform-origin: top left; }
        
        .scale-0 { --tw-scale-x: 0; --tw-scale-y: 0; transform: translate(var(--tw-translate-x), var(--tw-translate-y)) rotate(var(--tw-rotate)) skewX(var(--tw-skew-x)) skewY(var(--tw-skew-y)) scaleX(var(--tw-scale-x)) scaleY(var(--tw-scale-y)); }
        .scale-50 { --tw-scale-x: 0.5; --tw-scale-y: 0.5; transform: translate(var(--tw-translate-x), var(--tw-translate-y)) rotate(var(--tw-rotate)) skewX(var(--tw-skew-x)) skewY(var(--tw-skew-y)) scaleX(var(--tw-scale-x)) scaleY(var(--tw-scale-y)); }
        .scale-75 { --tw-scale-x: 0.75; --tw-scale-y: 0.75; transform: translate(var(--tw-translate-x), var(--tw-translate-y)) rotate(var(--tw-rotate)) skewX(var(--tw-skew-x)) skewY(var(--tw-skew-y)) scaleX(var(--tw-scale-x)) scaleY(var(--tw-scale-y)); }
        .scale-90 { --tw-scale-x: 0.9; --tw-scale-y: 0.9; transform: translate(var(--tw-translate-x), var(--tw-translate-y)) rotate(var(--tw-rotate)) skewX(var(--tw-skew-x)) skewY(var(--tw-skew-y)) scaleX(var(--tw-scale-x)) scaleY(var(--tw-scale-y)); }
        .scale-95 { --tw-scale-x: 0.95; --tw-scale-y: 0.95; transform: translate(var(--tw-translate-x), var(--tw-translate-y)) rotate(var(--tw-rotate)) skewX(var(--tw-skew-x)) skewY(var(--tw-skew-y)) scaleX(var(--tw-scale-x)) scaleY(var(--tw-scale-y)); }
        .scale-100 { --tw-scale-x: 1; --tw-scale-y: 1; transform: translate(var(--tw-translate-x), var(--tw-translate-y)) rotate(var(--tw-rotate)) skewX(var(--tw-skew-x)) skewY(var(--tw-skew-y)) scaleX(var(--tw-scale-x)) scaleY(var(--tw-scale-y)); }
        .scale-105 { --tw-scale-x: 1.05; --tw-scale-y: 1.05; transform: translate(var(--tw-translate-x), var(--tw-translate-y)) rotate(var(--tw-rotate)) skewX(var(--tw-skew-x)) skewY(var(--tw-skew-y)) scaleX(var(--tw-scale-x)) scaleY(var(--tw-scale-y)); }
        .scale-110 { --tw-scale-x: 1.1; --tw-scale-y: 1.1; transform: translate(var(--tw-translate-x), var(--tw-translate-y)) rotate(var(--tw-rotate)) skewX(var(--tw-skew-x)) skewY(var(--tw-skew-y)) scaleX(var(--tw-scale-x)) scaleY(var(--tw-scale-y)); }
        .scale-125 { --tw-scale-x: 1.25; --tw-scale-y: 1.25; transform: translate(var(--tw-translate-x), var(--tw-translate-y)) rotate(var(--tw-rotate)) skewX(var(--tw-skew-x)) skewY(var(--tw-skew-y)) scaleX(var(--tw-scale-x)) scaleY(var(--tw-scale-y)); }
        .scale-150 { --tw-scale-x: 1.5; --tw-scale-y: 1.5; transform: translate(var(--tw-translate-x), var(--tw-translate-y)) rotate(var(--tw-rotate)) skewX(var(--tw-skew-x)) skewY(var(--tw-skew-y)) scaleX(var(--tw-scale-x)) scaleY(var(--tw-scale-y)); }
        
        .scale-x-0 { --tw-scale-x: 0; transform: translate(var(--tw-translate-x), var(--tw-translate-y)) rotate(var(--tw-rotate)) skewX(var(--tw-skew-x)) skewY(var(--tw-skew-y)) scaleX(var(--tw-scale-x)) scaleY(var(--tw-scale-y)); }
        .scale-x-50 { --tw-scale-x: 0.5; transform: translate(var(--tw-translate-x), var(--tw-translate-y)) rotate(var(--tw-rotate)) skewX(var(--tw-skew-x)) skewY(var(--tw-skew-y)) scaleX(var(--tw-scale-x)) scaleY(var(--tw-scale-y)); }
        .scale-x-75 { --tw-scale-x: 0.75; transform: translate(var(--tw-translate-x), var(--tw-translate-y)) rotate(var(--tw-rotate)) skewX(var(--tw-skew-x)) skewY(var(--tw-skew-y)) scaleX(var(--tw-scale-x)) scaleY(var(--tw-scale-y)); }
        .scale-x-90 { --tw-scale-x: 0.9; transform: translate(var(--tw-translate-x), var(--tw-translate-y)) rotate(var(--tw-rotate)) skewX(var(--tw-skew-x)) skewY(var(--tw-skew-y)) scaleX(var(--tw-scale-x)) scaleY(var(--tw-scale-y)); }
        .scale-x-95 { --tw-scale-x: 0.95; transform: translate(var(--tw-translate-x), var(--tw-translate-y)) rotate(var(--tw-rotate)) skewX(var(--tw-skew-x)) skewY(var(--tw-skew-y)) scaleX(var(--tw-scale-x)) scaleY(var(--tw-scale-y)); }
        .scale-x-100 { --tw-scale-x: 1; transform: translate(var(--tw-translate-x), var(--tw-translate-y)) rotate(var(--tw-rotate)) skewX(var(--tw-skew-x)) skewY(var(--tw-skew-y)) scaleX(var(--tw-scale-x)) scaleY(var(--tw-scale-y)); }
        .scale-x-105 { --tw-scale-x: 1.05; transform: translate(var(--tw-translate-x), var(--tw-translate-y)) rotate(var(--tw-rotate)) skewX(var(--tw-skew-x)) skewY(var(--tw-skew-y)) scaleX(var(--tw-scale-x)) scaleY(var(--tw-scale-y)); }
        .scale-x-110 { --tw-scale-x: 1.1; transform: translate(var(--tw-translate-x), var(--tw-translate-y)) rotate(var(--tw-rotate)) skewX(var(--tw-skew-x)) skewY(var(--tw-skew-y)) scaleX(var(--tw-scale-x)) scaleY(var(--tw-scale-y)); }
        .scale-x-125 { --tw-scale-x: 1.25; transform: translate(var(--tw-translate-x), var(--tw-translate-y)) rotate(var(--tw-rotate)) skewX(var(--tw-skew-x)) skewY(var(--tw-skew-y)) scaleX(var(--tw-scale-x)) scaleY(var(--tw-scale-y)); }
        .scale-x-150 { --tw-scale-x: 1.5; transform: translate(var(--tw-translate-x), var(--tw-translate-y)) rotate(var(--tw-rotate)) skewX(var(--tw-skew-x)) skewY(var(--tw-skew-y)) scaleX(var(--tw-scale-x)) scaleY(var(--tw-scale-y)); }
        
        .scale-y-0 { --tw-scale-y: 0; transform: translate(var(--tw-translate-x), var(--tw-translate-y)) rotate(var(--tw-rotate)) skewX(var(--tw-skew-x)) skewY(var(--tw-skew-y)) scaleX(var(--tw-scale-x)) scaleY(var(--tw-scale-y)); }
        .scale-y-50 { --tw-scale-y: 0.5; transform: translate(var(--tw-translate-x), var(--tw-translate-y)) rotate(var(--tw-rotate)) skewX(var(--tw-skew-x)) skewY(var(--tw-skew-y)) scaleX(var(--tw-scale-x)) scaleY(var(--tw-scale-y)); }
        .scale-y-75 { --tw-scale-y: 0.75; transform: translate(var(--tw-translate-x), var(--tw-translate-y)) rotate(var(--tw-rotate)) skewX(var(--tw-skew-x)) skewY(var(--tw-skew-y)) scaleX(var(--tw-scale-x)) scaleY(var(--tw-scale-y)); }
        .scale-y-90 { --tw-scale-y: 0.9; transform: translate(var(--tw-translate-x), var(--tw-translate-y)) rotate(var(--tw-rotate)) skewX(var(--tw-skew-x)) skewY(var(--tw-skew-y)) scaleX(var(--tw-scale-x)) scaleY(var(--tw-scale-y)); }
        .scale-y-95 { --tw-scale-y: 0.95; transform: translate(var(--tw-translate-x), var(--tw-translate-y)) rotate(var(--tw-rotate)) skewX(var(--tw-skew-x)) skewY(var(--tw-skew-y)) scaleX(var(--tw-scale-x)) scaleY(var(--tw-scale-y)); }
        .scale-y-100 { --tw-scale-y: 1; transform: translate(var(--tw-translate-x), var(--tw-translate-y)) rotate(var(--tw-rotate)) skewX(var(--tw-skew-x)) skewY(var(--tw-skew-y)) scaleX(var(--tw-scale-x)) scaleY(var(--tw-scale-y)); }
        .scale-y-105 { --tw-scale-y: 1.05; transform: translate(var(--tw-translate-x), var(--tw-translate-y)) rotate(var(--tw-rotate)) skewX(var(--tw-skew-x)) skewY(var(--tw-skew-y)) scaleX(var(--tw-scale-x)) scaleY(var(--tw-scale-y)); }
        .scale-y-110 { --tw-scale-y: 1.1; transform: translate(var(--tw-translate-x), var(--tw-translate-y)) rotate(var(--tw-rotate)) skewX(var(--tw-skew-x)) skewY(var(--tw-skew-y)) scaleX(var(--tw-scale-x)) scaleY(var(--tw-scale-y)); }
        .scale-y-125 { --tw-scale-y: 1.25; transform: translate(var(--tw-translate-x), var(--tw-translate-y)) rotate(var(--tw-rotate)) skewX(var(--tw-skew-x)) skewY(var(--tw-skew-y)) scaleX(var(--tw-scale-x)) scaleY(var(--tw-scale-y)); }
        .scale-y-150 { --tw-scale-y: 1.5; transform: translate(var(--tw-translate-x), var(--tw-translate-y)) rotate(var(--tw-rotate)) skewX(var(--tw-skew-x)) skewY(var(--tw-skew-y)) scaleX(var(--tw-scale-x)) scaleY(var(--tw-scale-y)); }
        
        .rotate-0 { --tw-rotate: 0deg; transform: translate(var(--tw-translate-x), var(--tw-translate-y)) rotate(var(--tw-rotate)) skewX(var(--tw-skew-x)) skewY(var(--tw-skew-y)) scaleX(var(--tw-scale-x)) scaleY(var(--tw-scale-y)); }
        .rotate-1 { --tw-rotate: 1deg; transform: translate(var(--tw-translate-x), var(--tw-translate-y)) rotate(var(--tw-rotate)) skewX(var(--tw-skew-x)) skewY(var(--tw-skew-y)) scaleX(var(--tw-scale-x)) scaleY(var(--tw-scale-y)); }
        .rotate-2 { --tw-rotate: 2deg; transform: translate(var(--tw-translate-x), var(--tw-translate-y)) rotate(var(--tw-rotate)) skewX(var(--tw-skew-x)) skewY(var(--tw-skew-y)) scaleX
