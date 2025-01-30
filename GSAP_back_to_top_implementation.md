# GSAP Back-to-Top Button Implementation

## Overview
This document details the implementation of a back-to-top button with a circular progress indicator using GSAP (GreenSock Animation Platform). The implementation includes smooth animations, a progress circle that fills based on scroll position, and proper handling of button visibility.

## Changes Made

### 1. Added Required Dependencies
```html
<script src="https://cdnjs.cloudflare.com/ajax/libs/gsap/3.11.0/gsap.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/gsap/3.11.0/ScrollToPlugin.min.js"></script>
```

### 2. HTML Structure
```html
<div class="scroll-top-container">
    <button class="scroll-top" type="button" title="Scroll to top">
        <svg class="progress-circle" width="100%" height="100%" viewBox="-1 -1 102 102">
            <path d="M50,1 a49,49 0 0,1 0,98 a49,49 0 0,1 0,-98"/>
        </svg>
        <svg class="icon-tabler-arrow-up" xmlns="http://www.w3.org/2000/svg" width="32" height="32" viewBox="0 0 24 24" stroke-width="1.5">
            <path stroke="none" d="M0 0h24v24H0z" fill="none"/>
            <line x1="12" y1="5" x2="12" y2="19"/>
            <line x1="18" y1="11" x2="12" y2="5"/>
            <line x1="6" y1="11" x2="12" y2="5"/>
        </svg>
    </button>
</div>
```

### 3. CSS Styling
```css
.scroll-top {
    position: fixed;
    z-index: 50;
    right: 30px;
    bottom: 30px;
    height: 46px;
    width: 46px;
    cursor: pointer;
    display: flex;
    align-items: center;
    justify-content: center;
    border-radius: 50%;
    border: none;
    background-color: white;
    box-shadow: 0 2px 10px rgba(0,0,0,0.1);
}

.scroll-top .progress-circle {
    position: absolute;
    top: 0;
    left: 0;
    transform: rotate(-90deg);
}

.scroll-top .progress-circle path {
    fill: none;
    stroke: #22C55E;
    stroke-width: 4;
}

.scroll-top .icon-tabler-arrow-up {
    position: relative;
    z-index: 1;
    stroke: #333;
    stroke-width: 2;
}

.scroll-top:hover .icon-tabler-arrow-up {
    stroke: #22C55E;
}
```

### 4. GSAP Animation Implementation
```javascript
// Initial setup
const scrollTopBtn = document.querySelector('.scroll-top');
const progressPath = document.querySelector('.progress-circle path');
const pathLength = progressPath.getTotalLength();

// Set initial states
gsap.set(scrollTopBtn, { 
    opacity: 0,
    visibility: 'hidden',
    y: 15
});
gsap.set(progressPath, {
    strokeDasharray: pathLength,
    strokeDashoffset: pathLength
});

// Scroll event handler
window.addEventListener('scroll', () => {
    const totalScroll = document.documentElement.scrollHeight - window.innerHeight;
    const scrollPosition = window.pageYOffset;
    const scrollPercentage = scrollPosition / totalScroll;

    if (scrollPosition > 100) {
        // Show button and animate progress
        gsap.to(scrollTopBtn, {
            duration: 0.4,
            opacity: 1,
            visibility: 'visible',
            y: 0
        });

        gsap.to(progressPath, {
            duration: 0.1,
            strokeDashoffset: pathLength - (scrollPercentage * pathLength)
        });
    } else {
        // Hide button
        gsap.to(scrollTopBtn, {
            duration: 0.4,
            opacity: 0,
            visibility: 'hidden',
            y: 15
        });
    }
});

// Scroll to top on click
scrollTopBtn.addEventListener('click', () => {
    gsap.to(window, {
        duration: 1,
        scrollTo: 0,
        ease: "power2.inOut"
    });
});
```

## Features
- Smooth appearance/disappearance of the back-to-top button
- Circular progress indicator showing scroll position
- Smooth scroll animation when clicking the button
- Hover effects for better user interaction
- Mobile-friendly implementation

## Browser Compatibility
This implementation uses modern JavaScript and CSS features, compatible with:
- Chrome (latest)
- Firefox (latest)
- Safari (latest)
- Edge (latest)

## Notes
- The button appears after scrolling 100px from the top
- The progress circle fills based on the scroll position relative to the total scrollable height
- All animations use GSAP for smooth, performant transitions
- The implementation is responsive and works on both desktop and mobile devices
