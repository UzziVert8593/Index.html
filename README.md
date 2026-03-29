<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>The Controller Lab | Every Controller Deserves a Respawn</title>
    <link href="https://fonts.googleapis.com/css2?family=Orbitron:wght@400;700;900&family=Rajdhani:wght@300;500;700&family=Bebas+Neue&display=swap" rel="stylesheet">
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        :root {
            --primary-red: #E60012;
            --dark-red: #8B0000;
            --black: #000000;
            --dark-gray: #0a0a0a;
            --silver: #C0C0C0;
            --chrome: linear-gradient(135deg, #e0e0e0 0%, #ffffff 50%, #a0a0a0 100%);
        }

        html {
            scroll-behavior: smooth;
        }

        body {
            font-family: 'Rajdhani', sans-serif;
            background: var(--black);
            color: #fff;
            overflow-x: hidden;
        }

        /* CINEMATIC LETTERBOX */
        .letterbox {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            pointer-events: none;
            z-index: 1000;
        }

        .letterbox-top, .letterbox-bottom {
            position: absolute;
            width: 100%;
            height: 8vh;
            background: var(--black);
            transition: height 1s ease;
        }

        .letterbox-top { top: 0; }
        .letterbox-bottom { bottom: 0; }

        /* SCENE 1: THE VOID */
        #scene-void {
            height: 100vh;
            background: var(--black);
            display: flex;
            align-items: center;
            justify-content: center;
            position: relative;
            overflow: hidden;
        }

        .breathing-text {
            font-family: 'Orbitron', sans-serif;
            font-size: 1.5rem;
            letter-spacing: 1rem;
            color: var(--primary-red);
            opacity: 0;
            animation: breathe 4s ease-in-out infinite;
            text-transform: uppercase;
        }

        @keyframes breathe {
            0%, 100% { opacity: 0.1; transform: scale(0.95); }
            50% { opacity: 0.4; transform: scale(1.05); }
        }

        .electrical-hum {
            position: absolute;
            width: 100%;
            height: 100%;
            background: radial-gradient(circle at center, transparent 0%, var(--black) 70%);
            animation: pulse-hum 3s ease-in-out infinite;
        }

        @keyframes pulse-hum {
            0%, 100% { opacity: 0.3; }
            50% { opacity: 0.6; }
        }

        /* SCENE 2: FIRST BREATH */
        #scene-first-breath {
            height: 100vh;
            background: radial-gradient(ellipse at center, #1a0000 0%, var(--black) 70%);
            display: flex;
            align-items: center;
            justify-content: center;
            position: relative;
            overflow: hidden;
        }

        .controller-reveal {
            position: relative;
            width: 600px;
            height: 400px;
            perspective: 1000px;
        }

        .controller-3d {
            width: 100%;
            height: 100%;
            background: url('data:image/svg+xml,<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 600 400"><defs><linearGradient id="redShell" x1="0%" y1="0%" x2="100%" y2="100%"><stop offset="0%" style="stop-color:%23ff1a1a;stop-opacity:0.9" /><stop offset="50%" style="stop-color:%23cc0000;stop-opacity:0.8" /><stop offset="100%" style="stop-color:%238b0000;stop-opacity:0.9" /></linearGradient></defs><rect x="100" y="50" width="400" height="300" rx="60" fill="url(%23redShell)" opacity="0.3"/><circle cx="300" cy="150" r="30" fill="%23fff" opacity="0.9"/><path d="M285 150 L300 135 L315 150 M285 150 L300 165 L315 150" stroke="%23E60012" stroke-width="4" fill="none"/></svg>') center/contain no-repeat;
            animation: float-controller 6s ease-in-out infinite, rotate-controller 20s linear infinite;
            filter: drop-shadow(0 0 50px var(--primary-red));
        }

        @keyframes float-controller {
            0%, 100% { transform: translateY(0) rotateX(10deg); }
            50% { transform: translateY(-30px) rotateX(15deg); }
        }

        @keyframes rotate-controller {
            0% { transform: rotateY(0deg); }
            100% { transform: rotateY(360deg); }
        }

        .xbox-glow {
            position: absolute;
            top: 35%;
            left: 50%;
            transform: translate(-50%, -50%);
            width: 80px;
            height: 80px;
            background: radial-gradient(circle, #fff 0%, var(--primary-red) 40%, transparent 70%);
            border-radius: 50%;
            animation: glow-pulse 2s ease-in-out infinite;
            box-shadow: 0 0 100px var(--primary-red);
        }

        @keyframes glow-pulse {
            0%, 100% { opacity: 0.8; transform: translate(-50%, -50%) scale(1); }
            50% { opacity: 1; transform: translate(-50%, -50%) scale(1.2); }
        }

        .power-on-sound {
            position: absolute;
            bottom: 10%;
            font-family: 'Orbitron', monospace;
            font-size: 0.8rem;
            color: var(--primary-red);
            letter-spacing: 0.5rem;
            animation: sound-wave 0.5s ease-out;
        }

        /* SCENE 3: THE REVEAL SEQUENCE */
        #scene-reveal {
            min-height: 100vh;
            background: var(--dark-gray);
            padding: 5rem 2rem;
            position: relative;
            overflow: hidden;
        }

        .scene-title {
            text-align: center;
            font-family: 'Bebas Neue', sans-serif;
            font-size: 4rem;
            letter-spacing: 1rem;
            margin-bottom: 4rem;
            background: linear-gradient(to right, var(--primary-red), #ff6666);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            animation: title-reveal 2s ease-out;
        }

        @keyframes title-reveal {
            from { clip-path: inset(0 100% 0 0); }
            to { clip-path: inset(0 0 0 0); }
        }

        .controllers-showcase {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
            gap: 3rem;
            max-width: 1400px;
            margin: 0 auto;
        }

        .controller-card {
            background: linear-gradient(145deg, #111 0%, #222 100%);
            border-radius: 20px;
            padding: 2rem;
            text-align: center;
            position: relative;
            transform-style: preserve-3d;
            transition: transform 0.5s ease, box-shadow 0.5s ease;
            border: 1px solid rgba(230, 0, 18, 0.3);
            overflow: hidden;
        }

        .controller-card::before {
            content: '';
            position: absolute;
            top: -50%;
            left: -50%;
            width: 200%;
            height: 200%;
            background: radial-gradient(circle, rgba(230, 0, 18, 0.1) 0%, transparent 70%);
            opacity: 0;
            transition: opacity 0.5s ease;
        }

        .controller-card:hover::before {
            opacity: 1;
        }

        .controller-card:hover {
            transform: translateY(-20px) rotateX(10deg);
            box-shadow: 0 30px 60px rgba(230, 0, 18, 0.3);
            border-color: var(--primary-red);
        }

        .controller-icon {
            font-size: 6rem;
            margin-bottom: 1rem;
            display: block;
        }

        .controller-name {
            font-family: 'Orbitron', sans-serif;
            font-size: 1.5rem;
            color: var(--primary-red);
            margin-bottom: 0.5rem;
        }

        .controller-desc {
            font-size: 0.9rem;
            color: #888;
            text-transform: uppercase;
            letter-spacing: 0.2rem;
        }

        .orbit-animation {
            position: absolute;
            width: 100%;
            height: 100%;
            top: 0;
            left: 0;
            pointer-events: none;
        }

        .orbit-ring {
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            border: 1px solid rgba(230, 0, 18, 0.2);
            border-radius: 50%;
            animation: orbit-rotate 20s linear infinite;
        }

        .orbit-ring:nth-child(1) { width: 300px; height: 300px; }
        .orbit-ring:nth-child(2) { width: 500px; height: 500px; animation-duration: 30s; animation-direction: reverse; }
        .orbit-ring:nth-child(3) { width: 700px; height: 700px; animation-duration: 40s; }

        @keyframes orbit-rotate {
            from { transform: translate(-50%, -50%) rotate(0deg); }
            to { transform: translate(-50%, -50%) rotate(360deg); }
        }

        /* SCENE 4: THE TRANSFORMATION */
        #scene-transformation {
            min-height: 100vh;
            background: var(--black);
            position: relative;
            padding: 5rem 2rem;
        }

        .glitch-container {
            text-align: center;
            margin-bottom: 4rem;
        }

        .glitch-text {
            font-family: 'Orbitron', sans-serif;
            font-size: 3rem;
            font-weight: 900;
            position: relative;
            color: #fff;
            display: inline-block;
        }

        .glitch-text::before,
        .glitch-text::after {
            content: attr(data-text);
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
        }

        .glitch-text::before {
            animation: glitch-1 2s infinite linear alternate-reverse;
            color: var(--primary-red);
            z-index: -1;
        }

        .glitch-text::after {
            animation: glitch-2 3s infinite linear alternate-reverse;
            color: #00ffff;
            z-index: -2;
        }

        @keyframes glitch-1 {
            0%, 100% { clip-path: inset(0 0 0 0); transform: translate(0); }
            20% { clip-path: inset(20% 0 30% 0); transform: translate(-2px, 2px); }
            40% { clip-path: inset(50% 0 20% 0); transform: translate(2px, -2px); }
            60% { clip-path: inset(10% 0 60% 0); transform: translate(-2px, 0); }
            80% { clip-path: inset(80% 0 5% 0); transform: translate(2px, 2px); }
        }

        @keyframes glitch-2 {
            0%, 100% { clip-path: inset(0 0 0 0); transform: translate(0); }
            20% { clip-path: inset(60% 0 10% 0); transform: translate(2px, -2px); }
            40% { clip-path: inset(30% 0 40% 0); transform: translate(-2px, 2px); }
            60% { clip-path: inset(10% 0 80% 0); transform: translate(2px, 0); }
            80% { clip-path: inset(40% 0 30% 0); transform: translate(-2px, -2px); }
        }

        .repair-showcase {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
            gap: 2rem;
            max-width: 1200px;
            margin: 0 auto;
        }

        .repair-card {
            background: linear-gradient(135deg, #111 0%, #1a1a1a 100%);
            border: 1px solid #333;
            border-radius: 15px;
            padding: 2rem;
            position: relative;
            overflow: hidden;
            transition: all 0.5s ease;
        }

        .repair-card::after {
            content: '';
            position: absolute;
            top: 0;
            left: -100%;
            width: 100%;
            height: 100%;
            background: linear-gradient(90deg, transparent, rgba(230, 0, 18, 0.2), transparent);
            transition: left 0.5s ease;
        }

        .repair-card:hover::after {
            left: 100%;
        }

        .repair-card:hover {
            border-color: var(--primary-red);
            transform: scale(1.02);
        }

        .repair-icon {
            font-size: 3rem;
            margin-bottom: 1rem;
        }

        .repair-title {
            font-family: 'Orbitron', sans-serif;
            font-size: 1.3rem;
            color: var(--primary-red);
            margin-bottom: 0.5rem;
        }

        .repair-desc {
            color: #aaa;
            font-size: 0.95rem;
            line-height: 1.6;
        }

        .particle-effect {
            position: absolute;
            width: 100%;
            height: 100%;
            top: 0;
            left: 0;
            pointer-events: none;
            overflow: hidden;
        }

        .particle {
            position: absolute;
            width: 4px;
            height: 4px;
            background: var(--primary-red);
            border-radius: 50%;
            animation: float-particle 10s infinite;
            opacity: 0.6;
        }

        @keyframes float-particle {
            0%, 100% { transform: translateY(100vh) rotate(0deg); opacity: 0; }
            10% { opacity: 0.6; }
            90% { opacity: 0.6; }
            100% { transform: translateY(-100vh) rotate(720deg); opacity: 0; }
        }

        /* SCENE 5: THE LOGO REVEAL */
        #scene-logo {
            min-height: 100vh;
            background: radial-gradient(ellipse at center, #1a1a1a 0%, var(--black) 70%);
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            position: relative;
            overflow: hidden;
        }

        .logo-container {
            position: relative;
            width: 400px;
            height: 400px;
            animation: logo-rotate 20s linear infinite;
        }

        @keyframes logo-rotate {
            from { transform: rotate(0deg); }
            to { transform: rotate(360deg); }
        }

        .logo-ring {
            position: absolute;
            border: 3px solid transparent;
            border-top-color: var(--silver);
            border-radius: 50%;
            animation: ring-spin 10s linear infinite;
        }

        .logo-ring:nth-child(1) { width: 100%; height: 100%; top: 0; left: 0; }
        .logo-ring:nth-child(2) { width: 80%; height: 80%; top: 10%; left: 10%; animation-direction: reverse; animation-duration: 15s; border-top-color: var(--primary-red); }
        .logo-ring:nth-child(3) { width: 60%; height: 60%; top: 20%; left: 20%; animation-duration: 8s; border-top-color: #666; }

        @keyframes ring-spin {
            from { transform: rotate(0deg); }
            to { transform: rotate(360deg); }
        }

        .logo-center {
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            text-align: center;
            animation: logo-pulse 3s ease-in-out infinite;
        }

        @keyframes logo-pulse {
            0%, 100% { transform: translate(-50%, -50%) scale(1); }
            50% { transform: translate(-50%, -50%) scale(1.1); }
        }

        .tcl-logo {
            font-family: 'Orbitron', sans-serif;
            font-size: 4rem;
            font-weight: 900;
            background: var(--chrome);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            text-shadow: 0 0 30px rgba(255,255,255,0.3);
            letter-spacing: 0.5rem;
        }

        .anchor-symbol {
            font-size: 3rem;
            color: var(--silver);
            display: block;
            margin: 1rem 0;
        }

        .est-date {
            font-family: 'Rajdhani', sans-serif;
            font-size: 1rem;
            color: #666;
            letter-spacing: 0.5rem;
            border: 1px solid #444;
            padding: 0.5rem 1rem;
            display: inline-block;
            margin-bottom: 2rem;
        }

        .slogan {
            font-family: 'Bebas Neue', sans-serif;
            font-size: 3rem;
            letter-spacing: 0.8rem;
            color: var(--primary-red);
            text-align: center;
            margin-top: 3rem;
            animation: slogan-glow 3s ease-in-out infinite;
            text-transform: uppercase;
        }

        @keyframes slogan-glow {
            0%, 100% { text-shadow: 0 0 20px var(--primary-red); }
            50% { text-shadow: 0 0 50px var(--primary-red), 0 0 100px var(--dark-red); }
        }

        /* SCENE 6: PRICE LIST */
        #scene-prices {
            min-height: 100vh;
            background: var(--dark-gray);
            padding: 5rem 2rem;
            position: relative;
        }

        .holographic-container {
            max-width: 900px;
            margin: 0 auto;
            background: linear-gradient(135deg, rgba(10,10,10,0.9) 0%, rgba(20,20,20,0.9) 100%);
            border: 2px solid var(--primary-red);
            border-radius: 20px;
            padding: 3rem;
            position: relative;
            overflow: hidden;
            box-shadow: 0 0 50px rgba(230, 0, 18, 0.2);
        }

        .holographic-container::before {
            content: '';
            position: absolute;
            top: -2px;
            left: -2px;
            right: -2px;
            bottom: -2px;
            background: linear-gradient(45deg, var(--primary-red), transparent, var(--primary-red));
            z-index: -1;
            border-radius: 20px;
            animation: holographic-shift 3s linear infinite;
        }

        @keyframes holographic-shift {
            0%, 100% { opacity: 0.5; }
            50% { opacity: 1; }
        }

        .price-header {
            text-align: center;
            margin-bottom: 3rem;
        }

        .price-title {
            font-family: 'Orbitron', sans-serif;
            font-size: 2.5rem;
            color: var(--primary-red);
            margin-bottom: 0.5rem;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 1rem;
        }

        .price-category {
            margin-bottom: 2.5rem;
            border-left: 4px solid var(--primary-red);
            padding-left: 1.5rem;
        }

        .category-title {
            font-family: 'Orbitron', sans-serif;
            font-size: 1.3rem;
            color: #fff;
            margin-bottom: 1rem;
            text-transform: uppercase;
            letter-spacing: 0.2rem;
        }

        .price-item {
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 0.8rem 0;
            border-bottom: 1px solid #333;
            transition: all 0.3s ease;
        }

        .price-item:hover {
            background: rgba(230, 0, 18, 0.1);
            padding-left: 1rem;
            margin-left: -1rem;
        }

        .price-name {
            color: #ccc;
            font-size: 1.1rem;
        }

        .price-value {
            font-family: 'Orbitron', sans-serif;
            font-size: 1.5rem;
            color: var(--primary-red);
            font-weight: 700;
            animation: price-count 2s ease-out;
        }

        @keyframes price-count {
            from { opacity: 0; transform: translateX(20px); }
            to { opacity: 1; transform: translateX(0); }
        }

        .price-note {
            text-align: center;
            margin-top: 3rem;
            padding: 2rem;
            background: rgba(230, 0, 18, 0.1);
            border-radius: 10px;
            border: 1px solid rgba(230, 0, 18, 0.3);
        }

        .price-note p {
            color: #aaa;
            font-size: 1rem;
            line-height: 1.6;
        }

        .contact-highlight {
            color: var(--primary-red);
            font-weight: 700;
            font-size: 1.2rem;
        }

        /* SCENE 7: CALL TO ACTION */
        #scene-cta {
            min-height: 100vh;
            background: var(--black);
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            padding: 5rem 2rem;
            position: relative;
            overflow: hidden;
        }

        .final-controller {
            width: 300px;
            height: 200px;
            background: radial-gradient(ellipse at center, var(--primary-red) 0%, transparent 70%);
            border-radius: 50%;
            animation: final-pulse 3s ease-in-out infinite;
            margin-bottom: 3rem;
            position: relative;
        }

        .final-controller::after {
            content: '✕';
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            font-size: 4rem;
            color: #fff;
            font-family: 'Orbitron', sans-serif;
            animation: button-glow 2s ease-in-out infinite;
        }

        @keyframes final-pulse {
            0%, 100% { transform: scale(1); opacity: 0.8; }
            50% { transform: scale(1.2); opacity: 1; }
        }

        @keyframes button-glow {
            0%, 100% { text-shadow: 0 0 20px #fff; }
            50% { text-shadow: 0 0 50px #fff, 0 0 100px var(--primary-red); }
        }

        .contact-card {
            text-align: center;
            background: linear-gradient(145deg, #111 0%, #222 100%);
            padding: 3rem;
            border-radius: 20px;
            border: 2px solid var(--primary-red);
            max-width: 600px;
            width: 100%;
            animation: card-float 4s ease-in-out infinite;
        }

        @keyframes card-float {
            0%, 100% { transform: translateY(0); }
            50% { transform: translateY(-10px); }
        }

        .contact-name {
            font-family: 'Bebas Neue', sans-serif;
            font-size: 3rem;
            letter-spacing: 0.5rem;
            color: #fff;
            margin-bottom: 0.5rem;
        }

        .contact-role {
            color: var(--primary-red);
            font-size: 1.2rem;
            letter-spacing: 0.3rem;
            text-transform: uppercase;
            margin-bottom: 2rem;
        }

        .phone-number {
            font-family: 'Orbitron', sans-serif;
            font-size: 2.5rem;
            color: var(--primary-red);
            text-decoration: none;
            display: block;
            margin-bottom: 2rem;
            transition: all 0.3s ease;
            animation: number-pulse 2s ease-in-out infinite;
        }

        @keyframes number-pulse {
            0%, 100% { text-shadow: 0 0 10px var(--primary-red); }
            50% { text-shadow: 0 0 30px var(--primary-red), 0 0 60px var(--dark-red); }
        }

        .phone-number:hover {
            color: #fff;
            transform: scale(1.05);
        }

        .social-links {
            display: flex;
            gap: 2rem;
            justify-content: center;
            margin-top: 2rem;
        }

        .social-link {
            width: 50px;
            height: 50px;
            border: 2px solid var(--primary-red);
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            color: var(--primary-red);
            text-decoration: none;
            font-size: 1.5rem;
            transition: all 0.3s ease;
        }

        .social-link:hover {
            background: var(--primary-red);
            color: #fff;
            transform: translateY(-5px);
            box-shadow: 0 10px 30px rgba(230, 0, 18, 0.4);
        }

        /* NAVIGATION */
        .nav-dots {
            position: fixed;
            right: 2rem;
            top: 50%;
            transform: translateY(-50%);
            z-index: 100;
            display: flex;
            flex-direction: column;
            gap: 1rem;
        }

        .nav-dot {
            width: 12px;
            height: 12px;
            border-radius: 50%;
            background: #333;
            cursor: pointer;
            transition: all 0.3s ease;
            border: 2px solid transparent;
        }

        .nav-dot.active {
            background: var(--primary-red);
            border-color: #fff;
            transform: scale(1.3);
        }

        /* AUDIO VISUALIZER */
        .audio-viz {
            position: fixed;
            bottom: 10%;
            left: 50%;
            transform: translateX(-50%);
            display: flex;
            gap: 5px;
            opacity: 0.5;
            z-index: 50;
        }

        .viz-bar {
            width: 4px;
            height: 20px;
            background: var(--primary-red);
            animation: viz-dance 0.5s ease-in-out infinite;
        }

        .viz-bar:nth-child(1) { animation-delay: 0s; }
        .viz-bar:nth-child(2) { animation-delay: 0.1s; }
        .viz-bar:nth-child(3) { animation-delay: 0.2s; }
        .viz-bar:nth-child(4) { animation-delay: 0.3s; }
        .viz-bar:nth-child(5) { animation-delay: 0.4s; }

        @keyframes viz-dance {
            0%, 100% { height: 20px; }
            50% { height: 50px; }
        }

        /* RESPONSIVE */
        @media (max-width: 768px) {
            .scene-title { font-size: 2.5rem; letter-spacing: 0.5rem; }
            .slogan { font-size: 1.8rem; letter-spacing: 0.3rem; }
            .tcl-logo { font-size: 2.5rem; }
            .contact-name { font-size: 2rem; }
            .phone-number { font-size: 1.8rem; }
            .holographic-container { padding: 1.5rem; }
            .nav-dots { right: 1rem; }
        }

        /* SCROLL PROGRESS */
        .progress-bar {
            position: fixed;
            top: 0;
            left: 0;
            height: 4px;
            background: var(--primary-red);
            z-index: 1001;
            transition: width 0.1s ease;
        }

        /* LOADING SCREEN */
        #loader {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: var(--black);
            display: flex;
            align-items: center;
            justify-content: center;
            z-index: 9999;
            transition: opacity 1s ease, visibility 1s ease;
        }

        #loader.hidden {
            opacity: 0;
            visibility: hidden;
        }

        .loader-text {
            font-family: 'Orbitron', sans-serif;
            font-size: 2rem;
            color: var(--primary-red);
            letter-spacing: 1rem;
            animation: loader-pulse 1.5s ease-in-out infinite;
        }

        @keyframes loader-pulse {
            0%, 100% { opacity: 0.3; }
            50% { opacity: 1; }
        }
    </style>
</head>
<body>

    <!-- LOADER -->
    <div id="loader">
        <div class="loader-text">INITIALIZING...</div>
    </div>

    <!-- PROGRESS BAR -->
    <div class="progress-bar" id="progressBar"></div>

    <!-- LETTERBOX -->
    <div class="letterbox">
        <div class="letterbox-top"></div>
        <div class="letterbox-bottom"></div>
    </div>

    <!-- NAVIGATION DOTS -->
    <div class="nav-dots">
        <div class="nav-dot active" onclick="scrollToSection('scene-void')"></div>
        <div class="nav-dot" onclick="scrollToSection('scene-first-breath')"></div>
        <div class="nav-dot" onclick="scrollToSection('scene-reveal')"></div>
        <div class="nav-dot" onclick="scrollToSection('scene-transformation')"></div>
        <div class="nav-dot" onclick="scrollToSection('scene-logo')"></div>
        <div class="nav-dot" onclick="scrollToSection('scene-prices')"></div>
        <div class="nav-dot" onclick="scrollToSection('scene-cta')"></div>
    </div>

    <!-- AUDIO VISUALIZER -->
    <div class="audio-viz">
        <div class="viz-bar"></div>
        <div class="viz-bar"></div>
        <div class="viz-bar"></div>
        <div class="viz-bar"></div>
        <div class="viz-bar"></div>
    </div>

    <!-- SCENE 1: THE VOID -->
    <section id="scene-void">
        <div class="electrical-hum"></div>
        <div class="breathing-text">System Standby</div>
    </section>

    <!-- SCENE 2: FIRST BREATH -->
    <section id="scene-first-breath">
        <div class="controller-reveal">
            <div class="controller-3d"></div>
            <div class="xbox-glow"></div>
        </div>
        <div class="power-on-sound">[ POWER ON SEQUENCE ]</div>
    </section>

    <!-- SCENE 3: THE REVEAL SEQUENCE -->
    <section id="scene-reveal">
        <div class="orbit-animation">
            <div class="orbit-ring"></div>
            <div class="orbit-ring"></div>
            <div class="orbit-ring"></div>
        </div>
        
        <h2 class="scene-title">The Controllers</h2>
        
        <div class="controllers-showcase">
            <div class="controller-card">
                <span class="controller-icon">🎮</span>
                <h3 class="controller-name">Xbox Series S/X</h3>
                <p class="controller-desc">Red Transparent Edition</p>
            </div>
            
            <div class="controller-card">
                <span class="controller-icon">🎮</span>
                <h3 class="controller-name">Xbox One</h3>
                <p class="controller-desc">Classic Black</p>
            </div>
            
            <div class="controller-card">
                <span class="controller-icon">🎮</span>
                <h3 class="controller-name">PlayStation 5</h3>
                <p class="controller-desc">DualSense White</p>
            </div>
            
            <div class="controller-card">
                <span class="controller-icon">🎮</span>
                <h3 class="controller-name">PlayStation 4</h3>
                <p class="controller-desc">DualShock Black</p>
            </div>
        </div>
    </section>

    <!-- SCENE 4: THE TRANSFORMATION -->
    <section id="scene-transformation">
        <div class="particle-effect" id="particles"></div>
        
        <div class="glitch-container">
            <h2 class="glitch-text" data-text="THE TRANSFORMATION">THE TRANSFORMATION</h2>
        </div>
        
        <div class="repair-showcase">
            <div class="repair-card">
                <div class="repair-icon">🕹️</div>
                <h3 class="repair-title">STICK DRIFT?</h3>
                <p class="repair-desc">Analog stick dissolves and reforms pristine. Precision calibration restores dead zones and eliminates unwanted movement.</p>
            </div>
            
            <div class="repair-card">
                <div class="repair-icon">🔌</div>
                <h3 class="repair-title">BROKEN PORT?</h3>
                <p class="repair-desc">Charging port sparks, then glows clean. Full replacement of damaged aux and charging connections.</p>
            </div>
            
            <div class="repair-card">
                <div class="repair-icon">✨</div>
                <h3 class="repair-title">WORN OUT?</h3>
                <p class="repair-desc">Controller shell transforms from scuffed to pristine. Deep cleaning and housing replacement available.</p>
            </div>
            
            <div class="repair-card">
                <div class="repair-icon">⚡</div>
                <h3 class="repair-title">DEEP CLEAN</h3>
                <p class="repair-desc">Complete disassembly and ultrasonic cleaning. Remove dust, grime, and restore that new controller feel.</p>
            </div>
        </div>
    </section>

    <!-- SCENE 5: THE LOGO REVEAL -->
    <section id="scene-logo">
        <div class="est-date">EST. 2026</div>
        
        <div class="logo-container">
            <div class="logo-ring"></div>
            <div class="logo-ring"></div>
            <div class="logo-ring"></div>
            
            <div class="logo-center">
                <div class="tcl-logo">TCL</div>
                <span class="anchor-symbol">⚓</span>
                <div style="font-family: 'Orbitron'; font-size: 1.2rem; color: #888; letter-spacing: 0.3;
        }

        .price-name {
            color: #ccc;
            font-size: 1.1rem;
        }

        .price-value {
            font-family: 'Orbitron', sans-serif;
            font-size: 1.5rem;
            color: var(--primary-red);
            font-weight: 700;
            animation: price-count 2s ease-out;
        }

        @keyframes price-count {
            from { opacity: 0; transform: translateX(20px); }
            to { opacity: 1; transform: translateX(0); }
        }

        .price-note {
            text-align: center;
            margin-top: 3rem;
            padding: 2rem;
            background: rgba(230, 0, 18, 0.1);
            border-radius: 10px;
            border: 1px solid rgba(230, 0, 18, 0.3);
        }

        .price-note p {
            color: #aaa;
            font-size: 1rem;
            line-height: 1.6;
        }

        .contact-highlight {
            color: var(--primary-red);
            font-weight: 700;
            font-size: 1.2rem;
        }

        /* SCENE 7: CALL TO ACTION */
        #scene-cta {
            min-height: 100vh;
            background: var(--black);
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            padding: 5rem 2rem;
            position: relative;
            overflow: hidden;
        }

        .final-controller {
            width: 300px;
            height: 200px;
            background: radial-gradient(ellipse at center, var(--primary-red) 0%, transparent 70%);
            border-radius: 50%;
            animation: final-pulse 3s ease-in-out infinite;
            margin-bottom: 3rem;
            position: relative;
        }

        .final-controller::after {
            content: '✕';
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            font-size: 4rem;
            color: #fff;
            font-family: 'Orbitron', sans-serif;
            animation: button-glow 2s ease-in-out infinite;
        }

        @keyframes final-pulse {
            0%, 100% { transform: scale(1); opacity: 0.8; }
            50% { transform: scale(1.2); opacity: 1; }
        }

        @keyframes button-glow {
            0%, 100% { text-shadow: 0 0 20px #fff; }
            50% { text-shadow: 0 0 50px #fff, 0 0 100px var(--primary-red); }
        }

        .contact-card {
            text-align: center;
            background: linear-gradient(145deg, #111 0%, #222 100%);
            padding: 3rem;
            border-radius: 20px;
            border: 2px solid var(--primary-red);
            max-width: 600px;
            width: 100%;
            animation: card-float 4s ease-in-out infinite;
        }

        @keyframes card-float {
            0%, 100% { transform: translateY(0); }
            50% { transform: translateY(-10px); }
        }

        .contact-name {
            font-family: 'Bebas Neue', sans-serif;
            font-size: 3rem;
            letter-spacing: 0.5rem;
            color: #fff;
            margin-bottom: 0.5rem;
        }

        .contact-role {
            color: var(--primary-red);
            font-size: 1.2rem;
            letter-spacing: 0.3rem;
            text-transform: uppercase;
            margin-bottom: 2rem;
        }

        .phone-number {
            font-family: 'Orbitron', sans-serif;
            font-size: 2.5rem;
            color: var(--primary-red);
            text-decoration: none;
            display: block;
            margin-bottom: 2rem;
            transition: all 0.3s ease;
            animation: number-pulse 2s ease-in-out infinite;
        }

        @keyframes number-pulse {
            0%, 100% { text-shadow: 0 0 10px var(--primary-red); }
            50% { text-shadow: 0 0 30px var(--primary-red), 0 0 60px var(--dark-red); }
        }

        .phone-number:hover {
            color: #fff;
            transform: scale(1.05);
        }

        .social-links {
            display: flex;
            gap: 2rem;
            justify-content: center;
            margin-top: 2rem;
        }

        .social-link {
            width: 50px;
            height: 50px;
            border: 2px solid var(--primary-red);
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            color: var(--primary-red);
            text-decoration: none;
            font-size: 1.5rem;
            transition: all 0.3s ease;
        }

        .social-link:hover {
            background: var(--primary-red);
            color: #fff;
            transform: translateY(-5px);
            box-shadow: 0 10px 30px rgba(230, 0, 18, 0.4);
        }

        /* NAVIGATION */
        .nav-dots {
            position: fixed;
            right: 2rem;
            top: 50%;
            transform: translateY(-50%);
            z-index: 100;
            display: flex;
            flex-direction: column;
            gap: 1rem;
        }

        .nav-dot {
            width: 12px;
            height: 12px;
            border-radius: 50%;
            background: #333;
            cursor: pointer;
            transition: all 0.3s ease;
            border: 2px solid transparent;
        }

        .nav-dot.active {
            background: var(--primary-red);
            border-color: #fff;
            transform: scale(1.3);
        }

        /* AUDIO VISUALIZER */
        .audio-viz {
            position: fixed;
            bottom: 10%;
            left: 50%;
            transform: translateX(-50%);
            display: flex;
            gap: 5px;
            opacity: 0.5;
            z-index: 50;
        }

        .viz-bar {
            width: 4px;
            height: 20px;
            background: var(--primary-red);
            animation: viz-dance 0.5s ease-in-out infinite;
        }

        .viz-bar:nth-child(1) { animation-delay: 0s; }
        .viz-bar:nth-child(2) { animation-delay: 0.1s; }
        .viz-bar:nth-child(3) { animation-delay: 0.2s; }
        .viz-bar:nth-child(4) { animation-delay: 0.3s; }
        .viz-bar:nth-child(5) { animation-delay: 0.4s; }

        @keyframes viz-dance {
            0%, 100% { height: 20px; }
            50% { height: 50px; }
        }

        /* RESPONSIVE */
        @media (max-width: 768px) {
            .scene-title { font-size: 2.5rem; letter-spacing: 0.5rem; }
            .slogan { font-size: 1.8rem; letter-spacing: 0.3rem; }
            .tcl-logo { font-size: 2.5rem; }
            .contact-name { font-size: 2rem; }
            .phone-number { font-size: 1.8rem; }
            .holographic-container { padding: 1.5rem; }
            .nav-dots { right: 1rem; }
        }

        /* SCROLL PROGRESS */
        .progress-bar {
            position: fixed;
            top: 0;
            left: 0;
            height: 4px;
            background: var(--primary-red);
            z-index: 1001;
            transition: width 0.1s ease;
        }

        /* LOADING SCREEN */
        #loader {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: var(--black);
            display: flex;
            align-items: center;
            justify-content: center;
            z-index: 9999;
            transition: opacity 1s ease, visibility 1s ease;
        }

        #loader.hidden {
            opacity: 0;
            visibility: hidden;
        }

        .loader-text {
            font-family: 'Orbitron', sans-serif;
            font-size: 2rem;
            color: var(--primary-red);
            letter-spacing: 1rem;
            animation: loader-pulse 1.5s ease-in-out infinite;
        }

        @keyframes loader-pulse {
            0%, 100% { opacity: 0.3; }
            50% { opacity: 1; }
        }
    </style>
</head>
<body>

    <!-- LOADER -->
    <div id="loader">
        <div class="loader-text">INITIALIZING...</div>
    </div>

    <!-- PROGRESS BAR -->
    <div class="progress-bar" id="progressBar"></div>

    <!-- LETTERBOX -->
    <div class="letterbox">
        <div class="letterbox-top"></div>
        <div class="letterbox-bottom"></div>
    </div>

    <!-- NAVIGATION DOTS -->
    <div class="nav-dots">
        <div class="nav-dot active" onclick="scrollToSection('scene-void')"></div>
        <div class="nav-dot" onclick="scrollToSection('scene-first-breath')"></div>
        <div class="nav-dot" onclick="scrollToSection('scene-reveal')"></div>
        <div class="nav-dot" onclick="scrollToSection('scene-transformation')"></div>
        <div class="nav-dot" onclick="scrollToSection('scene-logo')"></div>
        <div class="nav-dot" onclick="scrollToSection('scene-prices')"></div>
        <div class="nav-dot" onclick="scrollToSection('scene-cta')"></div>
    </div>

    <!-- AUDIO VISUALIZER -->
    <div class="audio-viz">
        <div class="viz-bar"></div>
        <div class="viz-bar"></div>
        <div class="viz-bar"></div>
        <div class="viz-bar"></div>
        <div class="viz-bar"></div>
    </div>

    <!-- SCENE 1: THE VOID -->
    <section id="scene-void">
        <div class="electrical-hum"></div>
        <div class="breathing-text">System Standby</div>
    </section>

    <!-- SCENE 2: FIRST BREATH -->
    <section id="scene-first-breath">
        <div class="controller-reveal">
            <div class="controller-3d"></div>
            <div class="xbox-glow"></div>
        </div>
        <div class="power-on-sound">[ POWER ON SEQUENCE ]</div>
    </section>

    <!-- SCENE 3: THE REVEAL SEQUENCE -->
    <section id="scene-reveal">
        <div class="orbit-animation">
            <div class="orbit-ring"></div>
            <div class="orbit-ring"></div>
            <div class="orbit-ring"></div>
        </div>
        
        <h2 class="scene-title">The Controllers</h2>
        
        <div class="controllers-showcase">
            <div class="controller-card">
                <span class="controller-icon">🎮</span>
                <h3 class="controller-name">Xbox Series S/X</h3>
                <p class="controller-desc">Red Transparent Edition</p>
            </div>
            
            <div class="controller-card">
                <span class="controller-icon">🎮</span>
                <h3 class="controller-name">Xbox One</h3>
                <p class="controller-desc">Classic Black</p>
            </div>
            
            <div class="controller-card">
                <span class="controller-icon">🎮</span>
                <h3 class="controller-name">PlayStation 5</h3>
                <p class="controller-desc">DualSense White</p>
            </div>
            
            <div class="controller-card">
                <span class="controller-icon">🎮</span>
                <h3 class="controller-name">PlayStation 4</h3>
                <p class="controller-desc">DualShock Black</p>
            </div>
        </div>
    </section>

    <!-- SCENE 4: THE TRANSFORMATION -->
    <section id="scene-transformation">
        <div class="particle-effect" id="particles"></div>
        
        <div class="glitch-container">
            <h2 class="glitch-text" data-text="THE TRANSFORMATION">THE TRANSFORMATION</h2>
        </div>
        
        <div class="repair-showcase">
            <div class="repair-card">
                <div class="repair-icon">🕹️</div>
                <h3 class="repair-title">STICK DRIFT?</h3>
                <p class="repair-desc">Analog stick dissolves and reforms pristine. Precision calibration restores dead zones and eliminates unwanted movement.</p>
            </div>
            
            <div class="repair-card">
                <div class="repair-icon">🔌</div>
                <h3 class="repair-title">BROKEN PORT?</h3>
                <p class="repair-desc">Charging port sparks, then glows clean. Full replacement of damaged aux and charging connections.</p>
            </div>
            
            <div class="repair-card">
                <div class="repair-icon">✨</div>
                <h3 class="repair-title">WORN OUT?</h3>
                <p class="repair-desc">Controller shell transforms from scuffed to pristine. Deep cleaning and housing replacement available.</p>
            </div>
            
            <div class="repair-card">
                <div class="repair-icon">⚡</div>
                <h3 class="repair-title">DEEP CLEAN</h3>
                <p class="repair-desc">Complete disassembly and ultrasonic cleaning. Remove dust, grime, and restore that new controller feel.</p>
            </div>
        </div>
    </section>

    <!-- SCENE 5: THE LOGO REVEAL -->
    <section id="scene-logo">
        <div class="est-date">EST. 2026</div>
        
        <div class="logo-container">
            <div class="logo-ring"></div>
            <div class="logo-ring"></div>
            <div class="logo-ring"></div>
            
            <div class="logo-center">
                <div class="tcl-logo">TCL</div>
                <span class="anchor-symbol">⚓</span>
                <div style="font-family: 'Orbitron'; font-size: 1.2rem; color: #888; letter-spacing: 0.3rem;">THE CONTROLLER LAB</div>
            </div>
        </div>
        
        <h2 class="slogan">Every Controller Deserves a Respawn</h2>
    </section>

    <!-- SCENE 6: PRICE LIST -->
    <section id="scene-prices">
        <div class="holographic-container">
            <div class="price-header">
                <h2 class="price-title">
                    <span>🎮</span>
                    PRICE LIST
                    <span>🎮</span>
                </h2>
                <p style="color: #666; letter-spacing: 0.2rem;">HOLOGRAPHIC DISPLAY // EST. 2026</p>
            </div>
            
            <div class="price-category">
                <h3 class="category-title">Stick Drift Repair</h3>
                <div class="price-item">
                    <span class="price-name">Standard Analog Stick</span>
                    <span class="price-value">R120</span>
                </div>
                <div class="price-item">
                    <span class="price-name">Hall Effect Analog Stick</span>
                    <span class="price-value">R180</span>
                </div>
            </div>
            
            <div class="price-category">
                <h3 class="category-title">Components Replacement</h3>
                <div class="price-item">
                    <span class="price-name">Xbox One/Series S/X Aux Jack</span>
                    <span class="price-value">R50</span>
                </div>
                <div class="price-item">
                    <span class="price-name">Charging Port</span>
                    <span class="price-value">R120</span>
                </div>
            </div>
            
            <div class="price-category">
                <h3 class="category-title">Housing & Cleaning</h3>
                <div class="price-item">
                    <span class="price-name">Xbox Series S/X Controller Housing</span>
                    <span class="price-value">R250</span>
                </div>
                <div class="price-item">
                    <span class="price-name">Deep Clean (Per Controller)</span>
                    <span class="price-value">R50</span>
                </div>
            </div>
            
            <div class="price-note">
                <p>For all other controller issues/upgrades, please contact me and I'll be able to see what can be done at a <span class="contact-highlight">reasonable price</span>.</p>
            </div>
        </div>
    </section>

    <!-- SCENE 7: CALL TO ACTION -->
    <section id="scene-cta">
        <div class="final-controller"></div>
        
        <div class="contact-card">
            <h2 class="contact-name">CONTACT UZAIR</h2>
            <p class="contact-role">Master Technician</p>
            
            <a href="tel:0671237816" class="phone-number">067 123 7816</a>
            
            <p style="color: #666; margin-bottom: 2rem; font-size: 0.9rem; letter-spacing: 0.1rem;">
                WHATSAPP AVAILABLE • SOUTH AFRICA
            </p>
            
            <div class="social-links">
                <a href="#" class="social-link" title="Instagram">📷</a>
                <a href="#" class="social-link" title="Facebook">f</a>
                <a href="#" class="social-link" title="WhatsApp">📱</a>
            </div>
        </div>
        
        <p style="margin-top: 3rem; color: #444; font-size: 0.8rem; letter-spacing: 0.2rem;">
            © 2026 THE CONTROLLER LAB • EVERY CONTROLLER DESERVES A RESPAWN
        </p>
    </section>

    <script>
        // LOADER
        window.addEventListener('load', () => {
            setTimeout(() => {
                document.getElementById('loader').classList.add('hidden');
            }, 1500);
        });

        // SCROLL PROGRESS
        window.addEventListener('scroll', () => {
            const winScroll = document.body.scrollTop || document.documentElement.scrollTop;
            const height = document.documentElement.scrollHeight - document.documentElement.clientHeight;
            const scrolled = (winScroll / height) * 100;
            document.getElementById('progressBar').style.width = scrolled + '%';
            
            // Update nav dots
            updateNavDots();
        });

        // NAVIGATION
        function scrollToSection(id) {
            document.getElementById(id).scrollIntoView({ behavior: 'smooth' });
        }

        function updateNavDots() {
            const sections = ['scene-void', 'scene-first-breath', 'scene-reveal', 'scene-transformation', 'scene-logo', 'scene-prices', 'scene-cta'];
            const dots = document.querySelectorAll('.nav-dot');
            
            sections.forEach((section, index) => {
                const element = document.getElementById(section);
                const rect = element.getBoundingClientRect();
                
                if (rect.top <= window.innerHeight / 2 && rect.bottom >= window.innerHeight / 2) {
                    dots.forEach(d => d.classList.remove('active'));
                    dots[index].classList.add('active');
                }
            });
        }

        // PARTICLE GENERATION
        function createParticles() {
            const container = document.getElementById('particles');
            for (let i = 0; i < 30; i++) {
                const particle = document.createElement('div');
                particle.className = 'particle';
                particle.style.left = Math.random() * 100 + '%';
                particle.style.animationDelay = Math.random() * 10 + 's';
                particle.style.animationDuration = (10 + Math.random() * 10) + 's';
                container.appendChild(particle);
            }
        }
        createParticles();

        // PARALLAX EFFECT
        window.addEventListener('scroll', () => {
            const scrolled = window.pageYOffset;
            const parallaxElements = document.querySelectorAll('.controller-3d, .logo-container');
            
            parallaxElements.forEach(el => {
                const speed = 0.5;
                el.style.transform = `translateY(${scrolled * speed}px)`;
            });
        });

        // INTERSECTION OBSERVER FOR ANIMATIONS
        const observerOptions = {
            threshold: 0.3,
            rootMargin: '0px'
        };

        const observer = new IntersectionObserver((entries) => {
            entries.forEach(entry => {
                if (entry.isIntersecting) {
                    entry.target.style.opacity = '1';
                    entry.target.style.transform = 'translateY(0)';
                }
            });
        }, observerOptions);

        // Observe all cards
        document.querySelectorAll('.controller-card, .repair-card, .price-item').forEach(el => {
            el.style.opacity = '0';
            el.style.transform = 'translateY(30px)';
            el.style.transition = 'all 0.6s ease';
            observer.observe(el);
        });

        // AUDIO SIMULATION (Visual only)
        setInterval(() => {
            const bars = document.querySelectorAll('.viz-bar');
            bars.forEach(bar => {
                bar.style.height = (20 + Math.random() * 40) + 'px';
            });
        }, 200);

        // KEYBOARD NAVIGATION
        document.addEventListener('keydown', (e) => {
            const sections = ['scene-void', 'scene-first-breath', 'scene-reveal', 'scene-transformation', 'scene;
        }

        .price-name {
            color: #ccc;
            font-size: 1.1rem;
        }

        .price-value {
            font-family: 'Orbitron', sans-serif;
            font-size: 1.5rem;
            color: var(--primary-red);
            font-weight: 700;
            animation: price-count 2s ease-out;
        }

        @keyframes price-count {
            from { opacity: 0; transform: translateX(20px); }
            to { opacity: 1; transform: translateX(0); }
        }

        .price-note {
            text-align: center;
            margin-top: 3rem;
            padding: 2rem;
            background: rgba(230, 0, 18, 0.1);
            border-radius: 10px;
            border: 1px solid rgba(230, 0, 18, 0.3);
        }

        .price-note p {
            color: #aaa;
            font-size: 1rem;
            line-height: 1.6;
        }

        .contact-highlight {
            color: var(--primary-red);
            font-weight: 700;
            font-size: 1.2rem;
        }

        /* SCENE 7: CALL TO ACTION */
        #scene-cta {
            min-height: 100vh;
            background: var(--black);
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            padding: 5rem 2rem;
            position: relative;
            overflow: hidden;
        }

        .final-controller {
            width: 300px;
            height: 200px;
            background: radial-gradient(ellipse at center, var(--primary-red) 0%, transparent 70%);
            border-radius: 50%;
            animation: final-pulse 3s ease-in-out infinite;
            margin-bottom: 3rem;
            position: relative;
        }

        .final-controller::after {
            content: '✕';
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            font-size: 4rem;
            color: #fff;
            font-family: 'Orbitron', sans-serif;
            animation: button-glow 2s ease-in-out infinite;
        }

        @keyframes final-pulse {
            0%, 100% { transform: scale(1); opacity: 0.8; }
            50% { transform: scale(1.2); opacity: 1; }
        }

        @keyframes button-glow {
            0%, 100% { text-shadow: 0 0 20px #fff; }
            50% { text-shadow: 0 0 50px #fff, 0 0 100px var(--primary-red); }
        }

        .contact-card {
            text-align: center;
            background: linear-gradient(145deg, #111 0%, #222 100%);
            padding: 3rem;
            border-radius: 20px;
            border: 2px solid var(--primary-red);
            max-width: 600px;
            width: 100%;
            animation: card-float 4s ease-in-out infinite;
        }

        @keyframes card-float {
            0%, 100% { transform: translateY(0); }
            50% { transform: translateY(-10px); }
        }

        .contact-name {
            font-family: 'Bebas Neue', sans-serif;
            font-size: 3rem;
            letter-spacing: 0.5rem;
            color: #fff;
            margin-bottom: 0.5rem;
        }

        .contact-role {
            color: var(--primary-red);
            font-size: 1.2rem;
            letter-spacing: 0.3rem;
            text-transform: uppercase;
            margin-bottom: 2rem;
        }

        .phone-number {
            font-family: 'Orbitron', sans-serif;
            font-size: 2.5rem;
            color: var(--primary-red);
            text-decoration: none;
            display: block;
            margin-bottom: 2rem;
            transition: all 0.3s ease;
            animation: number-pulse 2s ease-in-out infinite;
        }

        @keyframes number-pulse {
            0%, 100% { text-shadow: 0 0 10px var(--primary-red); }
            50% { text-shadow: 0 0 30px var(--primary-red), 0 0 60px var(--dark-red); }
        }

        .phone-number:hover {
            color: #fff;
            transform: scale(1.05);
        }

        .social-links {
            display: flex;
            gap: 2rem;
            justify-content: center;
            margin-top: 2rem;
        }

        .social-link {
            width: 50px;
            height: 50px;
            border: 2px solid var(--primary-red);
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            color: var(--primary-red);
            text-decoration: none;
            font-size: 1.5rem;
            transition: all 0.3s ease;
        }

        .social-link:hover {
            background: var(--primary-red);
            color: #fff;
            transform: translateY(-5px);
            box-shadow: 0 10px 30px rgba(230, 0, 18, 0.4);
        }

        /* NAVIGATION */
        .nav-dots {
            position: fixed;
            right: 2rem;
            top: 50%;
            transform: translateY(-50%);
            z-index: 100;
            display: flex;
            flex-direction: column;
            gap: 1rem;
        }

        .nav-dot {
            width: 12px;
            height: 12px;
            border-radius: 50%;
            background: #333;
            cursor: pointer;
            transition: all 0.3s ease;
            border: 2px solid transparent;
        }

        .nav-dot.active {
            background: var(--primary-red);
            border-color: #fff;
            transform: scale(1.3);
        }

        /* AUDIO VISUALIZER */
        .audio-viz {
            position: fixed;
            bottom: 10%;
            left: 50%;
            transform: translateX(-50%);
            display: flex;
            gap: 5px;
            opacity: 0.5;
            z-index: 50;
        }

        .viz-bar {
            width: 4px;
            height: 20px;
            background: var(--primary-red);
            animation: viz-dance 0.5s ease-in-out infinite;
        }

        .viz-bar:nth-child(1) { animation-delay: 0s; }
        .viz-bar:nth-child(2) { animation-delay: 0.1s; }
        .viz-bar:nth-child(3) { animation-delay: 0.2s; }
        .viz-bar:nth-child(4) { animation-delay: 0.3s; }
        .viz-bar:nth-child(5) { animation-delay: 0.4s; }

        @keyframes viz-dance {
            0%, 100% { height: 20px; }
            50% { height: 50px; }
        }

        /* RESPONSIVE */
        @media (max-width: 768px) {
            .scene-title { font-size: 2.5rem; letter-spacing: 0.5rem; }
            .slogan { font-size: 1.8rem; letter-spacing: 0.3rem; }
            .tcl-logo { font-size: 2.5rem; }
            .contact-name { font-size: 2rem; }
            .phone-number { font-size: 1.8rem; }
            .holographic-container { padding: 1.5rem; }
            .nav-dots { right: 1rem; }
        }

        /* SCROLL PROGRESS */
        .progress-bar {
            position: fixed;
            top: 0;
            left: 0;
            height: 4px;
            background: var(--primary-red);
            z-index: 1001;
            transition: width 0.1s ease;
        }

        /* LOADING SCREEN */
        #loader {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: var(--black);
            display: flex;
            align-items: center;
            justify-content: center;
            z-index: 9999;
            transition: opacity 1s ease, visibility 1s ease;
        }

        #loader.hidden {
            opacity: 0;
            visibility: hidden;
        }

        .loader-text {
            font-family: 'Orbitron', sans-serif;
            font-size: 2rem;
            color: var(--primary-red);
            letter-spacing: 1rem;
            animation: loader-pulse 1.5s ease-in-out infinite;
        }

        @keyframes loader-pulse {
            0%, 100% { opacity: 0.3; }
            50% { opacity: 1; }
        }
    </style>
</head>
<body>

    <!-- LOADER -->
    <div id="loader">
        <div class="loader-text">INITIALIZING...</div>
    </div>

    <!-- PROGRESS BAR -->
    <div class="progress-bar" id="progressBar"></div>

    <!-- LETTERBOX -->
    <div class="letterbox">
        <div class="letterbox-top"></div>
        <div class="letterbox-bottom"></div>
    </div>

    <!-- NAVIGATION DOTS -->
    <div class="nav-dots">
        <div class="nav-dot active" onclick="scrollToSection('scene-void')"></div>
        <div class="nav-dot" onclick="scrollToSection('scene-first-breath')"></div>
        <div class="nav-dot" onclick="scrollToSection('scene-reveal')"></div>
        <div class="nav-dot" onclick="scrollToSection('scene-transformation')"></div>
        <div class="nav-dot" onclick="scrollToSection('scene-logo')"></div>
        <div class="nav-dot" onclick="scrollToSection('scene-prices')"></div>
        <div class="nav-dot" onclick="scrollToSection('scene-cta')"></div>
    </div>

    <!-- AUDIO VISUALIZER -->
    <div class="audio-viz">
        <div class="viz-bar"></div>
        <div class="viz-bar"></div>
        <div class="viz-bar"></div>
        <div class="viz-bar"></div>
        <div class="viz-bar"></div>
    </div>

    <!-- SCENE 1: THE VOID -->
    <section id="scene-void">
        <div class="electrical-hum"></div>
        <div class="breathing-text">System Standby</div>
    </section>

    <!-- SCENE 2: FIRST BREATH -->
    <section id="scene-first-breath">
        <div class="controller-reveal">
            <div class="controller-3d"></div>
            <div class="xbox-glow"></div>
        </div>
        <div class="power-on-sound">[ POWER ON SEQUENCE ]</div>
    </section>

    <!-- SCENE 3: THE REVEAL SEQUENCE -->
    <section id="scene-reveal">
        <div class="orbit-animation">
            <div class="orbit-ring"></div>
            <div class="orbit-ring"></div>
            <div class="orbit-ring"></div>
        </div>
        
        <h2 class="scene-title">The Controllers</h2>
        
        <div class="controllers-showcase">
            <div class="controller-card">
                <span class="controller-icon">🎮</span>
                <h3 class="controller-name">Xbox Series S/X</h3>
                <p class="controller-desc">Red Transparent Edition</p>
            </div>
            
            <div class="controller-card">
                <span class="controller-icon">🎮</span>
                <h3 class="controller-name">Xbox One</h3>
                <p class="controller-desc">Classic Black</p>
            </div>
            
            <div class="controller-card">
                <span class="controller-icon">🎮</span>
                <h3 class="controller-name">PlayStation 5</h3>
                <p class="controller-desc">DualSense White</p>
            </div>
            
            <div class="controller-card">
                <span class="controller-icon">🎮</span>
                <h3 class="controller-name">PlayStation 4</h3>
                <p class="controller-desc">DualShock Black</p>
            </div>
        </div>
    </section>

    <!-- SCENE 4: THE TRANSFORMATION -->
    <section id="scene-transformation">
        <div class="particle-effect" id="particles"></div>
        
        <div class="glitch-container">
            <h2 class="glitch-text" data-text="THE TRANSFORMATION">THE TRANSFORMATION</h2>
        </div>
        
        <div class="repair-showcase">
            <div class="repair-card">
                <div class="repair-icon">🕹️</div>
                <h3 class="repair-title">STICK DRIFT?</h3>
                <p class="repair-desc">Analog stick dissolves and reforms pristine. Precision calibration restores dead zones and eliminates unwanted movement.</p>
            </div>
            
            <div class="repair-card">
                <div class="repair-icon">🔌</div>
                <h3 class="repair-title">BROKEN PORT?</h3>
                <p class="repair-desc">Charging port sparks, then glows clean. Full replacement of damaged aux and charging connections.</p>
            </div>
            
            <div class="repair-card">
                <div class="repair-icon">✨</div>
                <h3 class="repair-title">WORN OUT?</h3>
                <p class="repair-desc">Controller shell transforms from scuffed to pristine. Deep cleaning and housing replacement available.</p>
            </div>
            
            <div class="repair-card">
                <div class="repair-icon">⚡</div>
                <h3 class="repair-title">DEEP CLEAN</h3>
                <p class="repair-desc">Complete disassembly and ultrasonic cleaning. Remove dust, grime, and restore that new controller feel.</p>
            </div>
        </div>
    </section>

    <!-- SCENE 5: THE LOGO REVEAL -->
    <section id="scene-logo">
        <div class="est-date">EST. 2026</div>
        
        <div class="logo-container">
            <div class="logo-ring"></div>
            <div class="logo-ring"></div>
            <div class="logo-ring"></div>
            
            <div class="logo-center">
                <div class="tcl-logo">TCL</div>
                <span class="anchor-symbol">⚓</span>
                <div style="font-family: 'Orbitron'; font-size: 1.2rem; color: #888; letter-spacing: 0.3rem;">THE CONTROLLER LAB</div>
            </div>
        </div>
        
        <h2 class="slogan">Every Controller Deserves a Respawn</h2>
    </section>

    <!-- SCENE 6: PRICE LIST -->
    <section id="scene-prices">
        <div class="holographic-container">
            <div class="price-header">
                <h2 class="price-title">
                    <span>🎮</span>
                    PRICE LIST
                    <span>🎮</span>
                </h2>
                <p style="color: #666; letter-spacing: 0.2rem;">HOLOGRAPHIC DISPLAY // EST. 2026</p>
            </div>
            
            <div class="price-category">
                <h3 class="category-title">Stick Drift Repair</h3>
                <div class="price-item">
                    <span class="price-name">Standard Analog Stick</span>
                    <span class="price-value">R120</span>
                </div>
                <div class="price-item">
                    <span class="price-name">Hall Effect Analog Stick</span>
                    <span class="price-value">R180</span>
                </div>
            </div>
            
            <div class="price-category">
                <h3 class="category-title">Components Replacement</h3>
                <div class="price-item">
                    <span class="price-name">Xbox One/Series S/X Aux Jack</span>
                    <span class="price-value">R50</span>
                </div>
                <div class="price-item">
                    <span class="price-name">Charging Port</span>
                    <span class="price-value">R120</span>
                </div>
            </div>
            
            <div class="price-category">
                <h3 class="category-title">Housing & Cleaning</h3>
                <div class="price-item">
                    <span class="price-name">Xbox Series S/X Controller Housing</span>
                    <span class="price-value">R250</span>
                </div>
                <div class="price-item">
                    <span class="price-name">Deep Clean (Per Controller)</span>
                    <span class="price-value">R50</span>
                </div>
            </div>
            
            <div class="price-note">
                <p>For all other controller issues/upgrades, please contact me and I'll be able to see what can be done at a <span class="contact-highlight">reasonable price</span>.</p>
            </div>
        </div>
    </section>

    <!-- SCENE 7: CALL TO ACTION -->
    <section id="scene-cta">
        <div class="final-controller"></div>
        
        <div class="contact-card">
            <h2 class="contact-name">CONTACT UZAIR</h2>
            <p class="contact-role">Master Technician</p>
            
            <a href="tel:0671237816" class="phone-number">067 123 7816</a>
            
            <p style="color: #666; margin-bottom: 2rem; font-size: 0.9rem; letter-spacing: 0.1rem;">
                WHATSAPP AVAILABLE • SOUTH AFRICA
            </p>
            
            <div class="social-links">
                <a href="#" class="social-link" title="Instagram">📷</a>
                <a href="#" class="social-link" title="Facebook">f</a>
                <a href="#" class="social-link" title="WhatsApp">📱</a>
            </div>
        </div>
        
        <p style="margin-top: 3rem; color: #444; font-size: 0.8rem; letter-spacing: 0.2rem;">
            © 2026 THE CONTROLLER LAB • EVERY CONTROLLER DESERVES A RESPAWN
        </p>
    </section>

    <script>
        // LOADER
        window.addEventListener('load', () => {
            setTimeout(() => {
                document.getElementById('loader').classList.add('hidden');
            }, 1500);
        });

        // SCROLL PROGRESS
        window.addEventListener('scroll', () => {
            const winScroll = document.body.scrollTop || document.documentElement.scrollTop;
            const height = document.documentElement.scrollHeight - document.documentElement.clientHeight;
            const scrolled = (winScroll / height) * 100;
            document.getElementById('progressBar').style.width = scrolled + '%';
            
            // Update nav dots
            updateNavDots();
        });

        // NAVIGATION
        function scrollToSection(id) {
            document.getElementById(id).scrollIntoView({ behavior: 'smooth' });
        }

        function updateNavDots() {
            const sections = ['scene-void', 'scene-first-breath', 'scene-reveal', 'scene-transformation', 'scene-logo', 'scene-prices', 'scene-cta'];
            const dots = document.querySelectorAll('.nav-dot');
            
            sections.forEach((section, index) => {
                const element = document.getElementById(section);
                const rect = element.getBoundingClientRect();
                
                if (rect.top <= window.innerHeight / 2 && rect.bottom >= window.innerHeight / 2) {
                    dots.forEach(d => d.classList.remove('active'));
                    dots[index].classList.add('active');
                }
            });
        }

        // PARTICLE GENERATION
        function createParticles() {
            const container = document.getElementById('particles');
            for (let i = 0; i < 30; i++) {
                const particle = document.createElement('div');
                particle.className = 'particle';
                particle.style.left = Math.random() * 100 + '%';
                particle.style.animationDelay = Math.random() * 10 + 's';
                particle.style.animationDuration = (10 + Math.random() * 10) + 's';
                container.appendChild(particle);
            }
        }
        createParticles();

        // PARALLAX EFFECT
        window.addEventListener('scroll', () => {
            const scrolled = window.pageYOffset;
            const parallaxElements = document.querySelectorAll('.controller-3d, .logo-container');
            
            parallaxElements.forEach(el => {
                const speed = 0.5;
                el.style.transform = `translateY(${scrolled * speed}px)`;
            });
        });

        // INTERSECTION OBSERVER FOR ANIMATIONS
        const observerOptions = {
            threshold: 0.3,
            rootMargin: '0px'
        };

        const observer = new IntersectionObserver((entries) => {
            entries.forEach(entry => {
                if (entry.isIntersecting) {
                    entry.target.style.opacity = '1';
                    entry.target.style.transform = 'translateY(0)';
                }
            });
        }, observerOptions);

        // Observe all cards
        document.querySelectorAll('.controller-card, .repair-card, .price-item').forEach(el => {
            el.style.opacity = '0';
            el.style.transform = 'translateY(30px)';
            el.style.transition = 'all 0.6s ease';
            observer.observe(el);
        });

        // AUDIO SIMULATION (Visual only)
        setInterval(() => {
            const bars = document.querySelectorAll('.viz-bar');
            bars.forEach(bar => {
                bar.style.height = (20 + Math.random() * 40) + 'px';
            });
        }, 200);

        // KEYBOARD NAVIGATION
        document.addEventListener('keydown', (e) => {
            const sections = ['scene-void', 'scene-first-breath', 'scene-reveal', 'scene-transformation', 'scene-logo', 'scene-prices', 'scene-cta'];
            const current = document.querySelector('.nav-dot.active');
            const currentIndex = Array.from(document.querySelectorAll('.nav-dot')).indexOf(current);
            
            if (e.key === 'ArrowDown' && currentIndex < sections.length - 1) {
                scrollToSection(sections[currentIndex + 1]);
            } else if (e.key === 'ArrowUp' && currentIndex > 0) {
                scrollToSection(sections[currentIndex - 1]);
            }
        });
    </script>
</body>
</html>
