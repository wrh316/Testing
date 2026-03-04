<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Class 10's Seat Random Number Program</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        /* 样式完全不变，保持原样 */
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }
        
        :root {
            --primary: #4361ee;
            --secondary: #3a0ca3;
            --accent: #4cc9f0;
            --success: #4ade80;
            --warning: #f59e0b;
            --danger: #ef4444;
            --light: #f8fafc;
            --dark: #1e293b;
            --gray: #64748b;
            --bg-gradient: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            --card-shadow: 0 10px 30px rgba(0, 0, 0, 0.1);
            --hover-shadow: 0 15px 40px rgba(0, 0, 0, 0.15);
        }
        
        .dark-theme {
            --primary: #5b6bf0;
            --secondary: #4c1d95;
            --accent: #60d9f0;
            --success: #5ae68a;
            --warning: #f7b955;
            --danger: #f87171;
            --light: #1e293b;
            --dark: #f1f5f9;
            --gray: #94a3b8;
            --bg-gradient: linear-gradient(135deg, #1e293b 0%, #334155 100%);
            --card-shadow: 0 10px 30px rgba(0, 0, 0, 0.3);
            --hover-shadow: 0 15px 40px rgba(0, 0, 0, 0.4);
        }
        
        body {
            font-family: 'Segoe UI', 'Microsoft YaHei', sans-serif;
            background: var(--bg-gradient);
            color: var(--dark);
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            padding: 20px;
            line-height: 1.6;
            transition: all 0.3s ease;
        }
        
        .container {
            background: rgba(255, 255, 255, 0.95);
            backdrop-filter: blur(20px);
            border-radius: 24px;
            box-shadow: 
                0 25px 50px -12px rgba(0, 0, 0, 0.25),
                inset 0 1px 0 rgba(255, 255, 255, 0.3);
            padding: 40px;
            max-width: 1200px;
            width: 100%;
            border: 1px solid rgba(255, 255, 255, 0.2);
            position: relative;
            overflow: hidden;
            animation: fadeIn 0.8s ease-out;
            transition: all 0.3s ease;
        }
        
        .dark-theme .container {
            background: rgba(30, 41, 59, 0.95);
            border: 1px solid rgba(255, 255, 255, 0.1);
        }
        
        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(20px); }
            to { opacity: 1; transform: translateY(0); }
        }
        
        .container::before {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            right: 0;
            height: 4px;
            background: linear-gradient(90deg, 
                var(--primary), 
                var(--accent), 
                var(--success), 
                var(--warning));
            border-radius: 24px 24px 0 0;
        }
        
        header {
            text-align: center;
            margin-bottom: 40px;
            position: relative;
        }
        
        .logo {
            width: 80px;
            height: 80px;
            background: linear-gradient(135deg, var(--primary), var(--secondary));
            border-radius: 50%;
            margin: 0 auto 20px;
            display: flex;
            align-items: center;
            justify-content: center;
            box-shadow: 0 8px 25px rgba(67, 97, 238, 0.3);
            animation: floating 3s ease-in-out infinite;
        }
        
        @keyframes floating {
            0%, 100% { transform: translateY(0); }
            50% { transform: translateY(-10px); }
        }
        
        .logo i {
            font-size: 2rem;
            color: white;
        }
        
        h1 {
            color: var(--dark);
            font-size: 2.8rem;
            font-weight: 700;
            margin-bottom: 10px;
            background: linear-gradient(135deg, var(--primary), var(--secondary));
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            text-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
        }
        
        .subtitle {
            color: var(--gray);
            font-size: 1.2rem;
            font-weight: 400;
        }
        
        .control-panel {
            display: flex;
            justify-content: center;
            gap: 20px;
            margin-bottom: 30px;
            flex-wrap: wrap;
        }
        
        .save-history-option {
            display: flex;
            justify-content: center;
            align-items: center;
            margin-bottom: 20px;
            background: var(--light);
            border-radius: 12px;
            padding: 15px;
            box-shadow: var(--card-shadow);
            border: 1px solid rgba(255, 255, 255, 0.5);
            transition: all 0.3s ease;
        }
        
        .dark-theme .save-history-option {
            background: #1e293b;
            border: 1px solid rgba(255, 255, 255, 0.1);
        }
        
        .save-history-option:hover {
            transform: translateY(-3px);
            box-shadow: var(--hover-shadow);
        }
        
        .save-history-label {
            font-weight: 500;
            color: var(--dark);
            margin-right: 15px;
            display: flex;
            align-items: center;
            gap: 10px;
        }
        
        .save-history-label i {
            color: var(--primary);
        }
        
        .toggle-switch {
            position: relative;
            display: inline-block;
            width: 50px;
            height: 24px;
        }
        
        .toggle-switch input {
            opacity: 0;
            width: 0;
            height: 0;
        }
        
        .toggle-slider {
            position: absolute;
            cursor: pointer;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background-color: #ccc;
            transition: .4s;
            border-radius: 24px;
        }
        
        .toggle-slider:before {
            position: absolute;
            content: "";
            height: 16px;
            width: 16px;
            left: 4px;
            bottom: 4px;
            background-color: white;
            transition: .4s;
            border-radius: 50%;
        }
        
        input:checked + .toggle-slider {
            background-color: var(--primary);
        }
        
        input:checked + .toggle-slider:before {
            transform: translateX(26px);
        }
        
        .btn {
            padding: 16px 32px;
            border: none;
            border-radius: 50px;
            font-size: 1.1rem;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
            display: flex;
            align-items: center;
            gap: 12px;
            box-shadow: var(--card-shadow);
            position: relative;
            overflow: hidden;
        }
        
        .btn::before {
            content: '';
            position: absolute;
            top: 0;
            left: -100%;
            width: 100%;
            height: 100%;
            background: linear-gradient(90deg, transparent, rgba(255, 255, 255, 0.4), transparent);
            transition: 0.5s;
        }
        
        .btn:hover::before {
            left: 100%;
        }
        
        .btn-primary {
            background: linear-gradient(135deg, var(--primary), var(--secondary));
            color: white;
        }
        
        .btn-secondary {
            background: linear-gradient(135deg, var(--gray), #475569);
            color: white;
        }
        
        .btn:hover {
            transform: translateY(-3px);
            box-shadow: var(--hover-shadow);
        }
        
        .btn:active {
            transform: translateY(-1px);
        }
        
        .output-section {
            background: var(--light);
            border-radius: 20px;
            padding: 30px;
            margin-bottom: 30px;
            box-shadow: 
                inset 0 2px 4px rgba(0, 0, 0, 0.05),
                0 4px 20px rgba(0, 0, 0, 0.08);
            border: 1px solid rgba(255, 255, 255, 0.5);
            transition: all 0.3s ease;
        }
        
        .dark-theme .output-section {
            border: 1px solid rgba(255, 255, 255, 0.1);
        }
        
        .output-section:hover {
            box-shadow: 
                inset 0 2px 4px rgba(0, 0, 0, 0.05),
                0 6px 25px rgba(0, 0, 0, 0.12);
        }
        
        .output-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 20px;
            padding-bottom: 15px;
            border-bottom: 2px solid rgba(0, 0, 0, 0.1);
        }
        
        .dark-theme .output-header {
            border-bottom: 2px solid rgba(255, 255, 255, 0.1);
        }
        
        .output-title {
            font-size: 1.4rem;
            font-weight: 600;
            color: var(--dark);
            display: flex;
            align-items: center;
            gap: 10px;
        }
        
        .output-container {
            background: #1e293b;
            color: #e2e8f0;
            padding: 25px;
            border-radius: 15px;
            font-family: 'Fira Code', 'Cascadia Code', 'Courier New', monospace;
            white-space: pre;
            overflow-x: auto;
            border: 2px solid #334155;
            box-shadow: inset 0 2px 10px rgba(0, 0, 0, 0.5);
            line-height: 1.8;
            font-size: 15px;
            font-weight: 500;
            min-height: 300px;
            position: relative;
            transition: all 0.3s ease;
        }
        
        .dark-theme .output-container {
            background: #0f172a;
            border: 2px solid #475569;
        }
        
        .visualization {
            display: none;
            margin-top: 30px;
        }
        
        .classroom {
            display: flex;
            flex-direction: column;
            align-items: center;
            margin-top: 30px;
            position: relative;
        }
        
        .teacher-desk {
            width: 350px;
            height: 50px;
            background: linear-gradient(135deg, #8e9eab, #eef2f3);
            color: #2c3e50;
            display: flex;
            justify-content: center;
            align-items: center;
            margin-bottom: 50px;
            border-radius: 10px;
            font-weight: bold;
            font-size: 1.3rem;
            box-shadow: 0 8px 25px rgba(0, 0, 0, 0.15);
            border: 3px solid #bdc3c7;
            text-shadow: 1px 1px 2px rgba(0,0,0,0.1);
            transition: all 0.3s ease;
        }
        
        .teacher-desk:hover {
            transform: translateY(-5px);
            box-shadow: 0 12px 30px rgba(0, 0, 0, 0.2);
        }
        
        .rows {
            display: flex;
            flex-direction: column;
            gap: 20px;
            width: 100%;
        }
        
        .row {
            display: flex;
            justify-content: center;
            gap: 20px;
        }
        
        .desk {
            width: 200px;
            height: 90px;
            background: linear-gradient(135deg, #f5f7fa, #c3cfe2);
            border: 3px solid #a5b1c2;
            border-radius: 12px;
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 12px;
            box-shadow: var(--card-shadow);
            transition: all 0.3s ease;
            position: relative;
            animation: deskAppear 0.5s ease-out;
        }
        
        @keyframes deskAppear {
            from { opacity: 0; transform: scale(0.8); }
            to { opacity: 1; transform: scale(1); }
        }
        
        .desk::after {
            content: '';
            position: absolute;
            top: 5px;
            left: 5px;
            right: 5px;
            bottom: 5px;
            border: 1px solid rgba(255,255,255,0.5);
            border-radius: 8px;
            pointer-events: none;
        }
        
        .desk:hover {
            transform: translateY(-5px) scale(1.02);
            box-shadow: var(--hover-shadow);
            border-color: #667eea;
        }
        
        .student {
            text-align: center;
            width: 48%;
            font-size: 1rem;
            font-weight: 600;
            padding: 5px;
            border-radius: 6px;
            transition: all 0.3s ease;
            animation: studentAppear 0.5s ease-out;
        }
        
        @keyframes studentAppear {
            from { opacity: 0; transform: translateY(10px); }
            to { opacity: 1; transform: translateY(0); }
        }
        
        .student.boy {
            background: linear-gradient(135deg, #74b9ff, #0984e3);
            color: white;
            box-shadow: 0 3px 10px rgba(116, 185, 255, 0.3);
        }
        
        .student.girl {
            background: linear-gradient(135deg, #fd79a8, #e84393);
            color: white;
            box-shadow: 0 3px 10px rgba(253, 121, 168, 0.3);
        }
        
        .student:hover {
            transform: scale(1.05);
        }
        
        .stats-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
            gap: 20px;
            margin-top: 30px;
        }
        
        .stat-card {
            background: linear-gradient(135deg, var(--light), white);
            padding: 25px;
            border-radius: 16px;
            text-align: center;
            box-shadow: var(--card-shadow);
            border: 1px solid rgba(255, 255, 255, 0.5);
            transition: all 0.3s ease;
            position: relative;
            overflow: hidden;
            animation: fadeIn 0.6s ease-out;
        }
        
        .dark-theme .stat-card {
            background: linear-gradient(135deg, #1e293b, #334155);
            border: 1px solid rgba(255, 255, 255, 0.1);
        }
        
        .stat-card::before {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            width: 4px;
            height: 100%;
            background: linear-gradient(to bottom, var(--primary), var(--accent));
        }
        
        .stat-card:hover {
            transform: translateY(-5px);
            box-shadow: var(--hover-shadow);
        }
        
        .stat-icon {
            width: 60px;
            height: 60px;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            margin: 0 auto 15px;
            font-size: 1.5rem;
            color: white;
        }
        
        .stat-boy .stat-icon { background: linear-gradient(135deg, #3b82f6, #1d4ed8); }
        .stat-girl .stat-icon { background: linear-gradient(135deg, #ec4899, #be185d); }
        .stat-total .stat-icon { background: linear-gradient(135deg, #10b981, #047857); }
        .stat-seat .stat-icon { background: linear-gradient(135deg, #f59e0b, #d97706); }
        
        .stat-card h3 {
            color: var(--gray);
            font-size: 0.95rem;
            margin-bottom: 8px;
            font-weight: 500;
        }
        
        .stat-card .number {
            font-size: 2.2rem;
            font-weight: 700;
            color: var(--dark);
        }
        
        .loading {
            display: none;
            text-align: center;
            padding: 30px;
        }
        
        .spinner {
            width: 60px;
            height: 60px;
            border: 4px solid rgba(67, 97, 238, 0.2);
            border-top: 4px solid var(--primary);
            border-radius: 50%;
            animation: spin 1s linear infinite;
            margin: 0 auto 15px;
        }
        
        @keyframes spin {
            0% { transform: rotate(0deg); }
            100% { transform: rotate(360deg); }
        }
        
        .pulse {
            animation: pulse 2s infinite;
        }
        
        @keyframes pulse {
            0%, 100% { opacity: 1; }
            50% { opacity: 0.7; }
        }
        
        footer {
            margin-top: 40px;
            text-align: center;
            color: var(--gray);
            padding-top: 25px;
            border-top: 1px solid rgba(0, 0, 0, 0.1);
        }
        
        .dark-theme footer {
            border-top: 1px solid rgba(255, 255, 255, 0.1);
        }
        
        .footer-content {
            display: flex;
            justify-content: space-between;
            align-items: center;
            flex-wrap: wrap;
            gap: 15px;
        }
        
        .version {
            background: var(--light);
            padding: 8px 16px;
            border-radius: 20px;
            font-size: 0.9rem;
            font-weight: 500;
        }
        
        .dark-theme .version {
            background: #334155;
        }
        
        @media (max-width: 768px) {
            .container {
                padding: 25px;
            }
            
            h1 {
                font-size: 2.2rem;
            }
            
            .control-panel {
                flex-direction: column;
                align-items: center;
            }
            
            .btn {
                width: 100%;
                max-width: 300px;
                justify-content: center;
            }
            
            .save-history-option {
                flex-direction: column;
                gap: 10px;
                text-align: center;
            }
            
            .save-history-label {
                margin-right: 0;
            }
            
            .output-container {
                font-size: 13px;
                padding: 20px;
            }
            
            .teacher-desk {
                width: 280px;
                font-size: 1.1rem;
            }
            
            .desk {
                width: 160px;
                height: 75px;
                padding: 8px;
            }
            
            .student {
                font-size: 0.85rem;
            }
            
            .stats-grid {
                grid-template-columns: repeat(2, 1fr);
            }
            
            .footer-content {
                flex-direction: column;
                text-align: center;
            }
        }
        
        @media (max-width: 480px) {
            .container {
                padding: 20px;
            }
            
            h1 {
                font-size: 1.8rem;
            }
            
            .teacher-desk {
                width: 220px;
                font-size: 1rem;
            }
            
            .desk {
                width: 140px;
                height: 65px;
            }
            
            .student {
                font-size: 0.75rem;
            }
            
            .stats-grid {
                grid-template-columns: 1fr;
            }
            
            .output-container {
                font-size: 12px;
            }
        }
        
        .success-message {
            position: fixed;
            top: 20px;
            right: 20px;
            background: var(--success);
            color: white;
            padding: 15px 25px;
            border-radius: 10px;
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.2);
            transform: translateX(400px);
            transition: transform 0.3s ease;
            z-index: 1000;
            display: flex;
            align-items: center;
            gap: 10px;
        }
        
        .success-message.show {
            transform: translateX(0);
        }
        
        .tab-buttons {
            display: flex;
            justify-content: center;
            margin-bottom: 20px;
            gap: 10px;
        }
        
        .tab-btn {
            padding: 12px 24px;
            background: var(--light);
            border: none;
            border-radius: 8px;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.3s ease;
            color: var(--gray);
        }
        
        .dark-theme .tab-btn {
            background: #334155;
        }
        
        .tab-btn.active {
            background: var(--primary);
            color: white;
        }
        
        .tab-content {
            display: none;
        }
        
        .tab-content.active {
            display: block;
        }
        
        .particles {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            z-index: -1;
            overflow: hidden;
        }
        
        .particle {
            position: absolute;
            background: rgba(255, 255, 255, 0.2);
            border-radius: 50%;
            animation: float 15s infinite linear;
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
        
        .class-badge {
            position: absolute;
            top: 20px;
            right: 20px;
            width: 60px;
            height: 60px;
            background: linear-gradient(135deg, #ffd700, #ffa500);
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            box-shadow: 0 5px 15px rgba(255, 165, 0, 0.3);
            animation: rotate 10s linear infinite;
        }
        
        @keyframes rotate {
            0% { transform: rotate(0deg); }
            100% { transform: rotate(360deg); }
        }
        
        .class-badge i {
            font-size: 1.5rem;
            color: white;
        }
        
        .decorative-border {
            position: absolute;
            top: 10px;
            left: 10px;
            right: 10px;
            bottom: 10px;
            border: 2px dashed rgba(67, 97, 238, 0.2);
            border-radius: 20px;
            pointer-events: none;
        }
        
        .bg-pattern {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background-image: 
                radial-gradient(circle at 10% 20%, rgba(67, 97, 238, 0.05) 0%, transparent 20%),
                radial-gradient(circle at 90% 80%, rgba(76, 201, 240, 0.05) 0%, transparent 20%),
                radial-gradient(circle at 50% 50%, rgba(74, 222, 128, 0.05) 0%, transparent 30%);
            z-index: -1;
        }
        
        .title-decoration {
            position: absolute;
            bottom: -10px;
            left: 50%;
            transform: translateX(-50%);
            width: 100px;
            height: 4px;
            background: linear-gradient(90deg, transparent, var(--primary), transparent);
            border-radius: 2px;
        }
        
        .desk-number {
            position: absolute;
            top: -10px;
            left: 50%;
            transform: translateX(-50%);
            background: var(--primary);
            color: white;
            width: 25px;
            height: 25px;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 0.8rem;
            font-weight: bold;
            box-shadow: 0 2px 5px rgba(0,0,0,0.2);
        }
        
        .decoration-star {
            position: absolute;
            color: rgba(255, 255, 255, 0.7);
            font-size: 1.2rem;
            animation: twinkle 3s infinite alternate;
        }
        
        @keyframes twinkle {
            0% { opacity: 0.3; transform: scale(0.8); }
            100% { opacity: 1; transform: scale(1.2); }
        }
        
        .time-display {
            position: absolute;
            top: 20px;
            left: 20px;
            background: rgba(255, 255, 255, 0.2);
            backdrop-filter: blur(10px);
            padding: 10px 15px;
            border-radius: 10px;
            font-size: 0.9rem;
            color: var(--dark);
            box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
            border: 1px solid rgba(255, 255, 255, 0.3);
        }
        
        .dark-theme .time-display {
            background: rgba(30, 41, 59, 0.5);
            color: var(--dark);
            border: 1px solid rgba(255, 255, 255, 0.1);
        }
        
        .class-motto {
            text-align: center;
            margin-top: 10px;
            font-style: italic;
            color: var(--gray);
            font-size: 1rem;
            position: relative;
            padding: 0 20px;
        }
        
        .class-motto::before,
        .class-motto::after {
            content: '"';
            font-size: 1.5rem;
            color: var(--primary);
            position: absolute;
            top: -5px;
        }
        
        .class-motto::before {
            left: 0;
        }
        
        .class-motto::after {
            right: 0;
        }
        
        .progress-bar {
            height: 6px;
            background: rgba(67, 97, 238, 0.2);
            border-radius: 3px;
            overflow: hidden;
            margin-top: 15px;
            display: none;
        }
        
        .progress-fill {
            height: 100%;
            background: linear-gradient(90deg, var(--primary), var(--accent));
            width: 0%;
            transition: width 0.5s ease;
            border-radius: 3px;
        }
        
        .student-card {
            perspective: 1000px;
            width: 48%;
        }
        
        .student-inner {
            position: relative;
            width: 100%;
            height: 100%;
            text-align: center;
            transition: transform 0.6s;
            transform-style: preserve-3d;
        }
        
        .student-card:hover .student-inner {
            transform: rotateY(180deg);
        }
        
        .student-front, .student-back {
            position: absolute;
            width: 100%;
            height: 100%;
            -webkit-backface-visibility: hidden;
            backface-visibility: hidden;
            border-radius: 6px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-weight: 600;
            padding: 5px;
        }
        
        .student-front {
            color: white;
        }
        
        .student-back {
            background: linear-gradient(135deg, #f8fafc, #e2e8f0);
            color: var(--dark);
            transform: rotateY(180deg);
            font-size: 0.8rem;
        }
        
        .dark-theme .student-back {
            background: linear-gradient(135deg, #334155, #475569);
        }
        
        .classroom-bg {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: 
                linear-gradient(90deg, transparent 49%, rgba(0,0,0,0.05) 50%, transparent 51%),
                linear-gradient(transparent 49%, rgba(0,0,0,0.05) 50%, transparent 51%);
            background-size: 50px 50px;
            opacity: 0.1;
            z-index: -1;
        }
        
        .top-controls {
            display: flex;
            justify-content: space-between;
            margin-bottom: 20px;
            flex-wrap: wrap;
            gap: 15px;
        }
        
        .search-box {
            display: flex;
            align-items: center;
            background: var(--light);
            border-radius: 50px;
            padding: 10px 20px;
            box-shadow: var(--card-shadow);
            border: 1px solid rgba(255, 255, 255, 0.5);
            transition: all 0.3s ease;
        }
        
        .dark-theme .search-box {
            background: #334155;
            border: 1px solid rgba(255, 255, 255, 0.1);
        }
        
        .search-box:focus-within {
            box-shadow: var(--hover-shadow);
            transform: translateY(-2px);
        }
        
        .search-box input {
            border: none;
            background: transparent;
            padding: 8px 12px;
            font-size: 1rem;
            color: var(--dark);
            width: 200px;
            outline: none;
        }
        
        .search-box i {
            color: var(--gray);
        }
        
        .theme-toggle {
            display: flex;
            align-items: center;
            gap: 10px;
            background: var(--light);
            border-radius: 50px;
            padding: 10px 20px;
            box-shadow: var(--card-shadow);
            border: 1px solid rgba(255, 255, 255, 0.5);
            cursor: pointer;
            transition: all 0.3s ease;
        }
        
        .dark-theme .theme-toggle {
            background: #334155;
            border: 1px solid rgba(255, 255, 255, 0.1);
        }
        
        .theme-toggle:hover {
            transform: translateY(-2px);
            box-shadow: var(--hover-shadow);
        }
        
        .theme-toggle i {
            color: var(--primary);
        }
        
        .history-panel {
            margin-top: 30px;
            background: var(--light);
            border-radius: 20px;
            padding: 25px;
            box-shadow: var(--card-shadow);
            border: 1px solid rgba(255, 255, 255, 0.5);
            display: none;
        }
        
        .dark-theme .history-panel {
            background: #1e293b;
            border: 1px solid rgba(255, 255, 255, 0.1);
        }
        
        .history-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 20px;
            padding-bottom: 15px;
            border-bottom: 2px solid rgba(0, 0, 0, 0.1);
        }
        
        .dark-theme .history-header {
            border-bottom: 2px solid rgba(255, 255, 255, 0.1);
        }
        
        .history-title {
            font-size: 1.4rem;
            font-weight: 600;
            color: var(--dark);
            display: flex;
            align-items: center;
            gap: 10px;
        }
        
        .history-list {
            max-height: 300px;
            overflow-y: auto;
            padding-right: 10px;
        }
        
        .history-item {
            background: white;
            border-radius: 12px;
            padding: 15px;
            margin-bottom: 15px;
            box-shadow: 0 4px 12px rgba(0, 0, 0, 0.05);
            border-left: 4px solid var(--primary);
            transition: all 0.3s ease;
        }
        
        .dark-theme .history-item {
            background: #334155;
        }
        
        .history-item:hover {
            transform: translateY(-3px);
            box-shadow: 0 6px 15px rgba(0, 0, 0, 0.1);
        }
        
        .history-item-header {
            display: flex;
            justify-content: space-between;
            margin-bottom: 10px;
            font-weight: 600;
            color: var(--dark);
        }
        
        .history-item-content {
            font-family: 'Fira Code', 'Cascadia Code', 'Courier New', monospace;
            white-space: pre;
            overflow-x: auto;
            background: #f8fafc;
            padding: 12px;
            border-radius: 8px;
            font-size: 0.9rem;
            color: #1e293b;
        }
        
        .dark-theme .history-item-content {
            background: #0f172a;
            color: #e2e8f0;
        }
        
        .settings-panel {
            margin-top: 30px;
            background: var(--light);
            border-radius: 20px;
            padding: 25px;
            box-shadow: var(--card-shadow);
            border: 1px solid rgba(255, 255, 255, 0.5);
            display: none;
        }
        
        .dark-theme .settings-panel {
            background: #1e293b;
            border: 1px solid rgba(255, 255, 255, 0.1);
        }
        
        .settings-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 20px;
            padding-bottom: 15px;
            border-bottom: 2px solid rgba(0, 0, 0, 0.1);
        }
        
        .dark-theme .settings-header {
            border-bottom: 2px solid rgba(255, 255, 255, 0.1);
        }
        
        .settings-title {
            font-size: 1.4rem;
            font-weight: 600;
            color: var(--dark);
            display: flex;
            align-items: center;
            gap: 10px;
        }
        
        .setting-item {
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 15px 0;
            border-bottom: 1px solid rgba(0, 0, 0, 0.05);
        }
        
        .dark-theme .setting-item {
            border-bottom: 1px solid rgba(255, 255, 255, 0.05);
        }
        
        .setting-item:last-child {
            border-bottom: none;
        }
        
        .setting-label {
            font-weight: 500;
            color: var(--dark);
        }
        
        .setting-description {
            font-size: 0.9rem;
            color: var(--gray);
            margin-top: 5px;
        }
        
        .highlight {
            background-color: rgba(255, 255, 0, 0.3);
            border-radius: 4px;
            padding: 2px 4px;
        }
        
        .student-result {
            margin-top: 20px;
            padding: 15px;
            background: var(--light);
            border-radius: 12px;
            box-shadow: var(--card-shadow);
            display: none;
        }
        
        .dark-theme .student-result {
            background: #1e293b;
        }
        
        .student-result-header {
            font-weight: 600;
            margin-bottom: 10px;
            color: var(--dark);
            display: flex;
            align-items: center;
            gap: 10px;
        }
        
        .student-details {
            display: flex;
            gap: 20px;
            flex-wrap: wrap;
        }
        
        .student-info {
            flex: 1;
            min-width: 200px;
        }
        
        .student-info p {
            margin-bottom: 8px;
            display: flex;
            justify-content: space-between;
        }
        
        .student-info span:first-child {
            font-weight: 500;
            color: var(--gray);
        }
        
        .student-info span:last-child {
            font-weight: 600;
            color: var(--dark);
        }
        
        .seat-info {
            background: var(--primary);
            color: white;
            padding: 8px 12px;
            border-radius: 8px;
            margin-top: 10px;
            font-weight: 500;
        }
        
        .student-seat-info {
            margin-top: 15px;
            padding: 15px;
            background: var(--light);
            border-radius: 12px;
            box-shadow: var(--card-shadow);
        }
        
        .dark-theme .student-seat-info {
            background: #1e293b;
        }
        
        .seat-position {
            display: flex;
            align-items: center;
            gap: 10px;
            margin-top: 10px;
        }
        
        .seat-position i {
            color: var(--primary);
        }
        
        .language-toggle {
            display: flex;
            align-items: center;
            gap: 10px;
            background: var(--light);
            border-radius: 50px;
            padding: 10px 20px;
            box-shadow: var(--card-shadow);
            border: 1px solid rgba(255, 255, 255, 0.5);
            cursor: pointer;
            transition: all 0.3s ease;
        }
        
        .dark-theme .language-toggle {
            background: #334155;
            border: 1px solid rgba(255, 255, 255, 0.1);
        }
        
        .language-toggle:hover {
            transform: translateY(-2px);
            box-shadow: var(--hover-shadow);
        }
        
        .language-toggle i {
            color: var(--primary);
        }
        
        .login-container {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: var(--bg-gradient);
            display: flex;
            justify-content: center;
            align-items: center;
            z-index: 9999;
            transition: all 0.5s ease;
        }
        
        .login-box {
            background: rgba(255, 255, 255, 0.95);
            backdrop-filter: blur(20px);
            border-radius: 24px;
            box-shadow: 
                0 25px 50px -12px rgba(0, 0, 0, 0.25),
                inset 0 1px 0 rgba(255, 255, 255, 0.3);
            padding: 40px;
            width: 400px;
            max-width: 90%;
            border: 1px solid rgba(255, 255, 255, 0.2);
            position: relative;
            overflow: hidden;
            animation: fadeIn 0.8s ease-out;
        }
        
        .dark-theme .login-box {
            background: rgba(30, 41, 59, 0.95);
            border: 1px solid rgba(255, 255, 255, 0.1);
        }
        
        .login-box::before {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            right: 0;
            height: 4px;
            background: linear-gradient(90deg, 
                var(--primary), 
                var(--accent), 
                var(--success), 
                var(--warning));
            border-radius: 24px 24px 0 0;
        }
        
        .login-logo {
            width: 80px;
            height: 80px;
            background: linear-gradient(135deg, var(--primary), var(--secondary));
            border-radius: 50%;
            margin: 0 auto 20px;
            display: flex;
            align-items: center;
            justify-content: center;
            box-shadow: 0 8px 25px rgba(67, 97, 238, 0.3);
        }
        
        .login-logo i {
            font-size: 2rem;
            color: white;
        }
        
        .login-title {
            text-align: center;
            margin-bottom: 30px;
            color: var(--dark);
            font-size: 1.8rem;
            font-weight: 700;
        }
        
        .login-form {
            display: flex;
            flex-direction: column;
            gap: 20px;
        }
        
        .form-group {
            display: flex;
            flex-direction: column;
            gap: 8px;
        }
        
        .form-label {
            font-weight: 600;
            color: var(--dark);
        }
        
        .form-input {
            padding: 12px 16px;
            border: 2px solid rgba(67, 97, 238, 0.2);
            border-radius: 12px;
            font-size: 1rem;
            transition: all 0.3s ease;
            background: var(--light);
            color: var(--dark);
        }
        
        .dark-theme .form-input {
            background: #334155;
            border: 2px solid rgba(255, 255, 255, 0.1);
        }
        
        .form-input:focus {
            outline: none;
            border-color: var(--primary);
            box-shadow: 0 0 0 3px rgba(67, 97, 238, 0.1);
        }
        
        .login-btn {
            padding: 14px;
            background: linear-gradient(135deg, var(--primary), var(--secondary));
            color: white;
            border: none;
            border-radius: 12px;
            font-size: 1.1rem;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.3s ease;
            margin-top: 10px;
        }
        
        .login-btn:hover {
            transform: translateY(-2px);
            box-shadow: 0 8px 20px rgba(67, 97, 238, 0.3);
        }
        
        .login-error {
            color: var(--danger);
            text-align: center;
            margin-top: 10px;
            font-weight: 500;
            display: none;
        }
        
        .user-info {
            display: flex;
            align-items: center;
            gap: 10px;
            background: var(--light);
            border-radius: 50px;
            padding: 10px 20px;
            box-shadow: var(--card-shadow);
            border: 1px solid rgba(255, 255, 255, 0.5);
        }
        
        .dark-theme .user-info {
            background: #334155;
            border: 1px solid rgba(255, 255, 255, 0.1);
        }
        
        .user-avatar {
            width: 36px;
            height: 36px;
            border-radius: 50%;
            background: linear-gradient(135deg, var(--primary), var(--secondary));
            display: flex;
            align-items: center;
            justify-content: center;
            color: white;
            font-weight: bold;
        }
        
        .logout-btn {
            background: var(--danger);
            color: white;
            border: none;
            border-radius: 50%;
            width: 36px;
            height: 36px;
            display: flex;
            align-items: center;
            justify-content: center;
            cursor: pointer;
            transition: all 0.3s ease;
        }
        
        .logout-btn:hover {
            transform: scale(1.1);
            box-shadow: 0 4px 10px rgba(239, 68, 68, 0.3);
        }
    </style>
</head>
<body>
    <!-- 登录界面 -->
    <div class="login-container" id="loginContainer">
        <div class="login-box">
            <div class="login-logo">
                <i class="fas fa-chalkboard-teacher"></i>
            </div>
            <h2 class="login-title" id="loginTitle">Class 10 Seat System</h2>
            <form class="login-form" id="loginForm">
                <div class="form-group">
                    <label class="form-label" for="username">Username</label>
                    <input type="text" id="username" class="form-input" placeholder="Enter your username" required>
                </div>
                <div class="form-group">
                    <label class="form-label" for="password">Password</label>
                    <input type="password" id="password" class="form-input" placeholder="Enter your password" required>
                </div>
                <button type="submit" class="login-btn">Login</button>
                <div class="login-error" id="loginError">Invalid username or password</div>
            </form>
        </div>
    </div>
    
    <div class="particles" id="particles"></div>
    <div class="bg-pattern"></div>
    <div class="decoration-star" style="top: 10%; left: 5%;"><i class="fas fa-star"></i></div>
    <div class="decoration-star" style="top: 15%; right: 10%;"><i class="fas fa-star"></i></div>
    <div class="decoration-star" style="bottom: 20%; left: 15%;"><i class="fas fa-star"></i></div>
    <div class="decoration-star" style="bottom: 10%; right: 5%;"><i class="fas fa-star"></i></div>
    
    <div class="container" id="mainContainer" style="display: none;">
        <div class="time-display" id="timeDisplay"></div>
        <div class="class-badge">
            <i class="fas fa-graduation-cap"></i>
        </div>
        <div class="decorative-border"></div>
        
        <header>
            <div class="logo">
                <i class="fas fa-chalkboard-teacher"></i>
            </div>
            <h1 id="pageTitle">Class 10, Grade 2024</h1>
            <p class="subtitle" id="pageSubtitle">Seat Random Number Program</p>
            <div class="class-motto" id="classMotto">Perfect in every way, Class 10 fearless, ten battles and ten victories, winning the golden cup.</div>
            <div class="title-decoration"></div>
        </header>
        
        <div class="top-controls">
            <div class="search-box">
                <i class="fas fa-search"></i>
                <input type="text" id="searchInput" placeholder="Search for a student...">
            </div>
            <div class="language-toggle" id="languageToggle">
                <i class="fas fa-globe"></i>
                <span id="languageText">中文</span>
            </div>
            <div class="theme-toggle" id="themeToggle">
                <i class="fas fa-moon"></i>
                <span id="themeText">Dark Mode</span>
            </div>
            <div class="user-info" id="userInfo">
                <div class="user-avatar" id="userAvatar">U</div>
                <span id="userName">User</span>
                <button class="logout-btn" id="logoutBtn" title="Logout">
                    <i class="fas fa-sign-out-alt"></i>
                </button>
            </div>
        </div>
        
        <div class="save-history-option">
            <div class="save-history-label">
                <i class="fas fa-history"></i>
                <span id="saveHistoryLabel">Save to history</span>
            </div>
            <label class="toggle-switch">
                <input type="checkbox" id="saveHistory" checked>
                <span class="toggle-slider"></span>
            </label>
        </div>
        
        <div class="control-panel">
            <button class="btn btn-primary" onclick="generateSeats()">
                <i class="fas fa-random"></i>
                <span id="generateBtnText">🎲 Generate Random Seating Chart</span>
            </button>
            <button class="btn btn-secondary" onclick="resetSystem()">
                <i class="fas fa-redo"></i>
                <span id="resetBtnText">🔄 Reset System</span>
            </button>
            <button class="btn btn-secondary" onclick="toggleHistory()">
                <i class="fas fa-history"></i>
                <span id="historyBtnText">History</span>
            </button>
            <button class="btn btn-secondary" onclick="toggleSettings()">
                <i class="fas fa-cog"></i>
                <span id="settingsBtnText">Settings</span>
            </button>
        </div>
        
        <div class="student-result" id="studentResult">
            <div class="student-result-header">
                <i class="fas fa-user-graduate"></i>
                <span id="searchResultText">Student Search Result</span>
            </div>
            <div class="student-details" id="studentDetails"></div>
        </div>
        
        <div class="progress-bar" id="progressBar">
            <div class="progress-fill" id="progressFill"></div>
        </div>
        
        <div class="loading" id="loading">
            <div class="spinner"></div>
            <p id="loadingText">Seats are being randomly assigned...</p>
        </div>
        
        <div class="output-section">
            <div class="output-header">
                <div class="output-title">
                    <i class="fas fa-table"></i>
                    <span id="outputTitleText">Seat Allocation Result</span>
                </div>
                <div class="tab-buttons">
                    <button class="tab-btn active" onclick="switchTab('console')">
                        <span id="consoleTabText">Console View</span>
                    </button>
                    <button class="tab-btn" onclick="switchTab('visual')">
                        <span id="visualTabText">Visual View</span>
                    </button>
                </div>
            </div>
            
            <div class="tab-content active" id="console-tab">
                <div class="output-container" id="output">
Welcome to use the intelligent seat allocation system.
Click the button above to start generating a random seating chart.
This system ensures the randomness and fairness of seat allocation, with a 3.2% probability of each person being assigned.
                </div>
            </div>
            
            <div class="tab-content" id="visual-tab">
                <div class="classroom">
                    <div class="classroom-bg"></div>
                    <div class="teacher-desk" id="teacherDesk">
                        <i class="fas fa-chalkboard"></i> <span id="teacherDeskText">Teacher's Desk</span>
                    </div>
                    <div class="rows" id="seatingPlan">
                        <div style="text-align: center; padding: 40px; color: #7f8c8d; font-size: 1.2rem;" id="visualPromptText">
                            Click the "Generate Random Seating Chart" button to start seat allocation.
                        </div>
                    </div>
                </div>
            </div>
        </div>
        
        <div class="history-panel" id="historyPanel">
            <div class="history-header">
                <div class="history-title">
                    <i class="fas fa-history"></i>
                    <span id="historyTitleText">Generation History</span>
                </div>
                <button class="btn btn-secondary" onclick="clearHistory()">
                    <i class="fas fa-trash"></i>
                    <span id="clearHistoryText">Clear History</span>
                </button>
            </div>
            <div class="history-list" id="historyList"></div>
        </div>
        
        <div class="settings-panel" id="settingsPanel">
            <div class="settings-header">
                <div class="settings-title">
                    <i class="fas fa-cog"></i>
                    <span id="settingsTitleText">System Settings</span>
                </div>
                <button class="btn btn-secondary" onclick="toggleSettings()">
                    <i class="fas fa-times"></i>
                    <span id="closeSettingsText">Close</span>
                </button>
            </div>
            <div class="settings-content">
                <div class="setting-item">
                    <div>
                        <div class="setting-label" id="autoSaveLabel">Auto-save History</div>
                        <div class="setting-description" id="autoSaveDesc">Automatically save each generation to history</div>
                    </div>
                    <label class="toggle-switch">
                        <input type="checkbox" id="autoSave" checked>
                        <span class="toggle-slider"></span>
                    </label>
                </div>
                <div class="setting-item">
                    <div>
                        <div class="setting-label" id="animationsLabel">Show Animation Effects</div>
                        <div class="setting-description" id="animationsDesc">Enable animations for a better user experience</div>
                    </div>
                    <label class="toggle-switch">
                        <input type="checkbox" id="showAnimations" checked>
                        <span class="toggle-slider"></span>
                    </label>
                </div>
                <div class="setting-item">
                    <div>
                        <div class="setting-label" id="highlightLabel">Highlight Search Results</div>
                        <div class="setting-description" id="highlightDesc">Highlight found students in the seating chart</div>
                    </div>
                    <label class="toggle-switch">
                        <input type="checkbox" id="highlightResults" checked>
                        <span class="toggle-slider"></span>
                    </label>
                </div>
            </div>
        </div>
        
        <div class="stats-grid">
            <div class="stat-card stat-boy">
                <div class="stat-icon">
                    <i class="fas fa-male"></i>
                </div>
                <h3 id="boyCountText">Boy Count</h3>
                <div class="number">31</div>
            </div>
            <div class="stat-card stat-girl">
                <div class="stat-icon">
                    <i class="fas fa-female"></i>
                </div>
                <h3 id="girlCountText">Girl Count</h3>
                <div class="number">23</div>
            </div>
            <div class="stat-card stat-total">
                <div class="stat-icon">
                    <i class="fas fa-users"></i>
                </div>
                <h3 id="totalCountText">All Count</h3>
                <div class="number">54</div>
            </div>
            <div class="stat-card stat-seat">
                <div class="stat-icon">
                    <i class="fas fa-chair"></i>
                </div>
                <h3 id="seatCountText">Seat Count</h3>
                <div class="number">27</div>
            </div>
        </div>
        
        <footer>
            <div class="footer-content">
                <div>
                    <p id="footerText">© 2026 Class 10's Seat Random Number Program | Author by @wrh316 | Website</p>
                    <p style="margin-top: 5px; font-size: 0.9rem; color: #94a3b8;">
                        <i class="fas fa-code"></i> <span id="versionInfoText">Website Versions，C++ Versions：https://note.ms/class10seat</span>
                    </p>
                </div>
                <div class="version">
                    <i class="fas fa-star"></i> <span id="versionText">Version 5.3.6 | Last Update: 2026.3.4</span>
                </div>
            </div>
        </footer>
    </div>

    <div class="success-message" id="successMessage">
        <i class="fas fa-check-circle"></i> <span id="successText">The Code has Completed Successfully</span>
    </div>

    <script>
        // 用户账户数据
        const users = {
            "public": "Lengendary Grandmaster",
            "wrh316": "998244353orz%%%"
        };
        
        // 语言资源 (保持不变)
        const resources = {
            en: {
                pageTitle: "Class 10, Grade 2024",
                pageSubtitle: "Seat Random Number Program",
                classMotto: "Perfect in every way, Class 10 fearless, ten battles and ten victories, winning the golden cup.",
                generateBtnText: "🎲 Generate Random Seating Chart",
                resetBtnText: "🔄 Reset System",
                historyBtnText: "History",
                settingsBtnText: "Settings",
                saveHistoryLabel: "Save to history",
                searchPlaceholder: "Search for a student...",
                searchResultText: "Student Search Result",
                outputTitleText: "Seat Allocation Result",
                consoleTabText: "Console View",
                visualTabText: "Visual View",
                teacherDeskText: "Teacher's Desk",
                visualPromptText: 'Click the "Generate Random Seating Chart" button to start seat allocation.',
                loadingText: "Seats are being randomly assigned...",
                boyCountText: "Boy Count",
                girlCountText: "Girl Count",
                totalCountText: "All Count",
                seatCountText: "Seat Count",
                historyTitleText: "Generation History",
                clearHistoryText: "Clear History",
                settingsTitleText: "System Settings",
                closeSettingsText: "Close",
                autoSaveLabel: "Auto-save History",
                autoSaveDesc: "Automatically save each generation to history",
                animationsLabel: "Show Animation Effects",
                animationsDesc: "Enable animations for a better user experience",
                highlightLabel: "Highlight Search Results",
                highlightDesc: "Highlight found students in the seating chart",
                themeText: "Dark Mode",
                languageText: "中文",
                successText: "The Code has Completed Successfully",
                footerText: "© 2026 Class 10's Seat Random Number Program | Author by @wrh316 | Website",
                versionInfoText: "Website Versions，C++ Versions：https://note.ms/class10seat",
                versionText: "Version 5.3.6 | Last Update: 2026.3.4",
                initialOutput: "Welcome to use the intelligent seat allocation system.\nClick the button above to start generating a random seating chart.\nThis system ensures the randomness and fairness of seat allocation, with a 3.2% probability of each person being assigned.",
                loginTitle: "Class 10 Seat System",
                usernameLabel: "Username",
                passwordLabel: "Password",
                loginButton: "Login",
                loginError: "Invalid username or password"
            },
            zh: {
                pageTitle: "2024级10班",
                pageSubtitle: "座位随机分配系统",
                classMotto: "十全十美，十班无畏，十战十胜，勇夺金杯。",
                generateBtnText: "🎲 生成随机座位表",
                resetBtnText: "🔄 重置系统",
                historyBtnText: "历史记录",
                settingsBtnText: "设置",
                saveHistoryLabel: "保存到历史记录",
                searchPlaceholder: "搜索学生...",
                searchResultText: "学生搜索结果",
                outputTitleText: "座位分配结果",
                consoleTabText: "控制台视图",
                visualTabText: "可视化视图",
                teacherDeskText: "讲台",
                visualPromptText: '点击"生成随机座位表"按钮开始座位分配。',
                loadingText: "正在随机分配座位...",
                boyCountText: "男生人数",
                girlCountText: "女生人数",
                totalCountText: "总人数",
                seatCountText: "座位数量",
                historyTitleText: "生成历史",
                clearHistoryText: "清空历史",
                settingsTitleText: "系统设置",
                closeSettingsText: "关闭",
                autoSaveLabel: "自动保存历史",
                autoSaveDesc: "自动将每次生成保存到历史记录",
                animationsLabel: "显示动画效果",
                animationsDesc: "启用动画以获得更好的用户体验",
                highlightLabel: "高亮搜索结果",
                highlightDesc: "在座位表中高亮显示找到的学生",
                themeText: "深色模式",
                languageText: "English",
                successText: "代码已成功完成",
                footerText: "© 2026 10班座位随机分配系统 | 作者：@wrh316 | 网站",
                versionInfoText: "网页版本，C++版本：https://note.ms/class10seat",
                versionText: "版本 5.3.6 | 最后更新：2026.3.4",
                initialOutput: "欢迎使用智能座位分配系统。\n点击上方按钮开始生成随机座位表。\n本系统确保座位分配的随机性和公平性，每个人被分配的概率为3.2%。",
                loginTitle: "10班座位系统",
                usernameLabel: "用户名",
                passwordLabel: "密码",
                loginButton: "登录",
                loginError: "用户名或密码错误"
            }
        };

        // 学生名单
        const boys = [
            "", "张逸轩", "张庐焜", "黄月童", "吴弘宇", "张涵睿", "余浩玮", "许正涛",
            "王若鸿", "张炜承", "郭辰睿", "闻泽", "高竟翔", "吴进宇", "章津跃", "辛伯辰",
            "昌靖茗", "高伟宸", "柯源", "徐浩中", "王海骁", "张欣成", "范洪鑫", "杨颜旗",
            "李元昊", "刘柯汉", "章横溢", "陈思友", "李浚玮", "李承志", "孙家腾", "李辰熙"
        ];
        
        const girls = [
            "", "毕思妍", "万金慧", "王泽予", "闫可欣", "袁灏亓", "王璟瑜", "周依冉",
            "魏可晗", "陈晓希", "汪嘉彦", "沈傲然", "刘洋东", "余正月", "王泺辰", "杨渃馨",
            "余正秋", "张瀛嘉", "汪悦桐", "陈雨谦", "孙天佑", "黄焱熔", "肖雅祺", "常昕妤"
        ];
        
        // 全局变量
        let currentSeating = null;
        let history = JSON.parse(localStorage.getItem('seatHistory')) || [];
        let isDarkTheme = localStorage.getItem('darkTheme') === 'true';
        let currentLanguage = localStorage.getItem('language') || 'en';
        let currentUser = null;
        let deskMateHistory = JSON.parse(localStorage.getItem('deskMateHistory')) || {};
        
        // 初始化应用 (保持不变)
        function initializeApp() {
            const savedUser = localStorage.getItem('currentUser');
            if (savedUser && users[savedUser]) {
                currentUser = savedUser;
                showMainApp();
            } else {
                showLogin();
            }
            
            if (isDarkTheme) {
                document.body.classList.add('dark-theme');
                document.getElementById('themeToggle').innerHTML = '<i class="fas fa-sun"></i><span id="themeText">Light Mode</span>';
            }
            
            setLanguage(currentLanguage);
            
            updateClock();
            setInterval(updateClock, 1000);
            
            createParticles();
        }
        
        function showLogin() {
            document.getElementById('loginContainer').style.display = 'flex';
            document.getElementById('mainContainer').style.display = 'none';
        }
        
        function showMainApp() {
            document.getElementById('loginContainer').style.display = 'none';
            document.getElementById('mainContainer').style.display = 'block';
            
            document.getElementById('userName').textContent = currentUser;
            document.getElementById('userAvatar').textContent = currentUser.charAt(0).toUpperCase();
        }
        
        function login(username, password) {
            if (users[username] && users[username] === password) {
                currentUser = username;
                localStorage.setItem('currentUser', username);
                showMainApp();
                return true;
            }
            return false;
        }
        
        function logout() {
            currentUser = null;
            localStorage.removeItem('currentUser');
            showLogin();
        }
        
        function setLanguage(lang) {
            currentLanguage = lang;
            localStorage.setItem('language', lang);
            
            const resource = resources[lang];
            
            document.getElementById('pageTitle').textContent = resource.pageTitle;
            document.getElementById('pageSubtitle').textContent = resource.pageSubtitle;
            document.getElementById('classMotto').textContent = resource.classMotto;
            document.getElementById('generateBtnText').textContent = resource.generateBtnText;
            document.getElementById('resetBtnText').textContent = resource.resetBtnText;
            document.getElementById('historyBtnText').textContent = resource.historyBtnText;
            document.getElementById('settingsBtnText').textContent = resource.settingsBtnText;
            document.getElementById('saveHistoryLabel').textContent = resource.saveHistoryLabel;
            document.getElementById('searchInput').placeholder = resource.searchPlaceholder;
            document.getElementById('searchResultText').textContent = resource.searchResultText;
            document.getElementById('outputTitleText').textContent = resource.outputTitleText;
            document.getElementById('consoleTabText').textContent = resource.consoleTabText;
            document.getElementById('visualTabText').textContent = resource.visualTabText;
            document.getElementById('teacherDeskText').textContent = resource.teacherDeskText;
            document.getElementById('visualPromptText').textContent = resource.visualPromptText;
            document.getElementById('loadingText').textContent = resource.loadingText;
            document.getElementById('boyCountText').textContent = resource.boyCountText;
            document.getElementById('girlCountText').textContent = resource.girlCountText;
            document.getElementById('totalCountText').textContent = resource.totalCountText;
            document.getElementById('seatCountText').textContent = resource.seatCountText;
            document.getElementById('historyTitleText').textContent = resource.historyTitleText;
            document.getElementById('clearHistoryText').textContent = resource.clearHistoryText;
            document.getElementById('settingsTitleText').textContent = resource.settingsTitleText;
            document.getElementById('closeSettingsText').textContent = resource.closeSettingsText;
            document.getElementById('autoSaveLabel').textContent = resource.autoSaveLabel;
            document.getElementById('autoSaveDesc').textContent = resource.autoSaveDesc;
            document.getElementById('animationsLabel').textContent = resource.animationsLabel;
            document.getElementById('animationsDesc').textContent = resource.animationsDesc;
            document.getElementById('highlightLabel').textContent = resource.highlightLabel;
            document.getElementById('highlightDesc').textContent = resource.highlightDesc;
            document.getElementById('themeText').textContent = resource.themeText;
            document.getElementById('languageText').textContent = resource.languageText;
            document.getElementById('successText').textContent = resource.successText;
            document.getElementById('footerText').textContent = resource.footerText;
            document.getElementById('versionInfoText').textContent = resource.versionInfoText;
            document.getElementById('versionText').textContent = resource.versionText;
            
            document.getElementById('loginTitle').textContent = resource.loginTitle;
            document.querySelector('label[for="username"]').textContent = resource.usernameLabel;
            document.querySelector('label[for="password"]').textContent = resource.passwordLabel;
            document.querySelector('.login-btn').textContent = resource.loginButton;
            document.getElementById('loginError').textContent = resource.loginError;
            
            if (!currentSeating) {
                document.getElementById('output').textContent = resource.initialOutput;
            }
        }
        
        function toggleLanguage() {
            const newLanguage = currentLanguage === 'en' ? 'zh' : 'en';
            setLanguage(newLanguage);
        }
        
        function createParticles() {
            const particlesContainer = document.getElementById('particles');
            const particleCount = 30;
            
            for (let i = 0; i < particleCount; i++) {
                const particle = document.createElement('div');
                particle.classList.add('particle');
                
                const size = Math.random() * 10 + 5;
                particle.style.width = `${size}px`;
                particle.style.height = `${size}px`;
                
                particle.style.left = `${Math.random() * 100}%`;
                
                particle.style.animationDelay = `${Math.random() * 15}s`;
                
                const duration = Math.random() * 10 + 10;
                particle.style.animationDuration = `${duration}s`;
                
                particlesContainer.appendChild(particle);
            }
        }
        
        async function getSecureRandomValues(length) {
            const array = new Uint32Array(length);
            window.crypto.getRandomValues(array);
            return array;
        }
        
        async function shuffleArraySecure(array) {
            const shuffledArray = [...array];
            const randomValues = await getSecureRandomValues(shuffledArray.length);
            
            for (let i = shuffledArray.length - 1; i > 0; i--) {
                const j = randomValues[i] % (i + 1);
                [shuffledArray[i], shuffledArray[j]] = [shuffledArray[j], shuffledArray[i]];
            }
            
            return shuffledArray;
        }
        
        function setw(text, width) {
            let str = text.toString();
            while (str.length < width) {
                str = ' ' + str;
            }
            return str;
        }
        
        function showSuccessMessage() {
            const message = document.getElementById('successMessage');
            message.classList.add('show');
            setTimeout(() => {
                message.classList.remove('show');
            }, 3000);
        }
        
        function switchTab(tabName) {
            document.querySelectorAll('.tab-content').forEach(tab => {
                tab.classList.remove('active');
            });
            document.querySelectorAll('.tab-btn').forEach(btn => {
                btn.classList.remove('active');
            });
            
            document.getElementById(`${tabName}-tab`).classList.add('active');
            event.target.classList.add('active');
        }
        
        function isBoy(name) {
            return boys.includes(name);
        }
        
        function updateClock() {
            const now = new Date();
            const timeString = now.toLocaleString('zh-CN', {
                year: 'numeric',
                month: '2-digit',
                day: '2-digit',
                hour: '2-digit',
                minute: '2-digit',
                second: '2-digit',
                weekday: 'long'
            });
            document.getElementById('timeDisplay').textContent = timeString;
        }
        
        function createDesk(student1, student2, deskNumber) {
            const desk = document.createElement('div');
            desk.className = 'desk';
            
            const deskNumberElem = document.createElement('div');
            deskNumberElem.className = 'desk-number';
            deskNumberElem.textContent = deskNumber;
            desk.appendChild(deskNumberElem);
            
            const student1Elem = document.createElement('div');
            student1Elem.className = `student ${isBoy(student1) ? 'boy' : 'girl'}`;
            student1Elem.textContent = student1;
            
            const student2Elem = document.createElement('div');
            student2Elem.className = `student ${isBoy(student2) ? 'boy' : 'girl'}`;
            student2Elem.textContent = student2;
            
            desk.appendChild(student1Elem);
            desk.appendChild(student2Elem);
            
            return desk;
        }
        
        // 获取学生未坐过的同桌列表 (过滤已使用的人)
        function getAvailableDeskMates(student, usedPeople) {
            const allStudents = [...boys.slice(1), ...girls.slice(1)];
            const currentDeskMates = deskMateHistory[student] || [];
            
            return allStudents.filter(otherStudent => 
                otherStudent !== student && 
                !currentDeskMates.includes(otherStudent) &&
                !usedPeople.includes(otherStudent) // 确保这个人还没有被分配出去
            );
        }
        
        // 更新同桌历史记录
        function updateDeskMateHistory(student1, student2) {
            if (!deskMateHistory[student1]) {
                deskMateHistory[student1] = [];
            }
            if (!deskMateHistory[student2]) {
                deskMateHistory[student2] = [];
            }
            
            if (!deskMateHistory[student1].includes(student2)) {
                deskMateHistory[student1].push(student2);
            }
            
            if (!deskMateHistory[student2].includes(student1)) {
                deskMateHistory[student2].push(student1);
            }
            
            localStorage.setItem('deskMateHistory', JSON.stringify(deskMateHistory));
        }
        
        // 生成座位表 (修改核心逻辑，确保23对男女，4对男男，优先匹配未做过同桌)
        async function generateSeats() {
            const output = document.getElementById('output');
            const seatingPlan = document.getElementById('seatingPlan');
            const loading = document.getElementById('loading');
            const progressBar = document.getElementById('progressBar');
            const progressFill = document.getElementById('progressFill');
            
            loading.style.display = 'block';
            progressBar.style.display = 'block';
            output.textContent = currentLanguage === 'en' 
                ? 'Seats are being allocated. Please wait a moment...' 
                : '正在分配座位，请稍候...';
            
            let progress = 0;
            const progressInterval = setInterval(() => {
                progress += 5;
                progressFill.style.width = `${progress}%`;
                if (progress >= 100) {
                    clearInterval(progressInterval);
                }
            }, 100);
            
            setTimeout(async () => {
                let outputText = '';
                
                // 创建男生索引数组 (1-31) 和女生索引数组 (1-23)
                let boyIndices = Array.from({length: 31}, (_, i) => i + 1);
                let girlIndices = Array.from({length: 23}, (_, i) => i + 1);
                
                // 打乱顺序
                boyIndices = await shuffleArraySecure(boyIndices);
                girlIndices = await shuffleArraySecure(girlIndices);
                
                // 存储当前分配中用过的学生名字 (确保不重复分配)
                let usedNames = [];
                
                // 存储待分配的男生名字列表和女生名字列表 (根据打乱后的索引)
                let boyNames = boyIndices.map(i => boys[i]);
                let girlNames = girlIndices.map(i => girls[i]);
                
                // --- 第一步：分配23对男女同桌 ---
                let boyGirlSeats = [];
                // 使用一个临时副本，方便移除已经分配的人
                let availableBoys = [...boyNames];
                let availableGirls = [...girlNames];
                
                for (let i = 0; i < 23; i++) {
                    // 从剩余男生女生中各取一个 (按顺序，但已打乱)
                    // 优先尝试寻找没做过同桌的组合
                    let chosenBoy = availableBoys[0]; // 默认取第一个
                    let chosenGirl = availableGirls[0];
                    let foundCompatible = false;
                    
                    // 遍历剩余男生，尝试为这个男生找一个没同桌过的女生
                    for (let bIdx = 0; bIdx < availableBoys.length; bIdx++) {
                        const boy = availableBoys[bIdx];
                        // 获取这个男生没同桌过的女生列表 (且还在availableGirls中)
                        const neverMetGirls = availableGirls.filter(girl => 
                            !(deskMateHistory[boy] && deskMateHistory[boy].includes(girl))
                        );
                        if (neverMetGirls.length > 0) {
                            chosenBoy = boy;
                            // 随机选一个没同桌过的女生 (也可以取第一个，但为了随机性，随机选)
                            const randomIndex = Math.floor(Math.random() * neverMetGirls.length);
                            chosenGirl = neverMetGirls[randomIndex];
                            foundCompatible = true;
                            break;
                        }
                    }
                    
                    // 如果没找到兼容的，就仍然用原配 (第一个男生和第一个女生)
                    if (!foundCompatible) {
                        chosenBoy = availableBoys[0];
                        chosenGirl = availableGirls[0];
                    }
                    
                    // 决定座位左右顺序：交替男女/女男，使前后排错开
                    let seatType = (i % 2 === 0) ? 'boy-girl' : 'girl-boy';
                    if (seatType === 'boy-girl') {
                        boyGirlSeats.push({
                            type: 'boy-girl',
                            student1: chosenBoy,
                            student2: chosenGirl
                        });
                    } else {
                        boyGirlSeats.push({
                            type: 'girl-boy',
                            student1: chosenGirl,
                            student2: chosenBoy
                        });
                    }
                    
                    // 从可用列表中移除已分配的两个人
                    availableBoys = availableBoys.filter(b => b !== chosenBoy);
                    availableGirls = availableGirls.filter(g => g !== chosenGirl);
                    
                    // 记录已用名字
                    usedNames.push(chosenBoy, chosenGirl);
                    // 更新历史 (先更新，供后续配对参考)
                    updateDeskMateHistory(chosenBoy, chosenGirl);
                }
                
                // --- 第二步：分配4对男男同桌 ---
                // 剩余男生: availableBoys 中应该还有 31-23 = 8 个男生
                let boyBoySeats = [];
                // 打乱剩余男生顺序，增加随机性
                availableBoys = await shuffleArraySecure(availableBoys);
                
                for (let i = 0; i < 4; i++) {
                    // 从剩余男生中取两个
                    let boy1 = availableBoys[0];
                    let boy2 = availableBoys[1];
                    let foundCompatible = false;
                    
                    // 尝试寻找没同桌过的男生对
                    for (let b1Idx = 0; b1Idx < availableBoys.length; b1Idx++) {
                        const candidate1 = availableBoys[b1Idx];
                        // 找candidate1没同桌过的男生 (从剩下的里面)
                        for (let b2Idx = b1Idx + 1; b2Idx < availableBoys.length; b2Idx++) {
                            const candidate2 = availableBoys[b2Idx];
                            if (!(deskMateHistory[candidate1] && deskMateHistory[candidate1].includes(candidate2)) &&
                                !(deskMateHistory[candidate2] && deskMateHistory[candidate2].includes(candidate1))) {
                                boy1 = candidate1;
                                boy2 = candidate2;
                                foundCompatible = true;
                                break;
                            }
                        }
                        if (foundCompatible) break;
                    }
                    
                    // 如果没找到兼容对，就用前两个
                    if (!foundCompatible) {
                        boy1 = availableBoys[0];
                        boy2 = availableBoys[1];
                    }
                    
                    boyBoySeats.push({
                        type: 'boy-boy',
                        student1: boy1,
                        student2: boy2
                    });
                    
                    // 移除已分配的两个人
                    availableBoys = availableBoys.filter(b => b !== boy1 && b !== boy2);
                    
                    usedNames.push(boy1, boy2);
                    updateDeskMateHistory(boy1, boy2);
                }
                
                // 合并所有座位
                let allSeats = [...boyGirlSeats, ...boyBoySeats];
                
                // 再次打乱座位顺序，增加随机性
                allSeats = await shuffleArraySecure(allSeats);
                
                // 进一步调整前后排男女交错 (简单逻辑：遍历，如果前排是男女，后排尝试改成相反的左右)
                for (let i = 4; i < allSeats.length; i++) {
                    const prevDesk = allSeats[i-4];
                    const currentDesk = allSeats[i];
                    // 如果前一桌是男女混合，且当前也是男女混合，则尝试交换左右位置实现交错
                    if ((prevDesk.type === 'boy-girl' || prevDesk.type === 'girl-boy') &&
                        (currentDesk.type === 'boy-girl' || currentDesk.type === 'girl-boy')) {
                        // 如果前一桌是 boy-girl，当前也是 boy-girl，就交换当前
                        if (prevDesk.type === 'boy-girl' && currentDesk.type === 'boy-girl') {
                            currentDesk.type = 'girl-boy';
                            [currentDesk.student1, currentDesk.student2] = [currentDesk.student2, currentDesk.student1];
                        } else if (prevDesk.type === 'girl-boy' && currentDesk.type === 'girl-boy') {
                            currentDesk.type = 'boy-girl';
                            [currentDesk.student1, currentDesk.student2] = [currentDesk.student2, currentDesk.student1];
                        }
                    }
                }
                
                // 生成可视化座位和输出文本
                seatingPlan.innerHTML = '';
                let deskCounter = 1;
                outputText = '';
                
                for (let i = 0; i < 7; i++) {
                    const row = document.createElement('div');
                    row.className = 'row';
                    let line = '| ';
                    
                    for (let j = 0; j < 4; j++) {
                        const seatIndex = i * 4 + j;
                        if (seatIndex < allSeats.length) {
                            const seat = allSeats[seatIndex];
                            line += setw(seat.student1, 4) + ' ' + setw(seat.student2, 4) + ' | ';
                            
                            const desk = createDesk(
                                seat.student1, 
                                seat.student2,
                                deskCounter++
                            );
                            row.appendChild(desk);
                        } else {
                            // 理论上不会超出，因为27个座位正好7*4=28，有1个空位？但实际27个座位，7行4列=28，会有一个空位，但我们只有27个，所以最后一排可能只有3个座位
                            // 处理剩余不足4个的情况
                            for (let k = j; k < 4; k++) {
                                line += '        | '; // 空位占位
                            }
                            break;
                        }
                    }
                    
                    outputText += line + '\n';
                    seatingPlan.appendChild(row);
                }
                
                // 如果因为座位数不是4的倍数，可能会有空位，但27个座位7行4列最后一排3个，正好。
                // 但为了严谨，再补充一个空行输出？
                // 实际上面循环已经处理。
                
                output.textContent = outputText;
                loading.style.display = 'none';
                progressBar.style.display = 'none';
                progressFill.style.width = '0%';
                
                const saveHistory = document.getElementById('saveHistory').checked;
                if (saveHistory) {
                    saveToHistory(outputText);
                }
                
                document.getElementById('saveHistory').checked = false;
                
                showSuccessMessage();
                
            }, 1500);
        }
        
        // 重置系统
        function resetSystem() {
            if (confirm(currentLanguage === 'en' 
                ? "Are you sure you want to reset the system? This will clear all history and cache." 
                : "确定要重置系统吗？这将清除所有历史记录和缓存。")) {
                
                history = [];
                localStorage.removeItem('seatHistory');
                
                deskMateHistory = {};
                localStorage.removeItem('deskMateHistory');
                
                const resource = resources[currentLanguage];
                document.getElementById('output').textContent = resource.initialOutput;
                document.getElementById('seatingPlan').innerHTML = `<div style="text-align: center; padding: 40px; color: #7f8c8d; font-size: 1.2rem;">${resource.visualPromptText}</div>`;
                document.getElementById('studentResult').style.display = 'none';
                document.getElementById('historyPanel').style.display = 'none';
                currentSeating = null;
                
                document.getElementById('saveHistory').checked = false;
                
                renderHistory();
                
                showSuccessMessage();
            }
        }
        
        function toggleTheme() {
            isDarkTheme = !isDarkTheme;
            document.body.classList.toggle('dark-theme');
            
            const themeToggle = document.getElementById('themeToggle');
            if (isDarkTheme) {
                themeToggle.innerHTML = '<i class="fas fa-sun"></i><span id="themeText">Light Mode</span>';
            } else {
                themeToggle.innerHTML = '<i class="fas fa-moon"></i><span id="themeText">Dark Mode</span>';
            }
            
            document.getElementById('themeText').textContent = resources[currentLanguage].themeText;
            
            localStorage.setItem('darkTheme', isDarkTheme);
        }
        
        function toggleHistory() {
            const historyPanel = document.getElementById('historyPanel');
            const settingsPanel = document.getElementById('settingsPanel');
            
            if (historyPanel.style.display === 'block') {
                historyPanel.style.display = 'none';
            } else {
                historyPanel.style.display = 'block';
                settingsPanel.style.display = 'none';
                renderHistory();
            }
        }
        
        function toggleSettings() {
            const settingsPanel = document.getElementById('settingsPanel');
            const historyPanel = document.getElementById('historyPanel');
            
            if (settingsPanel.style.display === 'block') {
                settingsPanel.style.display = 'none';
            } else {
                settingsPanel.style.display = 'block';
                historyPanel.style.display = 'none';
            }
        }
        
        function saveToHistory(outputText) {
            const historyItem = {
                output: outputText,
                timestamp: new Date().toISOString(),
                date: new Date().toLocaleString()
            };
            
            history.unshift(historyItem);
            
            if (history.length > 10) {
                history = history.slice(0, 10);
            }
            
            localStorage.setItem('seatHistory', JSON.stringify(history));
        }
        
        function renderHistory() {
            const historyList = document.getElementById('historyList');
            historyList.innerHTML = '';
            
            if (history.length === 0) {
                const noHistoryText = currentLanguage === 'en' 
                    ? 'No history records yet.' 
                    : '暂无历史记录。';
                historyList.innerHTML = `<div style="text-align: center; padding: 20px; color: var(--gray);">${noHistoryText}</div>`;
                return;
            }
            
            history.forEach((item, index) => {
                const historyItem = document.createElement('div');
                historyItem.className = 'history-item';
                
                const header = document.createElement('div');
                header.className = 'history-item-header';
                header.innerHTML = `
                    <span>${currentLanguage === 'en' ? 'Generation' : '生成'} #${history.length - index}</span>
                    <span>${item.date}</span>
                `;
                
                const content = document.createElement('div');
                content.className = 'history-item-content';
                content.textContent = item.output;
                
                historyItem.appendChild(header);
                historyItem.appendChild(content);
                
                historyItem.addEventListener('click', () => {
                    loadHistoryItem(item);
                });
                
                historyList.appendChild(historyItem);
            });
        }
        
        function loadHistoryItem(item) {
            document.getElementById('output').textContent = item.output;
            document.getElementById('historyPanel').style.display = 'none';
            showSuccessMessage();
        }
        
        function clearHistory() {
            if (confirm(currentLanguage === 'en' 
                ? "Are you sure you want to clear all history?" 
                : "确定要清空所有历史记录吗？")) {
                history = [];
                localStorage.setItem('seatHistory', JSON.stringify(history));
                renderHistory();
            }
        }
        
        // 搜索学生
        function searchStudent() {
            const searchInput = document.getElementById('searchInput');
            const searchTerm = searchInput.value.trim();
            const studentResult = document.getElementById('studentResult');
            const studentDetails = document.getElementById('studentDetails');
            
            if (!searchTerm) {
                studentResult.style.display = 'none';
                return;
            }
            
            const allStudents = [...boys.slice(1), ...girls.slice(1)];
            const foundStudents = allStudents.filter(student => 
                student.includes(searchTerm)
            );
            
            if (foundStudents.length === 0) {
                const noStudentText = currentLanguage === 'en' 
                    ? `No student found with name: "${searchTerm}"` 
                    : `未找到名为"${searchTerm}"的学生`;
                studentDetails.innerHTML = `<p>${noStudentText}</p>`;
                studentResult.style.display = 'block';
                return;
            }
            
            let resultHTML = '';
            foundStudents.forEach(student => {
                const isMale = isBoy(student);
                const gender = isMale 
                    ? (currentLanguage === 'en' ? 'Male' : '男生') 
                    : (currentLanguage === 'en' ? 'Female' : '女生');
                const className = isMale ? 'boy' : 'girl';
                const classText = currentLanguage === 'en' ? 'Class 10, Grade 2024' : '2024级10班';
                
                resultHTML += `
                    <div class="student-info">
                        <p><span>${currentLanguage === 'en' ? 'Name' : '姓名'}:</span> <span>${student}</span></p>
                        <p><span>${currentLanguage === 'en' ? 'Gender' : '性别'}:</span> <span class="${className}">${gender}</span></p>
                        <p><span>${currentLanguage === 'en' ? 'Class' : '班级'}:</span> <span>${classText}</span></p>
                    </div>
                `;
                
                if (currentSeating) {
                    const seatInfo = findStudentSeat(student);
                    if (seatInfo) {
                        const seatInfoText = currentLanguage === 'en' ? 'Seat Information' : '座位信息';
                        const positionText = seatInfo.position === 'Left' 
                            ? (currentLanguage === 'en' ? 'Left' : '左侧') 
                            : (currentLanguage === 'en' ? 'Right' : '右侧');
                        const deskMateText = currentLanguage === 'en' ? 'Desk mate' : '同桌';
                        
                        resultHTML += `
                            <div class="student-seat-info">
                                <div class="seat-info">
                                    <i class="fas fa-chair"></i> ${seatInfoText}
                                </div>
                                <div class="seat-position">
                                    <i class="fas fa-map-marker-alt"></i>
                                    <span>${currentLanguage === 'en' ? 'Desk' : '座位'} ${seatInfo.deskNumber}, ${positionText}</span>
                                </div>
                                <div class="seat-position">
                                    <i class="fas fa-user-friends"></i>
                                    <span>${deskMateText}: ${seatInfo.deskMate}</span>
                                </div>
                            </div>
                        `;
                    }
                }
            });
            
            studentDetails.innerHTML = resultHTML;
            studentResult.style.display = 'block';
            
            if (document.getElementById('highlightResults').checked && currentSeating) {
                highlightStudentInSeating(foundStudents);
            }
        }
        
        function findStudentSeat(studentName) {
            if (!currentSeating) return null;
            
            const desks = document.querySelectorAll('.desk');
            for (let i = 0; i < desks.length; i++) {
                const desk = desks[i];
                const students = desk.querySelectorAll('.student');
                const deskNumber = desk.querySelector('.desk-number').textContent;
                
                for (let j = 0; j < students.length; j++) {
                    if (students[j].textContent === studentName) {
                        const deskMate = students[(j + 1) % 2].textContent;
                        const position = j === 0 ? 'Left' : 'Right';
                        
                        return {
                            deskNumber: deskNumber,
                            position: position,
                            deskMate: deskMate
                        };
                    }
                }
            }
            
            return null;
        }
        
        function highlightStudentInSeating(studentNames) {
            const desks = document.querySelectorAll('.desk');
            
            desks.forEach(desk => {
                const students = desk.querySelectorAll('.student');
                students.forEach(student => {
                    student.classList.remove('highlight');
                });
            });
            
            desks.forEach(desk => {
                const students = desk.querySelectorAll('.student');
                students.forEach(student => {
                    if (studentNames.includes(student.textContent)) {
                        student.classList.add('highlight');
                    }
                });
            });
        }
        
        // 页面加载
        window.addEventListener('DOMContentLoaded', () => {
            console.log('The intelligent seat allocation system has been loaded.');
            initializeApp();
            
            document.getElementById('themeToggle').addEventListener('click', toggleTheme);
            document.getElementById('languageToggle').addEventListener('click', toggleLanguage);
            document.getElementById('searchInput').addEventListener('input', searchStudent);
            document.getElementById('logoutBtn').addEventListener('click', logout);
            
            document.getElementById('loginForm').addEventListener('submit', function(e) {
                e.preventDefault();
                const username = document.getElementById('username').value;
                const password = document.getElementById('password').value;
                
                if (login(username, password)) {
                    document.getElementById('loginError').style.display = 'none';
                } else {
                    document.getElementById('loginError').style.display = 'block';
                }
            });
            
            const autoSave = localStorage.getItem('autoSave');
            const showAnimations = localStorage.getItem('showAnimations');
            const highlightResults = localStorage.getItem('highlightResults');
            
            if (autoSave !== null) {
                document.getElementById('autoSave').checked = autoSave === 'true';
            }
            
            if (showAnimations !== null) {
                document.getElementById('showAnimations').checked = showAnimations === 'true';
            }
            
            if (highlightResults !== null) {
                document.getElementById('highlightResults').checked = highlightResults === 'true';
            }
            
            document.getElementById('autoSave').addEventListener('change', function() {
                localStorage.setItem('autoSave', this.checked);
            });
            
            document.getElementById('showAnimations').addEventListener('change', function() {
                localStorage.setItem('showAnimations', this.checked);
            });
            
            document.getElementById('highlightResults').addEventListener('change', function() {
                localStorage.setItem('highlightResults', this.checked);
            });
        });
    </script>
</body>
</html>
