<script setup lang="ts">
import PixelButton from './PixelButton.vue'
import { useRegisterModal } from '~/composables/useRegisterModal'

const { openModal } = useRegisterModal()
</script>

<template>
  <section class="min-h-screen flex flex-col items-center justify-center px-6 relative overflow-hidden">
    <!-- Hero-specific background - ensure it doesn't interfere with page background -->
    <div class="absolute inset-0 -z-10 bg-gradient-to-br from-[#1a1a4e] via-[#2d1b69] to-[#1a1a4e]" />

    <div class="absolute top-0 right-0 -z-10 w-2/3 h-2/5 bg-gradient-to-bl from-orange-500/40 via-yellow-500/25 to-transparent" />

    <div class="absolute bottom-0 left-0 -z-10 w-1/2 h-1/3 bg-gradient-to-tr from-cyan-500/15 via-blue-500/10 to-transparent" />



    <div class="absolute inset-0 overflow-hidden pointer-events-none">
      <div
        v-motion
        class="absolute top-[35%] left-1/2 -translate-x-1/2 w-[500px] md:w-[700px] h-[200px] md:h-[280px] border border-white/15 rounded-full"
        style="transform: translate(-50%, -50%) rotateX(70deg)"
      />

      <div
        v-motion
        class="absolute top-[38%] left-1/2 -translate-x-1/2 w-[350px] md:w-[500px] h-[140px] md:h-[180px] border border-white/10 rounded-full"
        style="transform: translate(-50%, -50%) rotateX(70deg)"
      />

      <div
        v-motion
        class="absolute top-[22%] left-[8%] md:left-[12%] px-3 py-1 bg-green-500/20 backdrop-blur-sm rounded text-green-400 font-mono text-sm border border-green-400/30"
        :animate="{ y: [0, -8, 0] }"
        :transition="{ duration: 4000, repeat: Infinity }"
      >
        {"&lt;hello&gt;"}
      </div>

      <div
        v-motion
        class="absolute top-[18%] right-[8%] md:right-[12%] px-3 py-1 bg-green-500/20 backdrop-blur-sm rounded text-green-400 font-mono text-sm border border-green-400/30"
        :animate="{ y: [0, -6, 0] }"
        :transition="{ duration: 5000, repeat: Infinity, delay: 1000 }"
      >
        23.5°N
      </div>

      <div
        v-motion
        class="absolute top-[28%] right-[20%] text-white/25 font-mono text-xs hidden md:block"
        :animate="{ opacity: [0.15, 0.35, 0.15] }"
        :transition="{ duration: 3000, repeat: Infinity }"
      >
        e35c2b94e8476563
      </div>

      <div
        v-motion
        class="absolute top-[25%] left-[18%] text-white/20 font-mono text-xs hidden md:block"
        :animate="{ opacity: [0.1, 0.3, 0.1] }"
        :transition="{ duration: 4000, repeat: Infinity, delay: 1000 }"
      >
        4e17908c32b
      </div>

      <div
        v-motion
        class="absolute top-[15%] left-[3%] hidden lg:flex flex-col items-center gap-1"
        :animate="{ opacity: [0.5, 0.8, 0.5] }"
        :transition="{ duration: 6000, repeat: Infinity }"
      >
        <div class="w-8 h-8 rounded-full border-2 border-green-400/50 flex items-center justify-center">
          <div class="w-2 h-2 rounded-full bg-green-400/80" />
        </div>
        <span class="text-green-400/70 text-[10px] font-mono">23.5° N</span>
        <span class="text-green-400/70 text-[10px] font-mono">GEEKDAY</span>
      </div>

      <div class="absolute top-[12%] right-[4%] flex gap-2">
        <div class="w-3 h-3 rounded-full bg-green-400/80" />
        <div class="w-3 h-3 rounded-full bg-green-400/40" />
      </div>
    </div>

    <div class="max-w-5xl mx-auto text-center relative z-10">
      <div
        v-motion
        :initial="{ opacity: 0 }"
        :visible="{ opacity: 1 }"
        :transition="{ duration: 600, delay: 500 }"
        class="mb-6"
      >
        <div class="flex flex-col sm:flex-row items-center justify-center gap-3">
          <span class="font-mono text-2xl md:text-3xl text-white/90 tracking-[0.35em]">
            2026.02.
          </span>
          <span class="keycap font-pixel text-xl md:text-2xl">23-25</span>
        </div>
      </div>

      <p
        v-motion
        :initial="{ opacity: 0 }"
        :visible="{ opacity: 1 }"
        :transition="{ duration: 600, delay: 600 }"
        class="text-lg md:text-xl font-pixel-cn text-white/80 mb-8"
      >
        用 <span class="text-cyan-300 font-bold">创造</span> · 触碰 <span class="text-orange-300 font-bold">认知边界</span>
      </p>

      <div
        v-motion
        :initial="{ opacity: 0, y: 30 }"
        :visible="{ opacity: 1, y: 0 }"
        :transition="{ duration: 800, delay: 700 }"
        class="mb-8"
      >
        <h1 class="text-5xl md:text-7xl lg:text-8xl font-bold tracking-tight text-white mb-2">
          <span class="font-pixel">23.5°N</span>
        </h1>
        <h2 class="text-4xl md:text-6xl lg:text-7xl font-bold tracking-widest">
          <span class="font-pixel mario-text">GEEKDAY</span>
        </h2>
      </div>

      <div
        v-motion
        :initial="{ opacity: 0, y: 20 }"
        :visible="{ opacity: 1, y: 0 }"
        :transition="{ duration: 600, delay: 900 }"
        class="flex flex-col sm:flex-row gap-4 justify-center items-center"
      >
        <PixelButton @click="openModal">
          立即报名
        </PixelButton>
        <a href="https://mp.weixin.qq.com/s/2yIlxKBQouacdL06iv6jrA" target="_blank">
          <PixelButton variant="outline" :show-dots="false">
            了解更多
          </PixelButton>
        </a>
      </div>
    </div>

    <div
      v-motion
      :initial="{ opacity: 0 }"
      :visible="{ opacity: 1 }"
      :transition="{ delay: 1000 }"
      class="absolute bottom-[25%] right-[8%] hidden md:block"
    >
      <div
        v-motion
        :animate="{ opacity: [1, 0, 1] }"
        :transition="{ duration: 1000, repeat: Infinity }"
        class="w-3 h-8 bg-green-400"
      />
    </div>

    <div
      v-motion
      :initial="{ opacity: 0 }"
      :visible="{ opacity: 1 }"
      :transition="{ delay: 1200 }"
      class="absolute bottom-6 left-1/2 -translate-x-1/2"
    >
      <div
        v-motion
        :animate="{ y: [0, 8, 0] }"
        :transition="{ duration: 1500, repeat: Infinity }"
        class="font-pixel-body text-2xl text-white/50"
      >
        ▼
      </div>
    </div>
  </section>
</template>

<style scoped>
.date-glow {
  text-shadow:
    0 0 12px rgba(129, 140, 248, 0.55),
    0 0 24px rgba(129, 140, 248, 0.45),
    0 0 36px rgba(129, 140, 248, 0.35);
}

/* Keycap group container */
.keycap-group {
  display: flex;
  align-items: center;
  gap: 0.5rem;
}

/* Individual keycap styling */
.keycap {
  position: relative;
  display: inline-flex;
  align-items: center;
  justify-content: center;
  min-width: 3rem;
  padding: 0.6rem 1rem;
  
  /* Gradient background - dark with subtle purple tint */
  background: linear-gradient(
    135deg,
    rgba(45, 35, 85, 0.95) 0%,
    rgba(30, 25, 60, 0.98) 50%,
    rgba(25, 20, 50, 1) 100%
  );
  
  /* 3D border effect */
  border: 1px solid rgba(139, 92, 246, 0.4);
  border-radius: 8px;
  
  /* Text styling */
  color: #c4b5fd;
  letter-spacing: 0.1em;
  
  /* 3D shadow for depth - key visual effect */
  box-shadow:
    /* Top highlight */
    inset 0 1px 0 rgba(255, 255, 255, 0.08),
    /* Inner shadow for concave effect */
    inset 0 -2px 6px rgba(0, 0, 0, 0.3),
    /* Base shadow - creates 3D lift */
    0 4px 0 rgba(20, 15, 40, 0.95),
    /* Soft shadow below */
    0 6px 12px rgba(0, 0, 0, 0.4),
    /* Neon glow */
    0 0 20px rgba(139, 92, 246, 0.25),
    0 0 40px rgba(139, 92, 246, 0.15);
  
  /* Smooth transition for hover effects */
  transition: all 0.15s cubic-bezier(0.4, 0, 0.2, 1);
  cursor: pointer;
  
  /* Text shadow for digital glow */
  text-shadow:
    0 0 8px rgba(196, 181, 253, 0.6),
    0 0 16px rgba(139, 92, 246, 0.4);
}

/* Scanline overlay for digital feel */
.keycap::before {
  content: '';
  position: absolute;
  inset: 0;
  background: repeating-linear-gradient(
    0deg,
    transparent,
    transparent 2px,
    rgba(255, 255, 255, 0.015) 2px,
    rgba(255, 255, 255, 0.015) 4px
  );
  border-radius: 7px;
  pointer-events: none;
}

/* Glowing border effect */
.keycap::after {
  content: '';
  position: absolute;
  inset: -2px;
  background: linear-gradient(
    135deg,
    rgba(139, 92, 246, 0.5) 0%,
    rgba(99, 102, 241, 0.3) 25%,
    rgba(139, 92, 246, 0.1) 50%,
    rgba(99, 102, 241, 0.3) 75%,
    rgba(139, 92, 246, 0.5) 100%
  );
  border-radius: 10px;
  z-index: -1;
  opacity: 0.6;
  animation: keycap-glow 3s ease-in-out infinite;
}

/* Hover state - pressed effect */
.keycap:hover {
  transform: translateY(2px);
  
  box-shadow:
    inset 0 1px 0 rgba(255, 255, 255, 0.08),
    inset 0 -1px 4px rgba(0, 0, 0, 0.3),
    0 2px 0 rgba(20, 15, 40, 0.95),
    0 3px 8px rgba(0, 0, 0, 0.4),
    0 0 30px rgba(139, 92, 246, 0.4),
    0 0 60px rgba(139, 92, 246, 0.2);
  
  color: #e9d5ff;
  text-shadow:
    0 0 12px rgba(233, 213, 255, 0.8),
    0 0 24px rgba(139, 92, 246, 0.6);
}

.keycap:hover::after {
  opacity: 1;
}

/* Active state - fully pressed */
.keycap:active {
  transform: translateY(4px);
  
  box-shadow:
    inset 0 2px 4px rgba(0, 0, 0, 0.4),
    0 0px 0 rgba(20, 15, 40, 0.95),
    0 1px 4px rgba(0, 0, 0, 0.3),
    0 0 40px rgba(139, 92, 246, 0.5);
}

/* Dash keycap - slightly smaller */
.keycap-dash {
  min-width: 2rem;
  padding: 0.6rem 0.6rem;
}

/* Pulsing glow animation */
@keyframes keycap-glow {
  0%, 100% {
    opacity: 0.4;
    filter: blur(4px);
  }
  50% {
    opacity: 0.7;
    filter: blur(6px);
  }
}

/* Responsive adjustments */
@media (min-width: 768px) {
  .keycap {
    min-width: 3.5rem;
    padding: 0.75rem 1.25rem;
    border-radius: 10px;
  }
  
  .keycap-dash {
    min-width: 2.5rem;
    padding: 0.75rem 0.75rem;
  }
  
  .keycap-group {
    gap: 0.75rem;
  }
}
</style>
