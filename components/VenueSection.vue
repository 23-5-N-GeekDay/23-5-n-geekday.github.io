<script setup lang="ts">
import { pixelSlideIn, pixelFadeIn, pixelCardFloat, pixelBuildIn, staggerContainer } from '~/composables/usePixelAnimation'

const details = [
  { icon: '📅', label: '日期', value: '2026年2月22-25日' },
  { icon: '⏱️', label: '时长', value: '48小时黑客松 + 极客节开放日' },
  { icon: '🎉', label: '初六地点', value: '汕头高新区潮创智造云谷 2号楼2楼' },
  { icon: '📍', label: '初七至初九', value: '普宁市潮实高级中学 体艺中心' },
  { icon: '🎫', label: '费用', value: '免费参加（需缴防鸽费）' },
]

const benefits = [
  { icon: '🍕', title: '餐食全包', desc: '三餐与无限零食' },
  { icon: '🛏️', title: '住宿支持', desc: '两晚宾馆与休息区域' },
  { icon: '🚗', title: '交通补贴', desc: '潮汕三市跨市大巴集体接送' },
]

const detailRow = {
  hidden: { opacity: 0, x: -20 },
  visibleOnce: {
    opacity: 1,
    x: 0,
    transition: {
      type: 'spring',
      stiffness: 200,
      damping: 20,
    },
  },
}

const benefitCard = {
  hidden: { opacity: 0, x: 30, rotateY: -10 },
  visibleOnce: {
    opacity: 1,
    x: 0,
    rotateY: 0,
    transition: {
      type: 'spring',
      stiffness: 150,
      damping: 15,
    },
  },
}

</script>

<template>
  <section id="venue" class="py-24 px-6">
    <div class="max-w-5xl mx-auto">
      <div
        v-motion
        :variants="pixelSlideIn"
        :initial="pixelSlideIn.initial"
        :visible-once="pixelSlideIn.visibleOnce"
        class="mb-8"
      >
        <span class="font-pixel text-xs text-primary">
          《 WELCOME ABOARD 》
        </span>
      </div>

      <h2
        v-motion
        :variants="pixelFadeIn"
        :initial="pixelFadeIn.initial"
        :visible-once="pixelFadeIn.visibleOnce"
        class="font-pixel-cn text-2xl md:text-3xl mb-12 leading-relaxed"
      >
        活动信息
      </h2>

      <div class="grid md:grid-cols-2 gap-8" style="perspective: 1000px">
        <div
          v-motion
          :variants="pixelCardFloat"
          :initial="pixelCardFloat.initial"
          :visible-once="pixelCardFloat.visibleOnce"
          class="pixel-card p-6"
        >
          <h3 class="font-pixel-cn text-sm mb-6 text-primary">◆ 基本信息</h3>
          <div
            v-motion
            class="space-y-4"
            :variants="staggerContainer"
            :initial="staggerContainer.initial"
            :visible-once="staggerContainer.visibleOnce"
          >
            <div
              v-for="item in details"
              :key="item.label"
              v-motion
              :variants="detailRow"
              :initial="detailRow.hidden"
              :visible-once="detailRow.visible"
              class="flex justify-between items-center border-b border-border/30 pb-3"
              :hover="{ x: 5 }"
            >
              <span class="font-pixel-cn text-lg">
                {{ item.icon }} {{ item.label }}
              </span>
              <span class="font-pixel-cn text-lg text-foreground">{{ item.value }}</span>
            </div>
          </div>
        </div>

        <div
          v-motion
          class="space-y-4"
          :variants="staggerContainer"
          :initial="staggerContainer.initial"
          :visible-once="staggerContainer.visibleOnce"
          style="perspective: 800px"
        >
          <h3
            v-motion
            class="font-pixel-cn text-sm mb-6 text-primary"
            :variants="pixelFadeIn"
            :initial="pixelFadeIn.initial"
            :visible-once="pixelFadeIn.visibleOnce"
          >
            ◆ 参赛者福利
          </h3>
          <div
            v-for="item in benefits"
            :key="item.title"
            v-motion
            :variants="benefitCard"
            :initial="benefitCard.hidden"
            :visible-once="benefitCard.visible"
            class="pixel-card p-4 flex items-center gap-4"
            :hover="{ x: 8, rotateY: 5, boxShadow: '6px 6px 0 0 hsl(35 95% 50% / 0.3)' }"
            :transition="{ type: 'spring', stiffness: 300 }"
          >
            <div
              v-motion
              class="w-12 h-12 flex-shrink-0 flex items-center justify-center text-2xl bg-gradient-to-br from-secondary/20 to-accent/20 border-2 border-secondary/30"
              style="clip-path: polygon(0 4px, 4px 4px, 4px 0, calc(100% - 4px) 0, calc(100% - 4px) 4px, 100% 4px, 100% calc(100% - 4px), calc(100% - 4px) calc(100% - 4px), calc(100% - 4px) 100%, 4px 100%, 4px calc(100% - 4px), 0 calc(100% - 4px))"
              :hover="{ scale: 1.15, rotate: -10 }"
              :transition="{ type: 'spring', stiffness: 300 }"
            >
              {{ item.icon }}
            </div>
            <div>
              <h4 class="font-pixel-cn text-sm text-primary">{{ item.title }}</h4>
              <p class="font-pixel-cn text-lg text-muted-foreground">{{ item.desc }}</p>
            </div>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>
