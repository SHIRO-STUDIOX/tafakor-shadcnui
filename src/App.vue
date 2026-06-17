<script setup lang="ts">
import { ConfigProvider } from 'reka-ui'
import { useDark, useToggle } from '@vueuse/core'
import { 
  SunIcon, 
  MoonIcon, 
  MonitorIcon
} from '@lucide/vue'

// Dark/Light theme manager via VueUse
const isDark = useDark({
  selector: 'html',
  attribute: 'class',
  valueDark: 'dark',
  valueLight: '',
})
const toggleDark = useToggle(isDark)
</script>

<template>
  <ConfigProvider dir="rtl">
    <div class="min-h-screen bg-background text-foreground font-sans transition-colors duration-300 flex flex-col" dir="rtl">
      
      <!-- Main Platform Header -->
      <header class="sticky top-0 z-50 w-full border-b border-border/80 bg-background/80 backdrop-blur-md flex justify-center">
        <div class="w-full max-w-6xl px-6 h-16 flex items-center justify-between">
          
          <!-- Logo & Brand -->
          <div class="flex items-center gap-2">
            <div class="size-8 rounded-lg bg-primary flex items-center justify-center text-primary-foreground shadow-sm">
              <MonitorIcon class="size-4" />
            </div>
            <div>
              <span class="font-extrabold tracking-tight text-sm">محصول اصلی AVA</span>
              <span class="text-[9px] block text-muted-foreground mt-0.5 leading-none">محیط تولید و توسعه اصلی</span>
            </div>
          </div>

          <!-- Utilities (Theme Switcher) -->
          <div class="flex items-center gap-2">
            <button 
              @click="toggleDark()" 
              class="size-8 rounded-lg border border-border/80 bg-background flex items-center justify-center hover:bg-muted transition-colors shadow-sm outline-none cursor-pointer"
              title="تغییر تم"
            >
              <SunIcon v-if="isDark" class="size-4 text-amber-500" />
              <MoonIcon v-else class="size-4 text-primary" />
            </button>
          </div>

        </div>
      </header>

      <!-- Main Application Container -->
      <main class="flex-1 w-full bg-linear-to-b from-background via-card/10 to-muted/20">
        <router-view />
      </main>

    </div>
  </ConfigProvider>
</template>