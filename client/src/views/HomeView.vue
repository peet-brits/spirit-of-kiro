<template>
  <div class="hollow">
    <!-- Ambient background layers -->
    <div class="forest-floor" />
    <div class="canopy-veil" />

    <!-- Animated bioluminescent orbs -->
    <div class="orb orb-1" />
    <div class="orb orb-2" />
    <div class="orb orb-3" />
    <div class="orb orb-4" />
    <div class="orb orb-5" />

    <!-- Firefly particles -->
    <div class="fireflies">
      <span v-for="n in 18" :key="n" class="firefly" :style="fireflyStyle(n)" />
    </div>

    <!-- Foreground fern silhouettes -->
    <div class="ferns ferns-left" />
    <div class="ferns ferns-right" />

    <!-- Hero content -->
    <main class="hero">
      <div class="crystal-frame">
        <div class="crystal-edge crystal-top-left" />
        <div class="crystal-edge crystal-top-right" />
        <div class="crystal-edge crystal-bottom-left" />
        <div class="crystal-edge crystal-bottom-right" />

        <div class="hero-inner">
          <p class="eyebrow">✦ The Hollow Awaits ✦</p>
          <h1 class="title">Spirit of Kiro</h1>
          <p class="tagline">
            A fae tinkerer's infinite workshop.<br />
            Craft anything. Sell everything.<br />
            Let the spirits decide what you become.
          </p>

          <router-link to="/play" class="btn btn-enter">
            <span class="btn-glow" />
            Enter the Hollow
          </router-link>

          <div class="auth-links">
            <router-link to="/signin" class="link-fae">Sign in</router-link>
            <span class="link-divider">·</span>
            <router-link to="/signup" class="link-fae">Create account</router-link>
          </div>
        </div>
      </div>
    </main>

    <!-- Mushroom ring decoration -->
    <div class="mushroom-ring">
      <span v-for="m in 8" :key="m" class="mushroom" :style="mushroomStyle(m)" />
    </div>
  </div>
</template>

<script setup lang="ts">
function fireflyStyle(n: number): Record<string, string> {
  const seed = n * 137.508 // golden angle spread
  const x = ((Math.sin(seed) + 1) / 2) * 100
  const y = ((Math.cos(seed * 0.7) + 1) / 2) * 100
  const delay = (n * 0.43) % 6
  const duration = 3 + (n % 4)
  const size = 2 + (n % 3)
  return {
    left: `${x}%`,
    top: `${y}%`,
    animationDelay: `${delay}s`,
    animationDuration: `${duration}s`,
    width: `${size}px`,
    height: `${size}px`,
  }
}

function mushroomStyle(m: number): Record<string, string> {
  const angle = (m / 8) * 360
  const radius = 42 // percent of the ring container
  const x = 50 + radius * Math.cos((angle * Math.PI) / 180)
  const y = 50 + radius * Math.sin((angle * Math.PI) / 180)
  const delay = (m * 0.3) % 2.4
  const scale = 0.7 + (m % 3) * 0.2
  return {
    left: `${x}%`,
    top: `${y}%`,
    animationDelay: `${delay}s`,
    transform: `translate(-50%, -50%) scale(${scale})`,
  }
}
</script>

<style scoped>
/* ============================================================
   ROOT LAYOUT
   ============================================================ */
.hollow {
  position: relative;
  min-height: 100vh;
  width: 100%;
  overflow: hidden;
  display: flex;
  align-items: center;
  justify-content: center;
  background: #050d08;
  font-family:
    'Georgia',
    'Palatino Linotype',
    'Book Antiqua',
    Palatino,
    serif;
}

/* ============================================================
   BACKGROUND LAYERS
   ============================================================ */
.forest-floor {
  position: absolute;
  inset: 0;
  background:
    radial-gradient(ellipse 120% 60% at 50% 110%, #0a1f0e 0%, transparent 70%),
    radial-gradient(ellipse 80% 40% at 20% 100%, #071a0c 0%, transparent 60%),
    radial-gradient(ellipse 80% 40% at 80% 100%, #071a0c 0%, transparent 60%),
    linear-gradient(to bottom, #020b05 0%, #050f07 40%, #081808 100%);
}

.canopy-veil {
  position: absolute;
  inset: 0;
  background:
    radial-gradient(ellipse 100% 50% at 50% -10%, rgba(34, 197, 94, 0.04) 0%, transparent 70%),
    radial-gradient(ellipse 60% 30% at 15% 20%, rgba(74, 222, 128, 0.03) 0%, transparent 60%),
    radial-gradient(ellipse 60% 30% at 85% 15%, rgba(52, 211, 153, 0.03) 0%, transparent 60%);
  pointer-events: none;
}

/* ============================================================
   BIOLUMINESCENT ORBS
   ============================================================ */
.orb {
  position: absolute;
  border-radius: 50%;
  pointer-events: none;
  filter: blur(60px);
  opacity: 0.35;
  animation: orb-drift linear infinite;
}

.orb-1 {
  width: 320px;
  height: 320px;
  background: radial-gradient(circle, #22c55e, transparent 70%);
  top: 10%;
  left: -5%;
  animation-duration: 22s;
  animation-delay: 0s;
}
.orb-2 {
  width: 240px;
  height: 240px;
  background: radial-gradient(circle, #34d399, transparent 70%);
  top: 60%;
  right: -3%;
  animation-duration: 18s;
  animation-delay: -7s;
}
.orb-3 {
  width: 180px;
  height: 180px;
  background: radial-gradient(circle, #a78bfa, transparent 70%);
  top: 25%;
  right: 20%;
  animation-duration: 26s;
  animation-delay: -12s;
  opacity: 0.2;
}
.orb-4 {
  width: 280px;
  height: 280px;
  background: radial-gradient(circle, #6ee7b7, transparent 70%);
  bottom: 5%;
  left: 30%;
  animation-duration: 20s;
  animation-delay: -4s;
  opacity: 0.25;
}
.orb-5 {
  width: 140px;
  height: 140px;
  background: radial-gradient(circle, #c4b5fd, transparent 70%);
  top: 50%;
  left: 10%;
  animation-duration: 15s;
  animation-delay: -9s;
  opacity: 0.18;
}

@keyframes orb-drift {
  0% {
    transform: translateY(0px) translateX(0px);
  }
  33% {
    transform: translateY(-30px) translateX(20px);
  }
  66% {
    transform: translateY(15px) translateX(-15px);
  }
  100% {
    transform: translateY(0px) translateX(0px);
  }
}

/* ============================================================
   FIREFLIES
   ============================================================ */
.fireflies {
  position: absolute;
  inset: 0;
  pointer-events: none;
}

.firefly {
  position: absolute;
  border-radius: 50%;
  background: #d1fae5;
  box-shadow:
    0 0 4px 1px #86efac,
    0 0 10px 2px #4ade80;
  animation: firefly-blink ease-in-out infinite;
}

@keyframes firefly-blink {
  0%,
  100% {
    opacity: 0;
    transform: translate(0, 0);
  }
  20% {
    opacity: 0.9;
  }
  50% {
    opacity: 0.4;
    transform: translate(8px, -12px);
  }
  80% {
    opacity: 0.85;
    transform: translate(-6px, 5px);
  }
}

/* ============================================================
   FERN SILHOUETTES
   ============================================================ */
.ferns {
  position: absolute;
  bottom: 0;
  width: 340px;
  height: 260px;
  pointer-events: none;
  background-repeat: no-repeat;
  background-size: contain;
}

/* CSS-drawn ferns using clip-path and gradients */
.ferns-left {
  left: -20px;
  background-image:
    /* frond 1 */
    radial-gradient(
      ellipse 18px 90px at 50% 50%,
      rgba(20, 83, 45, 0.9) 0%,
      transparent 100%
    ),
    /* frond 2 */
    radial-gradient(
      ellipse 14px 70px at 50% 50%,
      rgba(21, 128, 61, 0.7) 0%,
      transparent 100%
    );
  background-position:
    20px 40px,
    60px 70px;
  filter: blur(0.5px);
  opacity: 0.8;
}

.ferns-right {
  right: -20px;
  transform: scaleX(-1);
  background-image:
    radial-gradient(
      ellipse 18px 90px at 50% 50%,
      rgba(20, 83, 45, 0.9) 0%,
      transparent 100%
    ),
    radial-gradient(
      ellipse 14px 70px at 50% 50%,
      rgba(21, 128, 61, 0.7) 0%,
      transparent 100%
    );
  background-position:
    20px 40px,
    60px 70px;
  filter: blur(0.5px);
  opacity: 0.8;
}

/* ============================================================
   MUSHROOM RING
   ============================================================ */
.mushroom-ring {
  position: absolute;
  width: 520px;
  height: 520px;
  bottom: -80px;
  left: 50%;
  transform: translateX(-50%);
  pointer-events: none;
}

.mushroom {
  position: absolute;
  display: block;
  width: 14px;
  height: 20px;
  animation: mushroom-pulse ease-in-out 3s infinite;
}

.mushroom::before {
  /* stem */
  content: '';
  position: absolute;
  bottom: 0;
  left: 50%;
  transform: translateX(-50%);
  width: 4px;
  height: 10px;
  background: rgba(187, 247, 208, 0.6);
  border-radius: 2px;
}

.mushroom::after {
  /* cap */
  content: '';
  position: absolute;
  top: 0;
  left: 50%;
  transform: translateX(-50%);
  width: 14px;
  height: 10px;
  background: radial-gradient(ellipse at 40% 40%, #f0abfc, #a21caf);
  border-radius: 50% 50% 20% 20%;
  box-shadow:
    0 0 6px 2px rgba(192, 132, 252, 0.5),
    0 0 12px 4px rgba(168, 85, 247, 0.2);
}

@keyframes mushroom-pulse {
  0%,
  100% {
    opacity: 0.6;
    filter: brightness(1);
  }
  50% {
    opacity: 1;
    filter: brightness(1.4);
  }
}

/* ============================================================
   CRYSTAL FRAME
   ============================================================ */
.hero {
  position: relative;
  z-index: 10;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 2rem;
  width: 100%;
}

.crystal-frame {
  position: relative;
  padding: 3.5rem 4rem;
  max-width: 560px;
  width: 100%;
  text-align: center;
  background: rgba(5, 20, 10, 0.7);
  backdrop-filter: blur(12px);
  border: 1px solid rgba(74, 222, 128, 0.15);
  border-radius: 2px;
  box-shadow:
    0 0 40px rgba(34, 197, 94, 0.08),
    0 0 80px rgba(34, 197, 94, 0.04),
    inset 0 0 60px rgba(0, 0, 0, 0.4);
}

/* Crystal corner accents */
.crystal-edge {
  position: absolute;
  width: 18px;
  height: 18px;
  border-color: rgba(110, 231, 183, 0.6);
  border-style: solid;
}

.crystal-top-left {
  top: -1px;
  left: -1px;
  border-width: 2px 0 0 2px;
}
.crystal-top-right {
  top: -1px;
  right: -1px;
  border-width: 2px 2px 0 0;
}
.crystal-bottom-left {
  bottom: -1px;
  left: -1px;
  border-width: 0 0 2px 2px;
}
.crystal-bottom-right {
  bottom: -1px;
  right: -1px;
  border-width: 0 2px 2px 0;
}

/* ============================================================
   HERO CONTENT
   ============================================================ */
.hero-inner {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 1.25rem;
}

.eyebrow {
  font-size: 0.75rem;
  letter-spacing: 0.25em;
  text-transform: uppercase;
  color: rgba(110, 231, 183, 0.6);
  margin: 0;
}

.title {
  font-size: 3.5rem;
  font-weight: 400;
  letter-spacing: 0.04em;
  margin: 0;
  line-height: 1.1;
  color: transparent;
  background: linear-gradient(135deg, #d1fae5 20%, #6ee7b7 50%, #a78bfa 80%);
  background-clip: text;
  -webkit-background-clip: text;
  text-shadow: none;
  filter: drop-shadow(0 0 20px rgba(110, 231, 183, 0.4));
}

.tagline {
  font-size: 1rem;
  line-height: 1.8;
  color: rgba(187, 247, 208, 0.65);
  margin: 0;
  font-style: italic;
}

/* ============================================================
   ENTER BUTTON
   ============================================================ */
.btn-enter {
  position: relative;
  display: inline-flex;
  align-items: center;
  justify-content: center;
  margin-top: 0.5rem;
  padding: 0.85rem 2.75rem;
  font-size: 1rem;
  font-family: inherit;
  font-style: italic;
  letter-spacing: 0.06em;
  text-decoration: none;
  color: #d1fae5;
  background: rgba(21, 128, 61, 0.25);
  border: 1px solid rgba(74, 222, 128, 0.35);
  border-radius: 2px;
  cursor: pointer;
  overflow: hidden;
  transition:
    color 0.3s,
    background 0.3s,
    border-color 0.3s;
}

.btn-glow {
  position: absolute;
  inset: 0;
  background: radial-gradient(
    ellipse at 50% 100%,
    rgba(74, 222, 128, 0.18) 0%,
    transparent 70%
  );
  pointer-events: none;
  transition: opacity 0.3s;
  opacity: 1;
}

.btn-enter:hover {
  color: #ecfdf5;
  background: rgba(21, 128, 61, 0.45);
  border-color: rgba(110, 231, 183, 0.6);
}

.btn-enter:hover .btn-glow {
  opacity: 1.5;
}

/* ============================================================
   AUTH LINKS
   ============================================================ */
.auth-links {
  display: flex;
  align-items: center;
  gap: 0.6rem;
  font-size: 0.85rem;
  margin-top: 0.25rem;
}

.link-fae {
  color: rgba(167, 243, 208, 0.55);
  text-decoration: none;
  transition: color 0.2s;
  letter-spacing: 0.03em;
}

.link-fae:hover {
  color: #6ee7b7;
}

.link-divider {
  color: rgba(110, 231, 183, 0.25);
  user-select: none;
}

/* ============================================================
   RESPONSIVE
   ============================================================ */
@media (max-width: 600px) {
  .title {
    font-size: 2.4rem;
  }

  .crystal-frame {
    padding: 2.5rem 2rem;
  }

  .mushroom-ring {
    width: 340px;
    height: 340px;
  }
}
</style>
