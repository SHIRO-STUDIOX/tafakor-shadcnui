<script setup lang="ts">
import { computed } from 'vue'
import { ChevronLeftIcon } from '@lucide/vue'

const props = withDefaults(defineProps<{
  title: string
  description: string
  icon: any // Accepts a Lucide icon component reference
  badgeText?: string
  badgeVariant?: 'default' | 'secondary' | 'destructive'
}>(), {
  badgeVariant: 'default'
})

const emit = defineEmits<{
  (e: 'click'): void
}>()

// Compute badge variant classes
const badgeClasses = computed(() => {
  switch (props.badgeVariant) {
    case 'destructive':
      return 'bg-destructive/10 text-destructive border border-destructive/25'
    case 'secondary':
      return 'bg-secondary text-muted-foreground border border-border/60'
    case 'default':
    default:
      return 'bg-primary/10 text-primary border border-primary/25'
  }
})
</script>

<template>
  <div
    @click="emit('click')"
    class="w-full flex items-center justify-between p-3.5 rounded-xl bg-card/45 backdrop-blur-md border border-border/40 hover:bg-muted/40 hover:border-primary/20 transition-all duration-200 cursor-pointer select-none text-right relative active:scale-[0.995] group"
    dir="rtl"
  >
    <!-- Right side: Icon Container -->
    <div class="size-10 rounded-lg bg-primary/10 border border-primary/20 flex items-center justify-center text-primary shrink-0 transition-transform duration-200 group-hover:scale-102">
      <component :is="icon" class="size-5 shrink-0" />
    </div>

    <!-- Middle: Title & Description -->
    <div class="flex-1 flex flex-col mr-3.5 text-right gap-1 min-w-0">
      <span class="text-xs font-bold text-foreground truncate select-all leading-none">
        {{ title }}
      </span>
      <span class="text-[10px] text-muted-foreground truncate leading-relaxed">
        {{ description }}
      </span>
    </div>

    <!-- Left side: Badge & Arrow indicator -->
    <div class="flex items-center gap-2 shrink-0 mr-2">
      <!-- Badge element -->
      <span
        v-if="badgeText"
        :class="[
          'px-2 py-0.5 rounded-md text-[8.5px] font-black tracking-wide select-none',
          badgeClasses
        ]"
      >
        {{ badgeText }}
      </span>

      <!-- RTL pointing back chevron -->
      <ChevronLeftIcon class="size-4 text-muted-foreground/60 transition-transform duration-200 group-hover:-translate-x-0.5" />
    </div>
  </div>
</template>
