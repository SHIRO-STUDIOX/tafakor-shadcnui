<script setup lang="ts">
import { computed } from 'vue'
import { PencilIcon } from '@lucide/vue'
import { Badge } from '@/components/ui/badge'

const props = withDefaults(defineProps<{
  name: string
  phone: string
  avatar?: string
  statusText?: string
  statusVariant?: 'success' | 'warning' | 'info' | 'muted'
}>(), {
  statusText: 'فعال',
  statusVariant: 'success'
})

const emit = defineEmits<{
  (e: 'edit'): void
}>()

// Generate fallback letter
const nameLetter = computed(() => {
  return props.name ? props.name.trim().charAt(0) : 'ک'
})

// Compute badge styling based on status variant
const statusClasses = computed(() => {
  switch (props.statusVariant) {
    case 'warning':
      return 'bg-amber-500/10 border-amber-500/25 text-amber-500'
    case 'info':
      return 'bg-blue-500/10 border-blue-500/25 text-blue-500'
    case 'muted':
      return 'bg-muted border-border/60 text-muted-foreground'
    case 'success':
    default:
      return 'bg-emerald-500/10 border-emerald-500/25 text-emerald-650 dark:text-emerald-400'
  }
})
</script>

<template>
  <div
    class="w-full bg-card/45 backdrop-blur-md border border-border/40 rounded-2xl p-6 py-9 flex flex-col items-center justify-center text-center relative select-none shadow-xs"
    dir="rtl"
  >
    <!-- 40x40px Circular Avatar -->
    <div class="relative shrink-0 select-none">
      <img
        v-if="avatar"
        :src="avatar"
        class="size-10 rounded-full object-cover ring-2 ring-primary/20 ring-offset-2 ring-offset-background shadow-xs"
        alt="Profile Avatar"
      />
      <div
        v-else
        class="size-10 rounded-full bg-primary/10 border border-primary/20 flex items-center justify-center text-primary text-xs font-bold ring-2 ring-primary/10 ring-offset-2 ring-offset-background shadow-xs"
      >
        {{ nameLetter }}
      </div>
    </div>

    <!-- User Name & Inline Edit Button Row -->
    <div class="flex items-center justify-center gap-1.5 mt-3.5 w-full">
      <span class="text-xs font-extrabold text-foreground truncate select-all leading-none">
        {{ name }}
      </span>
      <button
        @click="emit('edit')"
        class="size-5 rounded-md flex items-center justify-center hover:bg-muted/60 text-muted-foreground hover:text-primary active:scale-90 transition-all cursor-pointer border border-transparent"
        title="ویرایش حساب کاربری"
      >
        <PencilIcon class="size-3 shrink-0" />
      </button>
    </div>

    <!-- Phone Number (Muted Importance) -->
    <span class="text-[10px] text-muted-foreground/80 font-mono mt-1 select-all font-semibold">
      {{ phone }}
    </span>

    <!-- Status Badge -->
    <div class="mt-3.5">
      <Badge
        :class="[
          'inline-flex items-center px-2.5 py-0.5 rounded-full text-[8.5px] font-bold border tracking-wide select-none',
          statusClasses
        ]"
      >
        {{ statusText }}
      </Badge>
    </div>
  </div>
</template>
