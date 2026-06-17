<script setup lang="ts">
import { ref, onMounted, watch } from 'vue'
import { 
  ArrowRightIcon, 
  SearchIcon, 
  MoreVerticalIcon 
} from '@lucide/vue'

// Props configuration
const props = withDefaults(defineProps<{
  isInnerPage?: boolean
  title?: string
  innerTitle?: string
  loading?: boolean
  error?: boolean
}>(), {
  isInnerPage: false,
  title: 'تفکر',
  innerTitle: 'گفتگو با پشتیبانی',
  loading: false,
  error: false
})

const emit = defineEmits<{
  (e: 'back'): void
  (e: 'actionClick', actionName: string): void
}>()

// Timer logic to fade out 'اتصال برقرار است' after 4 seconds
const showStatus = ref(true)
let statusTimer: any = null

const startStatusTimer = () => {
  if (statusTimer) clearTimeout(statusTimer)
  showStatus.value = true
  statusTimer = setTimeout(() => {
    showStatus.value = false
  }, 4000)
}

onMounted(() => {
  startStatusTimer()
})

watch(() => props.isInnerPage, (newVal) => {
  if (!newVal) {
    // Navigated back to Home - restart status fade timer
    startStatusTimer()
  } else {
    // Navigated to Inner page - show status instantly, clear timer
    showStatus.value = true
    if (statusTimer) clearTimeout(statusTimer)
  }
})
</script>

<template>
  <div class="w-full flex items-center text-right select-none font-sans" dir="rtl">
    
    <!-- Detachable Back Button (RTL, right-aligned) -->
    <button
      @click="emit('back')"
      :class="[
        'h-11 rounded-xl bg-background/60 backdrop-blur-md flex items-center justify-center text-foreground hover:bg-muted/80 active:scale-95 transition-all duration-300 ease-out shrink-0 cursor-pointer shadow-xs p-0',
        isInnerPage 
          ? 'w-11 opacity-100 ml-1 border border-border/60' 
          : 'w-0 opacity-0 ml-0 border-0 border-transparent overflow-hidden pointer-events-none'
      ]"
      title="بازگشت"
    >
      <ArrowRightIcon class="size-4 text-foreground/80 transition-transform group-hover:-translate-x-0.5" />
    </button>

    <!-- Main Header Card Bar -->
    <div
      :class="[
        'flex-1 h-11 rounded-xl border bg-background/60 backdrop-blur-md flex items-center justify-between px-3.5 transition-all duration-300 relative overflow-hidden shadow-xs',
        isInnerPage ? 'border-border/60' : 'border-border/80'
      ]"
    >
      <!-- Subtle Loading Bar Progress at the bottom -->
      <div
        v-if="loading"
        class="absolute bottom-0 left-0 right-0 h-0.5 bg-primary/20 overflow-hidden"
      >
        <div class="absolute inset-y-0 bg-primary w-1/3 animate-infinite-loading" />
      </div>

      <!-- Title & Status Section (RTL: right side) -->
      <div class="flex items-center gap-2">
        <!-- Dynamic Error Glow Indicator -->
        <span 
          v-if="error" 
          class="size-2 rounded-full bg-destructive animate-pulse shrink-0" 
          title="خطای ارتباطی"
        />
        
        <div class="flex flex-col text-right">
          <span class="text-xs font-extrabold text-foreground transition-all duration-300">
            {{ isInnerPage ? innerTitle : title }}
          </span>
          <transition name="status-fade">
            <span 
              v-if="isInnerPage || showStatus"
              class="text-[8.5px] font-medium transition-all duration-300 mt-0.5 text-muted-foreground block overflow-hidden"
            >
              {{ isInnerPage ? 'در حال مکالمه...' : 'اتصال برقرار است' }}
            </span>
          </transition>
        </div>
      </div>

      <!-- Actions Section (RTL: left side) -->
      <div class="flex items-center gap-1">
        <slot name="actions">
          <button 
            @click="emit('actionClick', 'search')"
            class="size-7.5 rounded-md hover:bg-muted/80 flex items-center justify-center text-muted-foreground hover:text-foreground transition-colors cursor-pointer"
            title="جستجو"
          >
            <SearchIcon class="size-4" />
          </button>
          <button 
            @click="emit('actionClick', 'options')"
            class="size-7.5 rounded-md hover:bg-muted/80 flex items-center justify-center text-muted-foreground hover:text-foreground transition-colors cursor-pointer"
            title="گزینه‌ها"
          >
            <MoreVerticalIcon class="size-4" />
          </button>
        </slot>
      </div>

    </div>
  </div>
</template>

<style scoped>
@keyframes loading-progress {
  0% { transform: translateX(250%); }
  100% { transform: translateX(-250%); }
}

.animate-infinite-loading {
  animation: loading-progress 1.6s infinite linear;
}

/* Status text fade transitions */
.status-fade-enter-active,
.status-fade-leave-active {
  transition: all 0.4s cubic-bezier(0.4, 0, 0.2, 1);
  max-height: 15px;
}

.status-fade-enter-from,
.status-fade-leave-to {
  opacity: 0;
  max-height: 0px;
  transform: translateY(-2px);
  margin-top: 0px;
}

.status-fade-enter-to,
.status-fade-leave-from {
  opacity: 1;
  max-height: 15px;
  transform: translateY(0);
  margin-top: 2px;
}
</style>
