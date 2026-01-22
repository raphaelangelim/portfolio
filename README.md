# portfolio
Welcome to Raphael's Portfolio
<div id="wp-video-ad-wrapper" style="position: relative; margin: 20px auto; max-width: 900px;">
    <div id="v-close-btn" aria-label="Close Ad" role="button" tabindex="0">×</div>

    <div id="wp-custom-video-slider-container" class="v-slider-wrapper">
        <a href="https://wa.me/1234567890?text=I%20am%20interested%20in%20the%20Winter%20Camp!" target="_blank" class="v-whatsapp-link">
            <div class="v-slider-track" id="v-slider-track">
                <div class="v-slide">
                    <video src="https://bli.ca/wp-content/uploads/2025/08/WinterCampPromo_banner01.mp4" muted playsinline></video>
                </div>
                <div class="v-slide">
                    <video src="https://bli.ca/wp-content/uploads/2025/08/WinterCampPromo_banner02.mp4" muted playsinline></video>
                </div>
                <div class="v-slide">
                    <video src="https://bli.ca/wp-content/uploads/2025/08/WinterCampPromo_banner03.mp4" muted playsinline></video>
                </div>
                <div class="v-slide">
                    <video src="https://bli.ca/wp-content/uploads/2025/08/WinterCampPromo_banner04.mp4" muted playsinline></video>
                </div>
            </div>
        </a>

        <div class="v-prev" role="button">‹</div>
        <div class="v-next" role="button">›</div>
        <div class="v-dots" id="v-dots-container"></div>
    </div>
</div>

<style>
/* Close Button Styling */
#v-close-btn {
    position: absolute;
    top: -15px;
    right: -15px;
    width: 30px;
    height: 30px;
    background: #ff4d4d;
    color: white;
    border-radius: 50%;
    text-align: center;
    line-height: 26px;
    font-size: 22px;
    font-weight: bold;
    cursor: pointer;
    z-index: 100;
    box-shadow: 0 2px 5px rgba(0,0,0,0.3);
    border: 2px solid white;
    transition: 0.2s;
}
#v-close-btn:hover {
    background: #cc0000;
    transform: scale(1.1);
}

/* Slider Styling */
.v-slider-wrapper {
    position: relative;
    width: 100%;
    overflow: hidden;
    border-radius: 10px;
    background: #000;
}
.v-whatsapp-link { display: block; text-decoration: none; }
.v-slider-track { display: flex; transition: transform 0.6s cubic-bezier(0.25, 1, 0.5, 1); }
.v-slide { min-width: 100%; }
.v-slide video { width: 100%; height: auto; display: block; }

/* Navigation Styling */
.v-prev, .v-next {
    cursor: pointer; position: absolute; top: 50%; transform: translateY(-50%);
    padding: 15px 10px; color: white; font-size: 30px;
    background: rgba(0, 0, 0, 0.3); z-index: 20; transition: 0.3s;
}
.v-prev { left: 0; border-radius: 0 5px 5px 0; }
.v-next { right: 0; border-radius: 5px 0 0 5px; }
.v-dots { text-align: center; position: absolute; bottom: 15px; width: 100%; z-index: 20; }
.v-dot {
    cursor: pointer; height: 10px; width: 10px; margin: 0 5px;
    background-color: rgba(255,255,255,0.5); border-radius: 50%; display: inline-block;
}
.v-dot.active { background-color: #fff; transform: scale(1.2); }
</style>

<script>
(function() {
    const wrapper = document.getElementById('wp-video-ad-wrapper');
    const container = document.getElementById('wp-custom-video-slider-container');
    const track = document.getElementById('v-slider-track');
    const videos = container.querySelectorAll('video');
    const dotsContainer = document.getElementById('v-dots-container');
    const closeBtn = document.getElementById('v-close-btn');
    let currentIndex = 0;
    let autoSlideTimeout;

    // Close logic
    closeBtn.addEventListener('click', function() {
        wrapper.style.display = 'none';
        videos.forEach(v => v.pause()); // Stop all videos
    });

    // Slider logic
    videos.forEach((_, i) => {
        const dot = document.createElement("span");
        dot.classList.add("v-dot");
        dot.addEventListener("click", (e) => { e.preventDefault(); e.stopPropagation(); goToSlide(i); });
        dotsContainer.appendChild(dot);
    });
    const dots = container.querySelectorAll('.v-dot');

    function updateSlider() {
        track.style.transform = `translateX(${-currentIndex * 100}%)`;
        dots.forEach((d, i) => d.classList.toggle('active', i === currentIndex));
        playVideo();
    }

    function playVideo() {
        videos.forEach(v => { v.pause(); v.currentTime = 0; });
        const currentVideo = videos[currentIndex];
        currentVideo.play().catch(() => {});
        currentVideo.onended = () => { autoSlideTimeout = setTimeout(() => move(1), 400); };
    }

    function move(dir) {
        clearTimeout(autoSlideTimeout);
        currentIndex = (currentIndex + dir + videos.length) % videos.length;
        updateSlider();
    }

    function goToSlide(index) {
        clearTimeout(autoSlideTimeout);
        currentIndex = index;
        updateSlider();
    }

    container.querySelector('.v-prev').addEventListener('click', (e) => { e.preventDefault(); e.stopPropagation(); move(-1); });
    container.querySelector('.v-next').addEventListener('click', (e) => { e.preventDefault(); e.stopPropagation(); move(1); });

    updateSlider();
})();
</script>
