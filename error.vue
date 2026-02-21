<template>
  <div class="min-h-screen">
    <!-- Navbar spacer -->
    <div class="h-16"></div>
    
    <div class="max-w-4xl mx-auto px-4 py-8">
      <!-- Terminal Window -->
      <div class="pixel-card overflow-hidden">
        <!-- Terminal Header -->
        <div class="flex items-center gap-2 px-4 py-3 bg-card border-b-2 border-border/50">
          <div class="flex gap-2">
            <span class="w-3 h-3 rounded-full bg-red-500/80"></span>
            <span class="w-3 h-3 rounded-full bg-yellow-500/80"></span>
            <span class="w-3 h-3 rounded-full bg-green-500/80"></span>
          </div>
          <span class="ml-4 font-pixel text-xs text-muted-foreground flex-1 text-center">
            ~/geekday/error
          </span>
          <NuxtLink
            to="/"
            class="font-pixel text-xs text-primary hover:text-primary/80 transition-colors"
          >
            [HOME]
          </NuxtLink>
        </div>
        
        <!-- Terminal Content -->
        <div class="p-6 bg-[#0a0f0a]/80 backdrop-blur-sm font-mono">
          <!-- Command prompt -->
          <div class="flex items-start gap-2 mb-6">
            <span class="text-primary font-pixel text-xs whitespace-nowrap">geek@23.5N:~$</span>
            <span class="font-pixel-cn text-sm text-foreground/80">cat {{ error?.statusCode || 404 }}.log</span>
          </div>
          
          <!-- Separator -->
          <div class="border-t border-dashed border-primary/30 mb-6"></div>
          
          <!-- Error Display -->
          <div class="text-center py-12">
            <div class="mb-8">
              <span class="font-pixel text-8xl md:text-9xl text-red-400">
                {{ error?.statusCode || 404 }}
              </span>
            </div>
            
            <h1 class="font-pixel-cn text-2xl md:text-3xl text-gradient mb-4">
              {{ errorTitle }}
            </h1>
            
            <p class="font-pixel-cn text-lg text-muted-foreground mb-8 max-w-md mx-auto">
              {{ errorDescription }}
            </p>
            
            <!-- ASCII Art -->
            <pre class="font-mono text-primary/60 text-xs mb-8 leading-tight">
    ¯\_(ツ)_/¯
            </pre>
          </div>
          
          <!-- Interactive Terminal -->
          <div class="border-t border-dashed border-primary/30 pt-6">
            <!-- Command history -->
            <div v-for="(entry, index) in commandHistory" :key="index" class="mb-4">
              <div class="flex items-start gap-2">
                <span class="text-primary font-pixel text-xs whitespace-nowrap">geek@23.5N:~$</span>
                <span class="font-pixel-cn text-sm text-foreground/80">{{ entry.command }}</span>
              </div>
              <div 
                v-if="entry.output" 
                :class="['ml-[88px] font-pixel-cn text-sm mt-1', entry.isError ? 'text-red-400' : 'text-muted-foreground']"
              >
                <span v-html="entry.output"></span>
              </div>
            </div>
            
            <!-- Command input -->
            <div class="flex items-center gap-2">
              <span class="text-primary font-pixel text-xs whitespace-nowrap">geek@23.5N:~$</span>
              <input
                ref="terminalInput"
                v-model="currentCommand"
                @keydown.enter="executeCommand"
                type="text"
                class="flex-1 bg-transparent border-none outline-none font-pixel-cn text-sm text-foreground/80 caret-primary"
                placeholder="输入 cd / 回到首页..."
                autocomplete="off"
                spellcheck="false"
              />
              <span class="w-2 h-4 bg-primary animate-pulse"></span>
            </div>
          </div>
        </div>
      </div>
      
      <!-- Quick Actions -->
      <div class="flex justify-center gap-4 mt-8">
        <button
          @click="handleError"
          class="pixel-card px-6 py-3 font-pixel-cn text-sm text-primary hover:text-foreground hover:border-primary/50 transition-colors"
        >
          ← 返回首页
        </button>
        <button
          @click="goBack"
          class="pixel-card px-6 py-3 font-pixel-cn text-sm text-muted-foreground hover:text-foreground hover:border-primary/50 transition-colors"
        >
          返回上一页
        </button>
      </div>
    </div>
    
    <!-- Footer -->
    <Footer />
  </div>
</template>

<script setup lang="ts">
import { withBase, withoutBase } from 'ufo'

const props = defineProps({
  error: Object
})

const route = useRoute()
const router = useRouter()
const baseURL = useRuntimeConfig().app.baseURL || '/'
const logicalPath = computed(() => withoutBase(route.path, baseURL) || '/')
const toBase = (path: string) => withBase(path, baseURL)

const errorTitle = computed(() => {
  switch (props.error?.statusCode) {
    case 404:
      return '页面走丢了'
    case 500:
      return '服务器开小差了'
    case 403:
      return '禁止访问'
    default:
      return '出了点问题'
  }
})

const errorDescription = computed(() => {
  switch (props.error?.statusCode) {
    case 404:
      return '你访问的页面可能已被移动、删除，或者根本不存在。试试用下面的终端导航吧！'
    case 500:
      return '服务器遇到了一些问题，我们的极客团队正在紧急修复中...'
    case 403:
      return '你没有权限访问这个页面。也许你需要先成为一名极客？'
    default:
      return props.error?.message || '发生了未知错误，请稍后再试。'
  }
})

const handleError = () => clearError({ redirect: toBase('/') })
const goBack = () => router.back()

// Interactive terminal logic
interface CommandEntry {
  command: string
  output: string
  isError: boolean
}

const terminalInput = ref<HTMLInputElement | null>(null)
const currentCommand = ref('')
const commandHistory = ref<CommandEntry[]>([])

const availablePages = [
  { path: '/', desc: '首页 - 北回归线极客节官网' },
  { path: '/about', desc: '关于北归节 - 赛事简介与背景' },
  { path: '/faq', desc: '常见问题 - 报名与参赛须知' },
  { path: '/prizes', desc: '奖项设置 - 奖金池与评分标准' },
  { path: '/sponsors', desc: '合作伙伴 - 指导单位与赞助商' },
  { path: '/schedule/day0', desc: 'DAY 0 - Warm-up 派对之夜' },
  { path: '/schedule/day1', desc: 'DAY 1 - Kick Off 开幕 + Hackathon 启动' },
  { path: '/schedule/day2', desc: 'DAY 2 - Hackathon 全天开发' },
  { path: '/schedule/day3', desc: 'DAY 3 - 公众开放日 · 闭幕活动' },
  { path: '/tracks/ai-agent', desc: 'AI Agent 赛道 - 大模型智能体' },
  { path: '/tracks/embodied-ai', desc: '具身智能赛道 - 机器人开发' },
  { path: '/workshops/ai-agent-dev', desc: 'AI Agent 工作坊 - 开发实战' },
  { path: '/workshops/embodied-ai', desc: '具身智能工作坊 - 入门教程' },
  { path: '/resources', desc: '赞助商资源指南 - 平台资源与启用方式总览' },
  { path: '/resources/ai-agent', desc: 'AI Agent 赛道资源 - TRAE/Tosea/算能启用指南' },
  { path: '/resources/embodied-ai', desc: 'Embodied AI 赛道资源 - 硬件设备与模型使用指南' },
]

const funnyErrors = [
  "🤖 404 页面也有终端？是的，我们就是这么极客！",
  "🎮 迷路了？输入 find 看看有哪些页面~",
  "💻 这个页面不存在，但你的热情存在！",
  "🚀 命令未找到，但北回归线永远找得到！",
]

const findPagesOutput = `<span class="text-primary">📂 可用页面列表:</span><br><br><span class="text-yellow-400">/ 首页</span><br>  └─ <span class="text-cyan-400">/about</span>          关于北归节<br>  └─ <span class="text-cyan-400">/faq</span>            常见问题<br>  └─ <span class="text-cyan-400">/prizes</span>         奖项设置<br>  └─ <span class="text-cyan-400">/sponsors</span>       合作伙伴<br><br><span class="text-yellow-400">/schedule/ 活动日程</span><br>  └─ <span class="text-cyan-400">/schedule/day0</span>  DAY 0 (Warm-up 派对之夜)<br>  └─ <span class="text-cyan-400">/schedule/day1</span>  DAY 1 (Kick Off 开幕 + Hackathon 启动)<br>  └─ <span class="text-cyan-400">/schedule/day2</span>  DAY 2 (Hackathon 全天开发)<br>  └─ <span class="text-cyan-400">/schedule/day3</span>  DAY 3 (公众开放日 · 闭幕活动)<br><br><span class="text-yellow-400">/tracks/ 赛道介绍</span><br>  └─ <span class="text-cyan-400">/tracks/ai-agent</span>     AI Agent 大模型智能体<br>  └─ <span class="text-cyan-400">/tracks/embodied-ai</span>  具身智能 机器人开发<br><br><span class="text-yellow-400">/workshops/ 技术工作坊</span><br>  └─ <span class="text-cyan-400">/workshops/ai-agent-dev</span>  AI Agent 开发实战<br>  └─ <span class="text-cyan-400">/workshops/embodied-ai</span>   具身智能入门<br><br><span class="text-yellow-400">/resources/ 赞助商资源指南</span><br>  └─ <span class="text-pink-400">/resources</span>              资源总览<br>  └─ <span class="text-pink-400">/resources/ai-agent</span>     AI Agent 赛道资源<br>  └─ <span class="text-pink-400">/resources/embodied-ai</span>  Embodied AI 赛道资源<br><br><span class="text-muted-foreground">使用 cd &lt;路径&gt; 导航，例如: cd /resources/ai-agent</span>`

function resetTerminal() {
  commandHistory.value = [
    {
      command: 'status',
      output: `<span class="text-red-400">404</span> <span class="text-muted-foreground">${logicalPath.value}</span>`,
      isError: true,
    },
    {
      command: 'pwd',
      output: `<span class="text-primary">${logicalPath.value}</span>`,
      isError: false,
    },
    {
      command: 'find',
      output: findPagesOutput,
      isError: false,
    },
  ]
  currentCommand.value = ''
}

function executeCommand() {
  const cmd = currentCommand.value.trim()
  if (!cmd) return
  
  const parts = cmd.split(/\s+/)
  const command = parts[0].toLowerCase()
  const args = parts.slice(1).join(' ')
  
  let output = ''
  let isError = false
  
  switch (command) {
    case 'cd':
      handleCd(args)
      return
      
    case 'find':
      {
        const normalized = normalizePath(args)
        if (!normalized || normalized === '/') {
          output = findPagesOutput
          break
        }
        const found = availablePages.find(p => p.path === normalized)
        if (found) {
          output = `<span class="text-cyan-400">${found.path}</span> <span class="text-muted-foreground">${found.desc}</span>`
        } else {
          output = `<span class="text-red-400">未找到 ${normalized}</span><br><span class="text-muted-foreground">输入 find 查看可用路径</span>`
          isError = true
        }
      }
      break
      
    case 'ls':
      output = `<span class="text-cyan-400">/                        </span> 首页<br>
<span class="text-cyan-400">/about                   </span> 关于北归节<br>
<span class="text-cyan-400">/faq                     </span> 常见问题<br>
<span class="text-cyan-400">/prizes                  </span> 奖项设置<br>
<span class="text-cyan-400">/sponsors                </span> 合作伙伴<br>
<span class="text-cyan-400">/schedule/day0           </span> DAY 0 日程<br>
<span class="text-cyan-400">/schedule/day1           </span> DAY 1 日程<br>
<span class="text-cyan-400">/schedule/day2           </span> DAY 2 日程<br>
<span class="text-cyan-400">/schedule/day3           </span> DAY 3 日程<br>
<span class="text-cyan-400">/tracks/ai-agent         </span> AI Agent 赛道<br>
<span class="text-cyan-400">/tracks/embodied-ai      </span> 具身智能赛道<br>
<span class="text-cyan-400">/workshops/ai-agent-dev  </span> AI Agent 工作坊<br>
<span class="text-cyan-400">/workshops/embodied-ai   </span> 具身智能工作坊<br>
<span class="text-pink-400">/resources               </span> 赞助商资源指南<br>
<span class="text-pink-400">/resources/ai-agent      </span> AI Agent 赛道资源<br>
<span class="text-pink-400">/resources/embodied-ai   </span> Embodied AI 赛道资源`
      break
      
    case 'help':
      output = `<span class="text-primary">可用命令:</span><br>  <span class="text-cyan-400">cd &lt;path&gt;</span>  - 导航到指定路径<br>  <span class="text-cyan-400">cd /</span>       - 回到首页<br>  <span class="text-cyan-400">ls</span>         - 列出可用页面<br>  <span class="text-cyan-400">find</span>       - 显示完整页面树<br>  <span class="text-cyan-400">download</span>   - 下载参赛秩序手册<br>  <span class="text-cyan-400">clear</span>      - 清空终端<br>  <span class="text-cyan-400">help</span>       - 显示此帮助`
      break
      
    case 'pwd':
      output = `<span class="text-primary">${logicalPath.value}</span>`
      break
      
    case 'clear':
      commandHistory.value = []
      currentCommand.value = ''
      return
    
    case 'download':
    case 'dl':
    case 'get':
      output = '📥 正在下载参赛秩序手册...'
      window.open('https://raw.githubusercontent.com/23-5-N-GeekDay/GeekDay2026/refs/heads/main/2026%E5%8C%97%E5%9B%9E%E5%BD%92%E7%BA%BF%E6%9E%81%E5%AE%A2%E8%8A%82%E9%BB%91%E5%AE%A2%E9%A9%AC%E6%8B%89%E6%9D%BE%E5%8F%82%E8%B5%9B%E7%A7%A9%E5%BA%8F%E5%86%8C.pdf', '_blank')
      break
      
    default:
      output = `<span class="text-red-400">bash: ${command}: command not found</span><br><span class="text-muted-foreground">${funnyErrors[Math.floor(Math.random() * funnyErrors.length)]}</span>`
      isError = true
  }
  
  commandHistory.value.push({ command: cmd, output, isError })
  currentCommand.value = ''
}

function handleCd(path: string) {
  if (!path || path === '~') {
    // cd with no args or ~ goes to home
    clearError({ redirect: toBase('/') })
    return
  }
  
  if (path === '/') {
    clearError({ redirect: toBase('/') })
    return
  }
  
  if (path === '..') {
    // Go up one level
    const segments = logicalPath.value.split('/').filter(Boolean)
    if (segments.length <= 1) {
      clearError({ redirect: toBase('/') })
    } else {
      segments.pop()
      clearError({ redirect: toBase('/' + segments.join('/')) })
    }
    return
  }
  
  if (path === '-') {
    // Go back (like cd -)
    router.back()
    return
  }
  
  // Handle absolute path
  if (path.startsWith('/')) {
    clearError({ redirect: toBase(path) })
    return
  }
  
  // Handle relative path - append to current path
  const basePath = logicalPath.value.endsWith('/') ? logicalPath.value : logicalPath.value + '/'
  let newPath = basePath + path
  
  // Normalize the path (handle ..)
  const segments = newPath.split('/').filter(Boolean)
  const normalized: string[] = []
  for (const seg of segments) {
    if (seg === '..') {
      normalized.pop()
    } else if (seg !== '.') {
      normalized.push(seg)
    }
  }
  
  clearError({ redirect: toBase('/' + normalized.join('/')) })
}

function normalizePath(input: string) {
  if (!input) return '/'
  const stripped = withoutBase(input.trim(), baseURL)
  return stripped.startsWith('/') ? stripped : '/' + stripped
}

watch(
  logicalPath,
  () => {
    resetTerminal()
  },
  { immediate: true }
)

onMounted(() => {
  // Focus input
  nextTick(() => {
    terminalInput.value?.focus()
  })
})
</script>

<style>
input::placeholder {
  @apply text-muted-foreground/50;
}
</style>
