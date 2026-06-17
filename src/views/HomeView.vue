<script setup lang="ts">
import { MonitorIcon, SparklesIcon } from '@lucide/vue'
</script>

<template>
  <div class="w-full flex justify-center py-12 px-6" dir="rtl">
    <div class="w-full max-w-4xl">
      <div class="relative bg-card/40 backdrop-blur-md border border-border/80 rounded-2xl p-10 shadow-xs overflow-hidden flex flex-col items-center justify-center">
        <!-- Ambient background decoration -->
        <div class="absolute -top-20 -right-20 size-64 bg-primary/5 rounded-full blur-3xl pointer-events-none" />
        <div class="absolute -bottom-20 -left-20 size-64 bg-primary/10 rounded-full blur-3xl pointer-events-none" />
        
        <div class="relative flex flex-col items-center text-center gap-6 max-w-xl">
          <!-- Minimalist Icon -->
          <div class="size-14 rounded-xl bg-secondary/80 flex items-center justify-center border border-border/60 shadow-xs">
            <MonitorIcon class="size-6 text-primary" />
          </div>
          
          <div class="flex flex-col gap-2.5 items-center">
            <h1 class="text-xl font-bold tracking-tight text-foreground flex items-center gap-2.5 justify-center">
              محیط محصول اصلی
              <span class="text-[10px] font-bold px-2 py-0.5 rounded-full bg-primary/10 text-primary flex items-center gap-1">
                <SparklesIcon class="size-3" />
                آماده توسعه
              </span>
            </h1>
            <p class="text-muted-foreground text-xs leading-relaxed mt-1">
              این محیط برای پیاده‌سازی محصول نهایی و منطق تجاری آن در نظر گرفته شده است. هم‌اکنون ساختار پروژه آماده است تا طراحی‌های اختصاصی خود را در این بخش آغاز کنید.
            </p>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
