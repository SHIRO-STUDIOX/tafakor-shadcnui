<script setup lang="ts">
import { ref } from 'vue'
import { SearchIcon, XIcon } from '@lucide/vue'

// V-model bindings
const query = defineModel<string>({ default: '' })
const isExpanded = defineModel<boolean>('expanded', { default: true })

const emit = defineEmits<{
  (e: 'search', q: string): void
  (e: 'clear'): void
}>()

// Input element reference for autofocus
const inputRef = ref<HTMLInputElement | null>(null)

// Clear search input
const handleClear = () => {
  query.value = ''
  inputRef.value?.focus()
  emit('clear')
}

// Handle keypress inside input
const handleKeyDown = (e: KeyboardEvent) => {
  if (e.key === 'Enter') {
    emit('search', query.value)
  }
}
</script>

<template>
  <div
    class="relative flex items-center h-11 bg-background/60 backdrop-blur-md border border-border/80 w-full rounded-xl pr-10 pl-10 focus-within:border-primary/50 focus-within:ring-2 focus-within:ring-primary/20 shadow-xs select-none"
    dir="rtl"
  >
    <!-- Search Icon -->
    <div 
      class="absolute right-3.5 top-1/2 -translate-y-1/2 flex items-center justify-center pointer-events-none text-muted-foreground transition-colors duration-200 size-4"
      :class="{ 'text-primary': query }"
    >
      <SearchIcon class="size-4 shrink-0" />
    </div>

    <!-- Text Input Area -->
    <input
      ref="inputRef"
      type="text"
      v-model="query"
      @keydown="handleKeyDown"
      placeholder="جستجو..."
      class="w-full bg-transparent border-0 outline-none text-xs font-bold text-foreground placeholder:text-muted-foreground/60 py-2.5 font-sans"
    />

    <!-- Action Buttons Container (Absolute Left) -->
    <div 
      class="absolute left-2.5 top-1/2 -translate-y-1/2 flex items-center gap-1"
    >
      <!-- Clear Text Button -->
      <button
        v-if="query"
        @click.stop="handleClear"
        class="size-7 rounded-lg flex items-center justify-center hover:bg-muted/60 text-muted-foreground hover:text-foreground active:scale-95 transition-all cursor-pointer border border-transparent"
        title="پاک کردن متن"
      >
        <XIcon class="size-3.5 shrink-0" />
      </button>
    </div>
  </div>
</template>
