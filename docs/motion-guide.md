# SOWN Motion Guide
> 버전: v1 · 2026

---

## 트랜지션 기본값

| 용도 | duration | easing |
|------|----------|--------|
| 기본 (hover, 색상) | 200ms | ease |
| 카드/버튼 이동 | 250ms | cubic-bezier(0.16,1,0.3,1) |
| 스크롤 리빌 (opacity+transform) | 700ms | ease |
| 카드 stagger 슬라이드인 | 600ms | cubic-bezier(0.16,1,0.3,1) |
| 텍스트 라인 리빌 | 700ms | cubic-bezier(0.16,1,0.3,1) |
| 토스트 진입 | 300ms | cubic-bezier(0.16,1,0.3,1) |
| 토스트 퇴장 | 200ms | ease-in |
| 모달 진입 | 250ms | cubic-bezier(0.16,1,0.3,1) |
| 모달 퇴장 | 200ms | ease-in |

---

## 스크롤 리빌 (IntersectionObserver 표준)

```css
[data-reveal] {
  opacity: 0;
  transform: translateY(24px);
  transition: opacity 0.7s ease, transform 0.7s ease;
}
[data-reveal].revealed {
  opacity: 1;
  transform: none;
}
```

```js
const io = new IntersectionObserver(
  entries => entries.forEach(e => {
    if (e.isIntersecting) e.target.classList.add('revealed');
  }),
  { threshold: 0.12, rootMargin: '0px 0px -40px 0px' }
);
document.querySelectorAll('[data-reveal]').forEach(el => io.observe(el));
```

---

## 카드 stagger

```css
[data-slide] {
  opacity: 0;
  transform: translateX(60px);
  transition: opacity 0.6s ease, transform 0.6s cubic-bezier(0.16,1,0.3,1);
}
[data-slide].revealed { opacity: 1; transform: none; }
[data-slide]:nth-child(1) { transition-delay: 0s; }
[data-slide]:nth-child(2) { transition-delay: 0.1s; }
[data-slide]:nth-child(3) { transition-delay: 0.2s; }
[data-slide]:nth-child(4) { transition-delay: 0.3s; }
```

---

## 버튼 hover

```css
/* Primary */
.btn-primary:hover {
  background: #033610;
  transform: translateY(-1px);
  box-shadow: 0 4px 16px rgba(5,79,25,0.12);
  transition: all 0.2s ease;
}
.btn-primary:active {
  transform: translateY(0);
  box-shadow: none;
}

/* Lime */
.btn-lime:hover {
  background: #B8CC3A;
  transform: translateY(-1px);
  box-shadow: 0 4px 16px rgba(5,79,25,0.12);
}

/* Arrow */
.btn-arrow { transition: gap 0.2s ease; }
.btn-arrow:hover { gap: 10px; }
```

---

## 카드 hover

```css
.card-product,
.card-curation {
  transition: box-shadow 0.2s ease, transform 0.2s ease;
}
.card-product:hover {
  box-shadow: 0 4px 16px rgba(5,79,25,0.12);
  transform: translateY(-2px);
}
.card-curation:hover {
  box-shadow: 0 4px 16px rgba(5,79,25,0.12);
}
```

---

## 토스트 진입·퇴장

```css
@keyframes toastIn {
  from { opacity: 0; transform: translateX(100%); }
  to   { opacity: 1; transform: translateX(0); }
}
@keyframes toastOut {
  from { opacity: 1; transform: translateX(0); }
  to   { opacity: 0; transform: translateX(100%); }
}

.toast.entering { animation: toastIn 300ms cubic-bezier(0.16,1,0.3,1) forwards; }
.toast.leaving  { animation: toastOut 200ms ease-in forwards; }
```

---

## 모달 진입·퇴장

```css
/* Overlay */
@keyframes overlayIn  { from { opacity:0; } to { opacity:1; } }
@keyframes overlayOut { from { opacity:1; } to { opacity:0; } }

/* Panel */
@keyframes modalIn  { from { opacity:0; transform:translateY(16px) scale(0.98); } to { opacity:1; transform:none; } }
@keyframes modalOut { from { opacity:1; transform:none; } to { opacity:0; transform:translateY(8px) scale(0.99); } }

.modal-overlay.open  { animation: overlayIn  250ms ease forwards; }
.modal-overlay.close { animation: overlayOut 200ms ease-in forwards; }
.modal-panel.open    { animation: modalIn  250ms cubic-bezier(0.16,1,0.3,1) forwards; }
.modal-panel.close   { animation: modalOut 200ms ease-in forwards; }
```

---

## 스켈레톤 shimmer

```css
@keyframes shimmer {
  0%   { background-position: -400px 0; }
  100% { background-position:  400px 0; }
}

.skeleton {
  background: linear-gradient(
    90deg,
    #EDE7D8 25%,
    #F5F0E8 50%,
    #EDE7D8 75%
  );
  background-size: 800px 100%;
  animation: shimmer 1.6s infinite;
}
```

---

## 이미지 패럴랙스 (히어로 전용, speed 0.1~0.15)

```js
const parallaxImgs = document.querySelectorAll('[data-parallax]');
function updateParallax() {
  parallaxImgs.forEach(img => {
    const rect = img.getBoundingClientRect();
    const speed = parseFloat(img.dataset.parallax) || 0.12;
    img.style.transform = `translateY(${
      (rect.top + rect.height / 2 - window.innerHeight / 2) * speed
    }px)`;
  });
}
window.addEventListener('scroll', updateParallax, { passive: true });
updateParallax();
```

---

## passive:true scroll 표준

```js
/* 모든 scroll 이벤트 — passive:true 필수 */
window.addEventListener('scroll', handler, { passive: true });
element.addEventListener('scroll', handler, { passive: true });
element.addEventListener('touchstart', handler, { passive: true });
element.addEventListener('touchend', handler, { passive: true });
element.addEventListener('touchmove', handler, { passive: true });
```

---

## 반응형 브레이크포인트

| 이름 | 값 | 용도 |
|------|----|------|
| xs | 375px | 최소 지원 너비 |
| sm | 640px | 모바일 가로 |
| md | 768px | 태블릿 세로 |
| lg | 1024px | 태블릿 가로 / 소형 데스크탑 |
| xl | 1280px | 데스크탑 기본 |

```css
/* 모바일 우선 */
@media (max-width: 768px) {
  /* 2컬럼→1컬럼 그리드 전환 */
  .grid-2, .grid-3, .grid-4 { grid-template-columns: 1fr !important; }

  /* GNB: 햄버거 메뉴 전환 */
  .gnb-nav { display: none; }
  .gnb-mobile { display: flex; }

  /* 버튼 풀너비 */
  .btn-primary, .btn-secondary { width: 100%; justify-content: center; }

  /* 카드 가로 스크롤 */
  .card-scroll-row { display: flex; overflow-x: auto; scroll-snap-type: x mandatory; -webkit-overflow-scrolling: touch; }
  .card-scroll-row::-webkit-scrollbar { display: none; }
  .card-scroll-row .card-product { flex-shrink: 0; width: 80vw; scroll-snap-align: start; }

  /* PULL 블록 항상 1컬럼 */
  .pull-page { max-width: 100%; }
}

@media (max-width: 640px) {
  /* 타이포 최소값 보호 */
  .type-hero { font-size: 36px; }
  .type-h2   { font-size: 28px; }
}

@media (min-width: 1280px) {
  /* 최대 컨테이너 */
  .container { max-width: 1200px; margin: 0 auto; }
}
```

---

*SOWN Motion Guide v1 · 2026*
