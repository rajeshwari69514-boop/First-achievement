# EXAMPLE
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Animal Galaxy - Fixed Menu</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css">
    <style>
        :root {
            --purple-main: #9d4edd;
            --purple-glow: #c77dff;
            --dark-bg: #0b090a;
            --panel-bg: #161a1d;
        }

        body {
            background-color: var(--dark-bg);
            margin: 0;
            min-height: 100vh;
            font-family: 'Segoe UI', sans-serif;
            color: white;
            overflow-x: hidden;
        }

        /* --- BACKGROUND --- */
        .smoke-bg {
            position: fixed;
            top: 0; left: 0; width: 100%; height: 100%;
            z-index: -1;
            background: radial-gradient(circle at 50% 50%, rgba(157, 78, 221, 0.15) 0%, transparent 80%);
            filter: blur(60px);
        }

        /* --- HEADER --- */
        header {
            display: flex;
            align-items: center;
            justify-content: space-between;
            padding: 15px 5%;
            background: rgba(22, 26, 29, 0.95);
            backdrop-filter: blur(10px);
            position: sticky;
            top: 0;
            z-index: 100;
            border-bottom: 2px solid var(--purple-main);
        }

        .menu-icon { font-size: 26px; cursor: pointer; color: var(--purple-glow); transition: 0.3s; }
        .menu-icon:hover { transform: scale(1.1); text-shadow: 0 0 10px var(--purple-glow); }

        .search-container { position: relative; width: 250px; }
        .search-container input {
            width: 100%; background: #000; border: 2px solid var(--purple-main);
            border-radius: 20px; padding: 8px 15px 8px 40px; color: white; outline: none;
        }
        .search-container i { position: absolute; left: 15px; top: 10px; color: var(--purple-main); }

        /* --- FIXED SIDE MENU --- */
        .side-nav {
            position: fixed;
            top: 0; left: -300px; 
            width: 280px; height: 100%;
            background: var(--panel-bg);
            z-index: 200;
            padding: 30px 20px;
            transition: 0.4s ease-in-out;
            border-right: 3px solid var(--purple-main);
            box-shadow: 10px 0 30px rgba(0,0,0,0.5);
        }

        .side-nav.active { left: 0; }
        .side-nav h2 { color: var(--purple-glow); margin-bottom: 30px; border-bottom: 1px solid #333; padding-bottom: 10px; }
        .side-nav a {
            display: block; color: white; padding: 15px; text-decoration: none;
            font-size: 18px; transition: 0.3s; border-radius: 10px; margin-bottom: 5px;
        }
        .side-nav a:hover { background: rgba(157, 78, 221, 0.2); color: var(--purple-glow); padding-left: 25px; }
        .close-btn { position: absolute; top: 20px; right: 20px; font-size: 24px; cursor: pointer; color: #666; }

        /* --- VIDEO GRID --- */
        .main-content { padding: 40px 20px; display: flex; flex-direction: column; align-items: center; }
        .video-wrapper { display: flex; gap: 25px; width: 100%; max-width: 1200px; justify-content: center; flex-wrap: wrap; }

        .video-card {
            flex: 1; min-width: 300px; max-width: 360px;
            background: var(--panel-bg); border: 10px solid var(--purple-main);
            border-radius: 40px; padding: 12px; transition: 0.3s; cursor: pointer;
        }
        .video-card:hover { transform: translateY(-5px); border-color: var(--purple-glow); }
        .video-card.hidden { display: none; }

        .video-container { position: relative; border-radius: 25px; overflow: hidden; aspect-ratio: 16/9; background: #000; }
        video { width: 100%; height: 100%; object-fit: cover; }

        /* --- TITLE SYMBOL STYLING --- */
        .video-title { 
            text-align: center; 
            margin-top: 15px; 
            font-weight: bold; 
            font-size: 17px; 
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 12px;
        }

        .animal-symbol {
            width: 32px;
            height: 32px;
            border-radius: 50%;
            border: 2px solid var(--purple-glow);
            object-fit: cover;
            box-shadow: 0 0 10px rgba(157, 78, 221, 0.5);
            transition: 0.3s;
        }

        .video-card:hover .animal-symbol {
            transform: scale(1.2);
            box-shadow: 0 0 15px var(--purple-glow);
        }

        /* --- UI LAYER & BUTTONS --- */
        .ui-layer {
            position: absolute; inset: 0;
            display: flex; flex-direction: column; justify-content: flex-end;
            padding: 15px; background: linear-gradient(transparent 50%, rgba(0,0,0,0.8));
            opacity: 0; transition: 0.3s; pointer-events: none;
        }
        .video-container:hover .ui-layer { opacity: 1; }
        .ui-layer * { pointer-events: auto; }

        .progress-box { width: 100%; height: 6px; background: rgba(255,255,255,0.2); border-radius: 5px; margin-bottom: 10px; cursor: pointer; }
        .progress-bar { height: 100%; width: 0%; background: var(--purple-glow); box-shadow: 0 0 8px var(--purple-glow); }

        .btn-row { display: flex; justify-content: center; gap: 15px; }
        button { background: none; border: none; color: white; font-size: 18px; cursor: pointer; transition: 0.2s; }
        button:hover { transform: scale(1.2); }
        
        button.liked { color: var(--purple-glow); text-shadow: 0 0 10px var(--purple-glow); }
        button.disliked { color: #ff4d6d; text-shadow: 0 0 10px #ff4d6d; }
    </style>
</head>
<body>

    <div class="smoke-bg"></div>

    <nav class="side-nav" id="mainMenu">
        <div class="close-btn" id="closeMenu"><i class="fas fa-times"></i></div>
        <h2><i class="fas fa-paw"></i> Explore</h2>
        <a href="#"><i class="fas fa-home"></i> Home</a>
        <a href="#"><i class="fas fa-fire"></i> Trending</a>
        <a href="#"><i class="fas fa-heart"></i> Favorites</a>
    </nav>

    <header>
        <div class="menu-icon" id="openMenu"><i class="fas fa-bars"></i></div>
        <div class="search-container">
            <i class="fas fa-search"></i>
            <input type="text" id="searchInput" placeholder="Search animals..." onkeyup="filterVideos()">
        </div>
        <div style="width: 24px;"></div>
    </header>

    <div class="main-content">
        <div class="video-wrapper">
            
            <div class="video-card" data-title="cat animal video">
                <div class="video-container">
                    <video src="myvideo.mp4" loop muted></video>
                    <div class="ui-layer">
                        <div class="progress-box"><div class="progress-bar"></div></div>
                        <div class="btn-row">
                            <button class="like-btn"><i class="fas fa-thumbs-up"></i></button>
                            <button class="dislike-btn"><i class="fas fa-thumbs-down"></i></button>
                            <button class="bell-btn"><i class="fas fa-bell"></i></button>
                            <button class="minimize-btn"><i class="fas fa-compress"></i></button>
                        </div>
                    </div>
                </div>
                <div class="video-title">
                    <img src="https://images.unsplash.com/photo-1514888286974-6c03e2ca1dba?w=100" class="animal-symbol">
                    Cat Animal Video
                </div>
            </div>

            <div class="video-card" data-title="dog animal video">
                <div class="video-container">
                    <video src="myvedio1.mp4" loop muted></video>
                    <div class="ui-layer">
                        <div class="progress-box"><div class="progress-bar"></div></div>
                        <div class="btn-row">
                            <button class="like-btn"><i class="fas fa-thumbs-up"></i></button>
                            <button class="dislike-btn"><i class="fas fa-thumbs-down"></i></button>
                            <button class="bell-btn"><i class="fas fa-bell"></i></button>
                            <button class="minimize-btn"><i class="fas fa-compress"></i></button>
                        </div>
                    </div>
                </div>
                <div class="video-title">
                    <img src="https://images.unsplash.com/photo-1517849845537-4d257902454a?w=100" class="animal-symbol">
                    Dog Animal Video
                </div>
            </div>

            <div class="video-card" data-title="bird animal video">
                <div class="video-container">
                    <video src="myvedio2.mp4" loop muted></video>
                    <div class="ui-layer">
                        <div class="progress-box"><div class="progress-bar"></div></div>
                        <div class="btn-row">
                            <button class="like-btn"><i class="fas fa-thumbs-up"></i></button>
                            <button class="dislike-btn"><i class="fas fa-thumbs-down"></i></button>
                            <button class="bell-btn"><i class="fas fa-bell"></i></button>
                            <button class="minimize-btn"><i class="fas fa-compress"></i></button>
                        </div>
                    </div>
                </div>
                <div class="video-title">
                    <img src="sample1.jpg" class="animal-symbol">
                    Bird Animal Video
                </div>
            </div>

        </div>
    </div>

    <script>
        const mainMenu = document.getElementById('mainMenu');
        const openMenu = document.getElementById('openMenu');
        const closeMenu = document.getElementById('closeMenu');

        openMenu.onclick = () => mainMenu.classList.add('active');
        closeMenu.onclick = () => mainMenu.classList.remove('active');

        function filterVideos() {
            let input = document.getElementById('searchInput').value.toLowerCase();
            document.querySelectorAll('.video-card').forEach(card => {
                let title = card.getAttribute('data-title');
                card.classList.toggle('hidden', !title.includes(input));
            });
        }

        document.querySelectorAll('.video-container').forEach(container => {
            const video = container.querySelector('video');
            const pBar = container.querySelector('.progress-bar');
            const pBox = container.querySelector('.progress-box');
            const likeBtn = container.querySelector('.like-btn');
            const dislikeBtn = container.querySelector('.dislike-btn');
            const bellBtn = container.querySelector('.bell-btn');
            const minBtn = container.querySelector('.minimize-btn');

            video.onclick = () => {
                if (container.requestFullscreen) container.requestFullscreen();
                video.play(); video.muted = false;
            };

            video.ontimeupdate = () => {
                pBar.style.width = (video.currentTime / video.duration) * 100 + "%";
            };

            pBox.onclick = (e) => {
                e.stopPropagation();
                const rect = pBox.getBoundingClientRect();
                video.currentTime = ((e.clientX - rect.left) / rect.width) * video.duration;
            };

            likeBtn.onclick = (e) => { e.stopPropagation(); likeBtn.classList.toggle('liked'); dislikeBtn.classList.remove('disliked'); };
            dislikeBtn.onclick = (e) => { e.stopPropagation(); dislikeBtn.classList.toggle('disliked'); likeBtn.classList.remove('liked'); };
            bellBtn.onclick = (e) => { e.stopPropagation(); bellBtn.classList.toggle('notified'); };
            minBtn.onclick = (e) => { e.stopPropagation(); if (document.fullscreenElement) document.exitFullscreen(); };
        });
    </script>
</body>
</html>

# Output
<img width="1765" height="585" alt="image" src="https://github.com/user-attachments/assets/dbbf2269-eb9c-48b0-809c-412bd264d6d6" />


 # Output
 https://rajeshwari69514-boop.github.io/First-achievement/
