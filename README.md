<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>vxcin.c | Professional Video Editor</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <link href="https://fonts.googleapis.com/css2?family=Orbitron:wght@400;700;900&family=Montserrat:wght@300;400;500&display=swap" rel="stylesheet">
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        :root {
            --primary-color: #ffffff;
            --accent-color: #00d9ff;
            --bg-color: #000000;
            --bg-dark: #0a0a0a;
            --glow-color: rgba(255, 255, 255, 0.1);
            --transition-speed: 0.6s;
            --section-spacing: 100px;
        }

        html {
            scroll-behavior: smooth;
        }

        body {
            font-family: 'Montserrat', sans-serif;
            background-color: var(--bg-color);
            color: var(--primary-color);
            overflow-x: hidden;
            line-height: 1.6;
        }

        /* Background Animation */
        #particles-js {
            position: fixed;
            width: 100%;
            height: 100%;
            z-index: -2;
        }

        .light-streaks {
            position: fixed;
            width: 100%;
            height: 100%;
            background: radial-gradient(circle at 30% 50%, rgba(120, 119, 198, 0.1) 0%, transparent 50%),
                        radial-gradient(circle at 80% 20%, rgba(255, 255, 255, 0.05) 0%, transparent 50%),
                        radial-gradient(circle at 20% 80%, rgba(255, 255, 255, 0.05) 0%, transparent 50%);
            z-index: -1;
            animation: pulse 15s infinite alternate;
        }

        @keyframes pulse {
            0% { opacity: 0.5; }
            100% { opacity: 1; }
        }

        /* Smooth Scroll Container */
        .smooth-scroll {
            position: relative;
            z-index: 1;
        }

        /* Header & Navigation */
        header {
            position: fixed;
            top: 0;
            width: 100%;
            padding: 25px 50px;
            display: flex;
            justify-content: space-between;
            align-items: center;
            z-index: 100;
            transition: all 0.5s ease;
            background: rgba(0, 0, 0, 0.5);
            backdrop-filter: blur(10px);
        }

        .logo {
            font-family: 'Orbitron', sans-serif;
            font-weight: 900;
            font-size: 28px;
            letter-spacing: 1px;
            color: white;
            text-decoration: none;
            position: relative;
            transition: all 0.3s ease;
        }

        .logo:hover {
            text-shadow: 0 0 15px rgba(255, 255, 255, 0.7);
        }

        .logo::after {
            content: '';
            position: absolute;
            bottom: -5px;
            left: 0;
            width: 0;
            height: 2px;
            background: var(--accent-color);
            transition: width 0.5s ease;
        }

        .logo:hover::after {
            width: 100%;
        }

        .nav-links {
            display: flex;
            list-style: none;
            gap: 40px;
        }

        .nav-links a {
            color: white;
            text-decoration: none;
            font-weight: 500;
            font-size: 16px;
            letter-spacing: 0.5px;
            position: relative;
            padding: 5px 0;
            transition: all 0.3s ease;
        }

        .nav-links a::after {
            content: '';
            position: absolute;
            bottom: 0;
            left: 0;
            width: 0;
            height: 1px;
            background: var(--accent-color);
            transition: width 0.3s ease;
        }

        .nav-links a:hover::after {
            width: 100%;
        }

        .nav-links a:hover {
            color: var(--accent-color);
        }

        .mobile-menu-btn {
            display: none;
            background: none;
            border: none;
            color: white;
            font-size: 24px;
            cursor: pointer;
        }

        /* Hero Section */
        .hero {
            min-height: 100vh;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            text-align: center;
            padding: 0 20px;
            position: relative;
            overflow: hidden;
        }

        .hero-content {
            max-width: 1200px;
            z-index: 2;
        }

        .brand-name {
            font-family: 'Orbitron', sans-serif;
            font-weight: 900;
            font-size: clamp(3rem, 10vw, 8rem);
            color: white;
            text-transform: uppercase;
            letter-spacing: 3px;
            margin-bottom: 20px;
            position: relative;
            animation: reveal 1.5s cubic-bezier(0.77, 0, 0.175, 1) forwards;
            opacity: 0;
            transform: translateY(30px);
        }

        @keyframes reveal {
            0% { 
                opacity: 0;
                transform: translateY(30px);
            }
            100% { 
                opacity: 1;
                transform: translateY(0);
            }
        }

        .brand-name span {
            display: inline-block;
            text-shadow: 0 0 20px rgba(255, 255, 255, 0.5);
            position: relative;
        }

        .brand-glow {
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            width: 100%;
            height: 100%;
            background: radial-gradient(circle, rgba(255, 255, 255, 0.1) 0%, transparent 70%);
            filter: blur(40px);
            z-index: -1;
            opacity: 0.7;
        }

        .subtitle {
            font-size: clamp(1.2rem, 3vw, 1.8rem);
            font-weight: 300;
            margin-bottom: 30px;
            opacity: 0;
            animation: fadeIn 1s 0.8s forwards;
            color: rgba(255, 255, 255, 0.9);
        }

        @keyframes fadeIn {
            to { opacity: 1; }
        }

        .divider {
            width: 200px;
            height: 2px;
            background: linear-gradient(90deg, transparent, var(--accent-color), transparent);
            margin: 30px auto;
            opacity: 0;
            animation: fadeIn 1s 1.2s forwards;
        }

        .scroll-indicator {
            position: absolute;
            bottom: 40px;
            left: 50%;
            transform: translateX(-50%);
            display: flex;
            flex-direction: column;
            align-items: center;
            opacity: 0;
            animation: fadeIn 1s 1.5s forwards;
        }

        .scroll-text {
            font-size: 14px;
            margin-bottom: 10px;
            color: rgba(255, 255, 255, 0.7);
            letter-spacing: 1px;
        }

        .scroll-line {
            width: 1px;
            height: 50px;
            background: linear-gradient(to bottom, var(--accent-color), transparent);
            animation: scrollLine 2s infinite;
        }

        @keyframes scrollLine {
            0% { height: 0; opacity: 0; }
            50% { height: 50px; opacity: 1; }
            100% { height: 0; opacity: 0; }
        }

        /* Sections Common Styling */
        section {
            padding: var(--section-spacing) 50px;
            position: relative;
            max-width: 1400px;
            margin: 0 auto;
        }

        .section-title {
            font-family: 'Orbitron', sans-serif;
            font-size: 2.5rem;
            font-weight: 700;
            margin-bottom: 60px;
            text-align: center;
            letter-spacing: 2px;
            position: relative;
            display: inline-block;
            left: 50%;
            transform: translateX(-50%);
        }

        .section-title::after {
            content: '';
            position: absolute;
            bottom: -15px;
            left: 0;
            width: 100%;
            height: 2px;
            background: linear-gradient(90deg, transparent, var(--accent-color), transparent);
        }

        /* About Section */
        .about-content {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 60px;
            align-items: center;
        }

        .about-text {
            font-size: 1.2rem;
            line-height: 1.8;
            color: rgba(255, 255, 255, 0.9);
        }

        .highlight {
            color: var(--accent-color);
            font-weight: 600;
        }

        .about-stats {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 30px;
        }

        .stat {
            background: rgba(255, 255, 255, 0.05);
            border: 1px solid rgba(255, 255, 255, 0.1);
            padding: 30px 20px;
            border-radius: 10px;
            text-align: center;
            transition: all 0.4s ease;
            backdrop-filter: blur(10px);
        }

        .stat:hover {
            transform: translateY(-10px);
            border-color: var(--accent-color);
            box-shadow: 0 10px 20px rgba(0, 217, 255, 0.1);
        }

        .stat-number {
            font-family: 'Orbitron', sans-serif;
            font-size: 2.5rem;
            font-weight: 700;
            margin-bottom: 10px;
            color: var(--accent-color);
        }

        .stat-label {
            font-size: 1rem;
            color: rgba(255, 255, 255, 0.8);
            text-transform: uppercase;
            letter-spacing: 1px;
        }

        /* Services Section */
        .services-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
            gap: 30px;
            margin-top: 60px;
        }

        .service-card {
            background: rgba(255, 255, 255, 0.05);
            border: 1px solid rgba(255, 255, 255, 0.1);
            border-radius: 15px;
            padding: 40px 30px;
            text-align: center;
            transition: all 0.4s ease;
            backdrop-filter: blur(10px);
            position: relative;
            overflow: hidden;
            z-index: 1;
        }

        .service-card::before {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: linear-gradient(135deg, rgba(255, 255, 255, 0.1) 0%, transparent 100%);
            opacity: 0;
            transition: opacity 0.4s ease;
            z-index: -1;
        }

        .service-card:hover {
            transform: translateY(-15px);
            border-color: var(--accent-color);
            box-shadow: 0 15px 30px rgba(0, 0, 0, 0.3);
        }

        .service-card:hover::before {
            opacity: 1;
        }

        .service-icon {
            font-size: 3rem;
            margin-bottom: 25px;
            color: var(--accent-color);
        }

        .service-title {
            font-family: 'Orbitron', sans-serif;
            font-size: 1.5rem;
            margin-bottom: 15px;
            font-weight: 700;
            letter-spacing: 1px;
        }

        .service-desc {
            color: rgba(255, 255, 255, 0.8);
            line-height: 1.6;
        }

        /* Footer */
        footer {
            padding: 80px 50px 40px;
            text-align: center;
            position: relative;
            border-top: 1px solid rgba(255, 255, 255, 0.1);
            margin-top: var(--section-spacing);
        }

        .footer-content {
            max-width: 800px;
            margin: 0 auto;
        }

        .brand-footer {
            font-family: 'Orbitron', sans-serif;
            font-weight: 900;
            font-size: 2.5rem;
            color: white;
            margin-bottom: 40px;
            letter-spacing: 2px;
        }

        .contact-info {
            display: flex;
            justify-content: center;
            gap: 40px;
            margin-bottom: 50px;
            flex-wrap: wrap;
        }

        .contact-item {
            display: flex;
            flex-direction: column;
            align-items: center;
            gap: 10px;
            transition: all 0.3s ease;
        }

        .contact-item:hover {
            transform: translateY(-5px);
        }

        .contact-icon {
            font-size: 1.8rem;
            color: var(--accent-color);
        }

        .contact-text {
            font-size: 1.1rem;
            color: rgba(255, 255, 255, 0.9);
        }

        .copyright {
            color: rgba(255, 255, 255, 0.5);
            font-size: 0.9rem;
            margin-top: 40px;
            padding-top: 20px;
            border-top: 1px solid rgba(255, 255, 255, 0.05);
        }

        /* Responsive Design */
        @media (max-width: 992px) {
            .about-content {
                grid-template-columns: 1fr;
                gap: 50px;
            }
            
            .nav-links {
                gap: 25px;
            }
            
            section {
                padding: 80px 30px;
            }
        }

        @media (max-width: 768px) {
            header {
                padding: 20px 30px;
            }
            
            .nav-links {
                position: fixed;
                top: 0;
                right: -100%;
                width: 250px;
                height: 100vh;
                background: rgba(0, 0, 0, 0.95);
                flex-direction: column;
                justify-content: center;
                align-items: center;
                transition: right 0.5s ease;
                backdrop-filter: blur(10px);
                gap: 30px;
            }
            
            .nav-links.active {
                right: 0;
            }
            
            .mobile-menu-btn {
                display: block;
            }
            
            .services-grid {
                grid-template-columns: 1fr;
            }
            
            .contact-info {
                flex-direction: column;
                gap: 30px;
            }
            
            .about-stats {
                grid-template-columns: 1fr;
            }
        }

        @media (max-width: 576px) {
            .brand-name {
                font-size: clamp(2.5rem, 12vw, 4rem);
            }
            
            section {
                padding: 60px 20px;
            }
            
            .section-title {
                font-size: 2rem;
            }
        }

        /* Page Transition Effects */
        .page-transition {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: var(--bg-color);
            z-index: 9999;
            opacity: 0;
            pointer-events: none;
            transition: opacity 0.8s ease;
        }

        .page-transition.active {
            opacity: 1;
        }

        /* Glitch Effect for Brand Name */
        .glitch {
            position: relative;
        }

        .glitch::before,
        .glitch::after {
            content: attr(data-text);
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            opacity: 0.8;
        }

        .glitch::before {
            color: #0ff;
            z-index: -1;
            animation: glitch-effect 3s infinite;
        }

        .glitch::after {
            color: #f0f;
            z-index: -2;
            animation: glitch-effect 2s infinite reverse;
        }

        @keyframes glitch-effect {
            0% { transform: translate(0); }
            20% { transform: translate(-2px, 2px); }
            40% { transform: translate(-2px, -2px); }
            60% { transform: translate(2px, 2px); }
            80% { transform: translate(2px, -2px); }
            100% { transform: translate(0); }
        }

        /* Smooth Fade-in for Sections */
        .section-fade {
            opacity: 0;
            transform: translateY(30px);
            transition: opacity 0.8s ease, transform 0.8s ease;
        }

        .section-fade.visible {
            opacity: 1;
            transform: translateY(0);
        }
    </style>
</head>
<body>
    <!-- Background Elements -->
    <div id="particles-js"></div>
    <div class="light-streaks"></div>
    
    <!-- Page Transition Overlay -->
    <div class="page-transition"></div>
    
    <!-- Header -->
    <header>
        <a href="#" class="logo">vxcin.c</a>
        <button class="mobile-menu-btn" id="mobileMenuBtn">
            <i class="fas fa-bars"></i>
        </button>
        <ul class="nav-links" id="navLinks">
            <li><a href="#home">Home</a></li>
            <li><a href="#about">About</a></li>
            <li><a href="#services">Services</a></li>
            <li><a href="#contact">Contact</a></li>
        </ul>
    </header>
    
    <!-- Smooth Scroll Container -->
    <div class="smooth-scroll">
        <!-- Hero Section -->
        <section class="hero" id="home">
            <div class="hero-content">
                <h1 class="brand-name glitch" data-text="vxcin.c">
                    <span>vxcin.c</span>
                    <div class="brand-glow"></div>
                </h1>
                <p class="subtitle">I am a Professional Video Editor</p>
                <div class="divider"></div>
            </div>
            <div class="scroll-indicator">
                <span class="scroll-text">SCROLL</span>
                <div class="scroll-line"></div>
            </div>
        </section>
        
        <!-- About Section -->
        <section class="about section-fade" id="about">
            <h2 class="section-title">CRAFTING VISUAL STORIES</h2>
            <div class="about-content">
                <div class="about-text">
                    <p>I transform raw footage into <span class="highlight">cinematic experiences</span> that captivate audiences and elevate brands. With a keen eye for detail and a passion for visual storytelling, I create content that stands out in today's digital landscape.</p>
                    <br>
                    <p>Specializing in <span class="highlight">dynamic reels</span>, <span class="highlight">eye-catching thumbnails</span>, and <span class="highlight">premium color grading</span>, I ensure every frame delivers maximum impact. My approach combines technical precision with creative vision to produce videos that not only look stunning but also connect with viewers on an emotional level.</p>
                    <br>
                    <p>Whether you're an influencer, a brand, or a creative professional, I'm here to bring your vision to life with edits that are both professional and unforgettable.</p>
                </div>
                <div class="about-stats">
                    <div class="stat">
                        <div class="stat-number">500+</div>
                        <div class="stat-label">Projects Edited</div>
                    </div>
                    <div class="stat">
                        <div class="stat-number">4+</div>
                        <div class="stat-label">Years Experience</div>
                    </div>
                    <div class="stat">
                        <div class="stat-number">100%</div>
                        <div class="stat-label">Client Satisfaction</div>
                    </div>
                    <div class="stat">
                        <div class="stat-number">48h</div>
                        <div class="stat-label">Avg. Delivery Time</div>
                    </div>
                </div>
            </div>
        </section>
        
        <!-- Services Section -->
        <section class="services section-fade" id="services">
            <h2 class="section-title">SERVICES</h2>
            <div class="services-grid">
                <div class="service-card">
                    <div class="service-icon">
                        <i class="fas fa-film"></i>
                    </div>
                    <h3 class="service-title">Video Editing</h3>
                    <p class="service-desc">Professional editing for YouTube, documentaries, commercials, and social media. Clean cuts, seamless transitions, and narrative flow that keeps viewers engaged.</p>
                </div>
                
                <div class="service-card">
                    <div class="service-icon">
                        <i class="fas fa-mobile-alt"></i>
                    </div>
                    <h3 class="service-title">Reels & Shorts</h3>
                    <p class="service-desc">Attention-grabbing vertical content optimized for Instagram, TikTok, and YouTube Shorts. Fast-paced edits with trending effects and audio synchronization.</p>
                </div>
                
                <div class="service-card">
                    <div class="service-icon">
                        <i class="fas fa-image"></i>
                    </div>
                    <h3 class="service-title">Thumbnails</h3>
                    <p class="service-desc">Custom thumbnails designed to increase CTR and views. Bold typography, compelling imagery, and strategic composition that stands out in crowded feeds.</p>
                </div>
                
                <div class="service-card">
                    <div class="service-icon">
                        <i class="fas fa-palette"></i>
                    </div>
                    <h3 class="service-title">Color Grading</h3>
                    <p class="service-desc">Cinematic color correction and grading to establish mood, enhance visuals, and create signature looks. From natural tones to bold stylistic choices.</p>
                </div>
            </div>
        </section>
        
        <!-- Footer -->
        <footer class="section-fade" id="contact">
            <div class="footer-content">
                <div class="brand-footer">vxcin.c</div>
                
                <div class="contact-info">
                    <a href="https://instagram.com/vxcin.c" target="_blank" class="contact-item">
                        <i class="fab fa-instagram contact-icon"></i>
                        <span class="contact-text">@vxcin.c</span>
                    </a>
                    
                    <a href="mailto:vxcin.c@gmail.com" class="contact-item">
                        <i class="fas fa-envelope contact-icon"></i>
                        <span class="contact-text">vxcin.c@gmail.com</span>
                    </a>
                </div>
                
                <p style="color: rgba(255, 255, 255, 0.7); max-width: 600px; margin: 0 auto 40px;">
                    Ready to elevate your video content? Let's collaborate to create something extraordinary. Reach out for project inquiries, rates, and availability.
                </p>
                
                <div class="copyright">
                    &copy; 2023 vxcin.c | Professional Video Editing Services
                </div>
            </div>
        </footer>
    </div>

    <script src="https://cdn.jsdelivr.net/particles.js/2.0.0/particles.min.js"></script>
    <script>
        // Initialize Particles.js Background
        document.addEventListener('DOMContentLoaded', function() {
            // Particles.js configuration
            particlesJS("particles-js", {
                particles: {
                    number: { value: 80, density: { enable: true, value_area: 800 } },
                    color: { value: "#ffffff" },
                    shape: { type: "circle" },
                    opacity: { value: 0.3, random: true },
                    size: { value: 3, random: true },
                    line_linked: {
                        enable: true,
                        distance: 150,
                        color: "#ffffff",
                        opacity: 0.1,
                        width: 1
                    },
                    move: {
                        enable: true,
                        speed: 1,
                        direction: "none",
                        random: true,
                        straight: false,
                        out_mode: "out",
                        bounce: false
                    }
                },
                interactivity: {
                    detect_on: "canvas",
                    events: {
                        onhover: { enable: true, mode: "repulse" },
                        onclick: { enable: true, mode: "push" },
                        resize: true
                    }
                },
                retina_detect: true
            });

            // Mobile menu toggle
            const mobileMenuBtn = document.getElementById('mobileMenuBtn');
            const navLinks = document.getElementById('navLinks');
            
            mobileMenuBtn.addEventListener('click', () => {
                navLinks.classList.toggle('active');
                mobileMenuBtn.innerHTML = navLinks.classList.contains('active') 
                    ? '<i class="fas fa-times"></i>' 
                    : '<i class="fas fa-bars"></i>';
            });
            
            // Close mobile menu when clicking a link
            document.querySelectorAll('.nav-links a').forEach(link => {
                link.addEventListener('click', () => {
                    navLinks.classList.remove('active');
                    mobileMenuBtn.innerHTML = '<i class="fas fa-bars"></i>';
                });
            });
            
            // Scroll-based animations
            const sectionFades = document.querySelectorAll('.section-fade');
            
            const fadeInOnScroll = () => {
                sectionFades.forEach(section => {
                    const sectionTop = section.getBoundingClientRect().top;
                    const windowHeight = window.innerHeight;
                    
                    if (sectionTop < windowHeight * 0.85) {
                        section.classList.add('visible');
                    }
                });
            };
            
            // Initial check
            fadeInOnScroll();
            
            // Check on scroll
            window.addEventListener('scroll', fadeInOnScroll);
            
            // Smooth scroll for anchor links
            document.querySelectorAll('a[href^="#"]').forEach(anchor => {
                anchor.addEventListener('click', function(e) {
                    e.preventDefault();
                    
                    const targetId = this.getAttribute('href');
                    if (targetId === '#') return;
                    
                    const targetElement = document.querySelector(targetId);
                    if (targetElement) {
                        // Add page transition effect
                        const pageTransition = document.querySelector('.page-transition');
                        pageTransition.classList.add('active');
                        
                        setTimeout(() => {
                            window.scrollTo({
                                top: targetElement.offsetTop - 80,
                                behavior: 'smooth'
                            });
                            
                            setTimeout(() => {
                                pageTransition.classList.remove('active');
                            }, 300);
                        }, 300);
                    }
                });
            });
            
            // Header scroll effect
            window.addEventListener('scroll', () => {
                const header = document.querySelector('header');
                if (window.scrollY > 100) {
                    header.style.padding = '15px 50px';
                    header.style.background = 'rgba(0, 0, 0, 0.9)';
                } else {
                    header.style.padding = '25px 50px';
                    header.style.background = 'rgba(0, 0, 0, 0.5)';
                }
            });
            
            // Glitch effect on brand name at intervals
            const brandName = document.querySelector('.brand-name');
            setInterval(() => {
                brandName.classList.add('glitch');
                setTimeout(() => {
                    brandName.classList.remove('glitch');
                }, 500);
            }, 8000);
            
            // Service card hover effect enhancement
            const serviceCards = document.querySelectorAll('.service-card');
            serviceCards.forEach(card => {
                card.addEventListener('mouseenter', function() {
                    this.style.transform = 'translateY(-15px) scale(1.02)';
                });
                
                card.addEventListener('mouseleave', function() {
                    this.style.transform = 'translateY(0) scale(1)';
                });
            });
        });
    </script>
</body>
</html>
