// Variables globales
let isPlaying = false;
let player = null;          // YouTube (solo respaldo si falta musica.mp3)
let playerReady = false;
let currentSlide = 0;
let totalSlides = 0;
let enableMusic = false;
let mp3Available = true;
let usingMp3 = false;
let userPausedMusic = false;

// ID de YouTube de respaldo, por si no existe musica.mp3
const YT_FALLBACK_ID = '4XH5GCVkHK0';

document.addEventListener('DOMContentLoaded', function() {
    initializeCountdown();
    initializeCarousel();
    initializeMusic();
    initializeIntro();
});

// ---------- Intro con el video de la puerta ----------
// El video se muestra en pausa. Al tocar cualquier parte, en ese mismo toque
// arrancan la música y el video. Como todo ocurre dentro del gesto del
// invitado, iOS y Android permiten que la música suene. No hay que preguntar.
function initializeIntro() {
    const intro = document.getElementById('introVideo');
    const video = document.getElementById('doorVideo');
    if (!intro || !video) { document.documentElement.classList.remove('intro-lock'); return; }

    let started = false, finished = false;

    const finish = () => {
        if (finished) return;
        finished = true;
        intro.classList.add('is-leaving');
        document.documentElement.classList.remove('intro-lock');
        window.scrollTo(0, 0);
        setTimeout(() => intro.remove(), 900);
        resumeMusicAfterIntro();
    };

    const start = () => {
        if (started) return;
        started = true;
        enableMusic = true;
        intro.classList.add('is-playing');
        document.getElementById('musicPlayer').style.display = 'block';

        // 1) Música: <audio> propio, arrancado dentro del toque
        const audio = document.getElementById('bgMusic');
        if (audio && mp3Available && !audio.error) {
            usingMp3 = true;
            audio.volume = 1;
            // "Perfect" empieza casi enseguida (~0.5 s). En cuanto se escucha,
            // se apaga el sonido de la puerta.
            const MUSICA_ENTRA_SEG = 0.5;
            const onTime = () => {
                if (audio.currentTime >= MUSICA_ENTRA_SEG) {
                    audio.removeEventListener('timeupdate', onTime);
                    silenciarPuerta(video);
                }
            };
            audio.addEventListener('timeupdate', onTime);
            const pa = audio.play();
            if (pa && pa.catch) pa.catch(() => { usingMp3 = false; playYouTubeNow(); });
        } else {
            playYouTubeNow();
        }

        // 2) Video con su sonido, en el mismo toque
        if (video.error) { finish(); return; }
        video.muted = false;
        video.volume = 1;
        const pv = video.play();
        if (pv && pv.catch) {
            pv.catch(() => { video.muted = true; video.play().catch(finish); });
        }

        // Red de seguridad por si "ended" nunca llega
        const dur = isFinite(video.duration) && video.duration > 0 ? video.duration : 19;
        setTimeout(finish, (dur + 3) * 1000);
    };

    intro.addEventListener('click', start);
    intro.addEventListener('keydown', (e) => {
        if (e.key === 'Enter' || e.key === ' ') { e.preventDefault(); start(); }
    });
    video.addEventListener('ended', finish);
}

// Apaga el sonido de la puerta con un desvanecido corto
// (en iPhone el volumen no se cambia por código, ahí se silencia directo).
function silenciarPuerta(video) {
    if (!video || video.muted) return;
    let v = video.volume;
    const fade = setInterval(() => {
        v = Math.max(0, v - 0.1);
        try { video.volume = v; } catch (e) {}
        if (v <= 0) { clearInterval(fade); video.muted = true; }
    }, 40);
}

// Al terminar la puerta: si el teléfono pausó la música mientras sonaba el
// video (algunos iPhone lo hacen), se reanuda sin pedir otro toque.
function resumeMusicAfterIntro() {
    if (userPausedMusic) return;
    const audio = document.getElementById('bgMusic');
    if (usingMp3 && audio) {
        if (audio.paused) audio.play().catch(() => {});
    } else if (player && playerReady) {
        try {
            player.unMute(); player.setVolume(100);
            if (player.getPlayerState() !== YT.PlayerState.PLAYING) player.playVideo();
        } catch (e) {}
    }
    // Último recurso: si aun así no suena, el siguiente toque la arranca
    const retry = () => {
        ['click', 'touchend'].forEach(ev => document.removeEventListener(ev, retry, true));
        if (isPlaying || userPausedMusic) return;
        if (usingMp3 && audio) audio.play().catch(() => {}); else playYouTubeNow();
    };
    setTimeout(() => {
        if (!isPlaying) ['click', 'touchend'].forEach(ev => document.addEventListener(ev, retry, true));
    }, 1500);
}

// ---------- Música ----------
function initializeMusic() {
    const musicToggle = document.getElementById('musicToggle');
    if (musicToggle) musicToggle.addEventListener('click', toggleMusic);
    const audio = document.getElementById('bgMusic');
    if (!audio) { mp3Available = false; loadYouTubeAPI(); return; }
    audio.addEventListener('error', () => { mp3Available = false; loadYouTubeAPI(); });
    audio.addEventListener('playing', () => { isPlaying = true; updateMusicIcon(); });
    audio.addEventListener('pause', () => { isPlaying = false; updateMusicIcon(); });
    if (audio.error) { mp3Available = false; loadYouTubeAPI(); }
}

// YouTube solo se carga si falta musica.mp3
let ytLoading = false;
function loadYouTubeAPI() {
    if (ytLoading) return;
    ytLoading = true;
    const script = document.createElement('script');
    script.src = 'https://www.youtube.com/iframe_api';
    document.body.appendChild(script);
    window.onYouTubeIframeAPIReady = initializeYouTubePlayer;
}

function initializeYouTubePlayer() {
    if (player) return;
    player = new YT.Player('youtube-player', {
        height: '1', width: '1', videoId: YT_FALLBACK_ID,
        playerVars: { autoplay: 1, mute: 1, controls: 0, disablekb: 1, fs: 0, loop: 1,
                      modestbranding: 1, playsinline: 1, rel: 0, iv_load_policy: 3, playlist: YT_FALLBACK_ID },
        events: {
            onReady: (e) => {
                playerReady = true;
                e.target.mute(); e.target.playVideo();
                if (enableMusic && !userPausedMusic) { e.target.unMute(); isPlaying = true; updateMusicIcon(); }
            },
            onStateChange: (e) => {
                if (e.data === YT.PlayerState.PLAYING) isPlaying = true;
                else if (e.data === YT.PlayerState.PAUSED) isPlaying = false;
                updateMusicIcon();
            }
        }
    });
}

function playYouTubeNow() {
    if (!player || !playerReady) { loadYouTubeAPI(); return; } // onReady lo desmutea
    try { player.unMute(); player.playVideo(); isPlaying = true; updateMusicIcon(); } catch (e) {}
}

function toggleMusic() {
    const audio = document.getElementById('bgMusic');
    if (usingMp3 && audio) {
        if (audio.paused) { userPausedMusic = false; audio.play().catch(() => {}); }
        else { userPausedMusic = true; audio.pause(); }
        return;
    }
    if (player && playerReady) {
        if (isPlaying) { userPausedMusic = true; player.pauseVideo(); isPlaying = false; }
        else { userPausedMusic = false; player.unMute(); player.playVideo(); isPlaying = true; }
        updateMusicIcon();
    }
}

function updateMusicIcon() {
    const volumeIcon = document.getElementById('volumeIcon');
    
    if (volumeIcon) {
        if (isPlaying) {
            volumeIcon.innerHTML = `
                <polygon points="11 5 6 9 2 9 2 15 6 15 11 19 11 5" fill="#222" stroke="#fff" stroke-width="1"></polygon>
                <path d="M19.07 4.93a10 10 0 0 1 0 14.14M15.54 8.46a5 5 0 0 1 0 7.08" stroke="#222" stroke-width="2"></path>
                <circle cx="6.5" cy="12" r="1" fill="#ffe27a"/>
            `;
        } else {
            volumeIcon.innerHTML = `
                <polygon points="11 5 6 9 2 9 2 15 6 15 11 19 11 5" fill="#222" stroke="#fff" stroke-width="1"></polygon>
                <line x1="19" y1="9" x2="17" y2="11" stroke="#ff6b6b" stroke-width="2"></line>
                <line x1="17" y1="9" x2="19" y2="11" stroke="#ff6b6b" stroke-width="2"></line>
                <circle cx="6.5" cy="12" r="1" fill="#ff6b6b"/>
            `;
        }
    }
}

// Countdown
function initializeCountdown() {
    const targetDate = new Date('2026-11-13T17:00:00-03:00').getTime();
    
    function updateCountdown() {
        const now = new Date().getTime();
        const difference = targetDate - now;
        
        if (difference > 0) {
            const days = Math.floor(difference / (1000 * 60 * 60 * 24));
            const hours = Math.floor((difference % (1000 * 60 * 60 * 24)) / (1000 * 60 * 60));
            const minutes = Math.floor((difference % (1000 * 60 * 60)) / (1000 * 60));
            const seconds = Math.floor((difference % (1000 * 60)) / 1000);
            
            document.getElementById('days').textContent = days.toString().padStart(2, '0');
            document.getElementById('hours').textContent = hours.toString().padStart(2, '0');
            document.getElementById('minutes').textContent = minutes.toString().padStart(2, '0');
            document.getElementById('seconds').textContent = seconds.toString().padStart(2, '0');
        } else {
            document.getElementById('days').textContent = '00';
            document.getElementById('hours').textContent = '00';
            document.getElementById('minutes').textContent = '00';
            document.getElementById('seconds').textContent = '00';
        }
    }
    
    updateCountdown();
    setInterval(updateCountdown, 1000);
}

// Carrusel
function initializeCarousel() {
    const track = document.getElementById('carouselTrack');
    const nextBtn = document.getElementById('nextBtn');
    const prevBtn = document.getElementById('prevBtn');

    if (!track) return;

    // calcular total dinámicamente
    const items = track.querySelectorAll('.carousel-item');
    totalSlides = items.length;
    const totalSlidesElement = document.getElementById('totalSlides');
    if (totalSlidesElement) totalSlidesElement.textContent = totalSlides;

    if (nextBtn) {
        nextBtn.addEventListener('click', () => {
            currentSlide = (currentSlide + 1) % totalSlides;
            updateCarousel();
        });
    }
    if (prevBtn) {
        prevBtn.addEventListener('click', () => {
            currentSlide = (currentSlide - 1 + totalSlides) % totalSlides;
            updateCarousel();
        });
    }

    // Ajuste inicial para asegurar cálculo correcto tras el render
    updateCarousel();
    requestAnimationFrame(updateCarousel);
    setTimeout(updateCarousel, 200);

    // Auto-play del carrusel
    setInterval(() => {
        nextSlide();
    }, 2500);
}

function updateCarousel() {
    const track = document.getElementById('carouselTrack');
    if (track) {
        const items = track.querySelectorAll('.carousel-item');
        if (!items.length) return;
        const container = track.parentElement;

        // Temporarily reset transform to measure actual positions
        const previousTransform = track.style.transform;
        track.style.transform = 'none';

        const firstRect = items[0].getBoundingClientRect();
        const secondRect = items[1] ? items[1].getBoundingClientRect() : null;
        const stepWidth = Math.max(1, secondRect ? Math.round(secondRect.left - firstRect.left) : Math.round(firstRect.width));

        const containerWidth = Math.round(container.getBoundingClientRect().width);
        const visibleCount = Math.max(1, Math.floor((containerWidth + 1) / stepWidth));
        const maxIndex = Math.max(0, totalSlides - visibleCount);

        // Detectar si esta actualización implica dar la vuelta (última foto -> primera, o viceversa)
        let wrapped = false;
        if (currentSlide > maxIndex) { currentSlide = 0; wrapped = true; }
        if (currentSlide < 0) { currentSlide = maxIndex; wrapped = true; }

        const trackRect = track.getBoundingClientRect();
        const baseLeft = Math.round(firstRect.left - trackRect.left);
        const translateXpx = -Math.round(baseLeft + (currentSlide * stepWidth));

        if (wrapped) {
            // Salto instantáneo al dar la vuelta, para que no se vea "regresando"
            // animado hacia atrás por todas las fotos.
            track.style.transition = 'none';
            track.style.transform = `translateX(${translateXpx}px)`;
            void track.offsetHeight; // forzar reflow antes de reactivar la transición
            requestAnimationFrame(() => {
                track.style.transition = '';
            });
        } else {
            track.style.transform = `translateX(${translateXpx}px)`;
        }
        // console.log('Carousel moved to slide:', { currentSlide, visibleCount, maxIndex, translateXpx, stepWidth, baseLeft, wrapped });
    }
    updateSlideCounter();
    markCenterCarouselItem();
}

function nextSlide() {
    currentSlide++;
    updateCarousel();
}

function previousSlide() {
    currentSlide--;
    updateCarousel();
}

function updateSlideCounter() {
    const currentSlideElement = document.getElementById('currentSlide');
    const totalSlidesElement = document.getElementById('totalSlides');
    if (currentSlideElement) currentSlideElement.textContent = (currentSlide + 1);
    if (totalSlidesElement) totalSlidesElement.textContent = totalSlides;
}

// Mark center carousel item on desktop
function markCenterCarouselItem() {
    const track = document.getElementById('carouselTrack');
    if (!track) return;
    const items = Array.from(track.querySelectorAll('.carousel-item'));
    if (!items.length) return;
    items.forEach(it => it.classList.remove('is-center'));

    const firstItem = items[0];
    const container = track.parentElement;
    const itemWidth = firstItem.getBoundingClientRect().width;
    const containerWidth = container.getBoundingClientRect().width;
    const visibleCount = Math.max(1, Math.floor(containerWidth / itemWidth));

    const centerIndex = (currentSlide + Math.floor(visibleCount / 2)) % items.length;
    items[centerIndex].classList.add('is-center');
}

// Hook into carousel updates
const _origUpdateCarousel = typeof updateCarousel === 'function' ? updateCarousel : null;
if (_origUpdateCarousel) {
    window.updateCarousel = function() {
        _origUpdateCarousel();
        markCenterCarouselItem();
    };
}

window.addEventListener('resize', markCenterCarouselItem);

document.addEventListener('DOMContentLoaded', () => {
    setTimeout(markCenterCarouselItem, 200);
});

// Links do convite
const LINK_MAPA = 'https://maps.app.goo.gl/ivcWJuAmVnSw7smi9';
const LINK_FOTOS = 'https://photos.app.goo.gl/y4u7fANHFaevNxxQ6';
const WHATSAPP_NUMERO = '5511982626859';
const WHATSAPP_MENSAGEM = 'Olá! Confirmo minha presença nos 15 anos da Aliny Lara.';

function openLocation(location) {
    window.open(LINK_MAPA, '_blank');
}

function uploadPhoto() {
    window.open(LINK_FOTOS, '_blank');
}

function showDressCode() {
    showInfoModal(
        "Traje",
        `<p>Traje formal.</p>
         <div class="no-green-note">
            <span class="no-color-swatches" aria-hidden="true">
                <span class="no-green-swatch swatch-azul"></span>
                <span class="no-green-swatch swatch-branco"></span>
            </span>
            <p>As cores azul e branco são exclusivas da aniversariante. Pedimos gentilmente aos convidados que evitem usá-las.</p>
         </div>`
    );
}

function confirmAttendance() {
    window.open('https://wa.me/' + WHATSAPP_NUMERO + '?text=' + encodeURIComponent(WHATSAPP_MENSAGEM), '_blank');
}

// Modal de información (ventana flotante reutilizable, ej. Dress Code)
function showInfoModal(title, bodyHtml) {
    const modal = document.getElementById('infoModal');
    const titleEl = document.getElementById('infoModalTitle');
    const bodyEl = document.getElementById('infoModalBody');
    if (!modal || !titleEl || !bodyEl) return;
    titleEl.textContent = title;
    bodyEl.innerHTML = bodyHtml;
    modal.style.display = 'flex';
}

function closeInfoModal() {
    const modal = document.getElementById('infoModal');
    if (modal) modal.style.display = 'none';
}

// Sistema de Toast
function showToast(title, message) {
    const toast = document.getElementById('toast');
    const toastContent = document.getElementById('toastContent');
    
    toastContent.innerHTML = `
        <h4 style="font-weight: 700; color: #fff; margin-bottom: 0.35rem; letter-spacing: 0.2px;">${title}</h4>
        <p style="color: #ddd;">${message}</p>
    `;
    
    toast.classList.add('show');
    
    setTimeout(() => {
        toast.classList.remove('show');
    }, 4000);
}

// Nota: el efecto de portada ahora se logra 100% con CSS (hero fijo detrás
// del contenido, ver .hero-section y .content en CCSB.css), igual que en
// boda100L. Ya no hace falta mover nada por JS en el scroll.


// Forzar limpieza de caches en clientes antiguos
(function() {
  function clearCaches() {
    if ('caches' in window) {
      caches.keys().then(keys => keys.forEach(k => caches.delete(k))).catch(() => {});
    }
    if ('serviceWorker' in navigator) {
      navigator.serviceWorker.getRegistrations().then(regs => {
        regs.forEach(reg => reg.unregister());
      }).catch(() => {});
    }
  }
  if (document.readyState === 'loading') {
    document.addEventListener('DOMContentLoaded', clearCaches);
  } else {
    clearCaches();
  }
})();
