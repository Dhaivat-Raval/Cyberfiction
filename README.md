# 🪐 Cyberfiction

> A premium, interactive 3D scroll-animation landing page featuring canvas frame scrubbing, smooth scroll mechanics, and cyber-aesthetic typography.

---

## 🔗 Overview

**Cyberfiction** is a showcase of high-end frontend animation techniques, combining **Locomotive Scroll** for custom momentum-based smooth scrolling and **GSAP (GreenSock Animation Platform) ScrollTrigger** to control an interactive, frame-by-frame image sequence on an HTML5 `<canvas>`. As the user scrolls, a detailed 3D avatar rotates and animates dynamically in sync with the viewport progression, creating a highly engaging, immersive story-telling experience.

---

## 🚀 Live Demo / Visual Showcase

### Key Features
*   **3D Frame-Scrubbing Canvas:** Renders a 300-frame high-resolution sequence of a 3D avatar that responds instantly to user scrolling.
*   **GSAP & ScrollTrigger Integration:** Binds the canvas rendering state directly to scroll position with customizable scrubbing easing.
*   **Locomotive Scroll Smoothness:** Provides a sleek, fluid momentum-based scrolling feel across all modern browsers.
*   **Responsive Scaling Engine:** Automatically scales, crops, and centers the heavy image sequence using custom aspect-ratio math to ensure perfect rendering across mobile, tablet, and desktop viewports.
*   **Cyberpunk Visual Identity:** Features striking typography, marquee animated text, clean contrast overlays, and smooth layout pinning.

---

## 🛠️ Technology Stack

*   **Markup:** Semantic [HTML5](https://developer.mozilla.org/en-US/docs/Web/HTML)
*   **Styles:** Vanilla [CSS3](https://developer.mozilla.org/en-US/docs/Web/CSS) (using custom fonts, absolute positioning overlays, transitions, and marquees)
*   **Scripting:** Vanilla Modern [JavaScript (ES6+)](https://developer.mozilla.org/en-US/docs/Web/JavaScript)
*   **Smooth Scroll:** [Locomotive Scroll v3.5.4](https://github.com/locomotivemtl/locomotive-scroll)
*   **Animation Engine:** [GSAP 3.11.5](https://greensock.com/gsap/) with [ScrollTrigger](https://greensock.com/scrolltrigger/)

---

## 📂 Project Structure

```bash
Cyberfiction/
├── assets/
│   ├── css/
│   │   └── style.css       # Layout styles, keyframe marquee loop, typographic setup
│   ├── images/
│   │   ├── male0001.png    # Frame sequence starts (300 frames)
│   │   │   ...
│   │   └── male0300.png    # Frame sequence ends
│   └── js/
│       └── script.js       # Locomotive-Scroll wrapper, GSAP ScrollTrigger proxy & Canvas rendering pipeline
├── index.html              # Main page markup with GSAP/Locomotive CDN imports
└── README.md               # Documentation
```

---

## ⚙️ How It Works (Under the Hood)

### 1. Locomotive Scroll & ScrollTrigger Proxy
Because Locomotive Scroll shifts the container using CSS transforms (`translate3d`), standard browser scroll listeners (which GSAP relies on) are bypassed. To bridge this, a proxy scroll handler is initialized:
```javascript
ScrollTrigger.scrollerProxy("#main", {
  scrollTop(value) {
    return arguments.length
      ? locoScroll.scrollTo(value, 0, 0)
      : locoScroll.scroll.instance.scroll.y;
  },
  getBoundingClientRect() {
    return { top: 0, left: 0, width: window.innerWidth, height: window.innerHeight };
  },
  pinType: document.querySelector("#main").style.transform ? "transform" : "fixed"
});
```

### 2. Canvas Frame Preloading
To ensure stutter-free scrubbing, 300 PNG frames are preloaded into memory before the scroll animations begin:
```javascript
const images = [];
const imageSeq = { frame: 0 };

for (let i = 0; i < frameCount; i++) {
  const img = new Image();
  img.src = files(i);
  images.push(img);
}
```

### 3. Dynamic Image Aspect Scaling
To prevent squishing, a custom rendering algorithm mimics `object-fit: cover` within the 2D canvas context:
```javascript
function scaleImage(img, ctx) {
  var canvas = ctx.canvas;
  var hRatio = canvas.width / img.width;
  var vRatio = canvas.height / img.height;
  var ratio = Math.max(hRatio, vRatio);
  var centerShift_x = (canvas.width - img.width * ratio) / 2;
  var centerShift_y = (canvas.height - img.height * ratio) / 2;
  ctx.clearRect(0, 0, canvas.width, canvas.height);
  ctx.drawImage(img, 0, 0, img.width, img.height,
                centerShift_x, centerShift_y, img.width * ratio, img.height * ratio);
}
```

---

## 📦 Getting Started

To run the project locally, you don't need compilation tools or package managers since it's a standard static web application.

### Prerequisites
Make sure you have a web server to handle local assets without CORS issues (since some browsers restrict loading canvas images from a local `file://` protocol).

### Run with Live Server (Recommended)
1. Clone the repository.
2. Open the project folder in your preferred editor (e.g., VS Code).
3. Right-click `index.html` and select **Open with Live Server**.
4. Alternatively, use python to start a quick server in the directory:
   ```bash
   python3 -m http.server 8000
   ```
5. Open `http://localhost:8000` in your browser.

---

## 🎨 Visual Details & Typography

*   **Typeface:** Gilroy (geometric sans-serif)
*   **Aesthetic:** Clean dark/light contrast, minimalism combined with high-tech elements.
*   **Palette:**
    *   Main Background: `#f1f1f1` (Warm off-white)
    *   Subtext: `#7c7c7c` (Medium grey)
    *   Accents/Buttons: `#000000` & `#ffffff`

---

## 📜 License

This project is open-source and available under the [MIT License](LICENSE).
