<script setup lang="ts">
import { ref, watch, nextTick } from 'vue'
import { SearchIcon, XIcon, ArrowRightIcon } from '@lucide/vue'

// V-model bindings
const query = defineModel<string>({ default: '' })
const isExpanded = defineModel<boolean>('expanded', { default: false })

const emit = defineEmits<{
  (e: 'search', q: string): void
  (e: 'clear'): void
  (e: 'expand'): void
  (e: 'collapse'): void
}>()

// Input element reference for autofocus
const inputRef = ref<HTMLInputElement | null>(null)

// Handle container click to expand
const handleContainerClick = () => {
  if (!isExpanded.value) {
    isExpanded.value = true
    emit('expand')
  }
}

// Watch expanded state to autofocus or clear
watch(isExpanded, (expandedValue) => {
  if (expandedValue) {
    nextTick(() => {
      inputRef.value?.focus()
    })
  } else {
    query.value = ''
    emit('collapse')
  }
})

// Clear search input
const handleClear = () => {
  query.value = ''
  inputRef.value?.focus()
  emit('clear')
}

// Collapse search bar
const handleCollapse = (e: Event) => {
  e.stopPropagation()
  isExpanded.value = false
}

// Handle keypress inside input
const handleKeyDown = (e: KeyboardEvent) => {
  if (e.key === 'Escape') {
    isExpanded.value = false
    inputRef.value?.blur()
  } else if (e.key === 'Enter') {
    emit('search', query.value)
  }
}
</script>

<template>
  <div
    @click="handleContainerClick"
    :class="[
      'relative flex items-center h-11 bg-background/60 backdrop-blur-md border transition-all duration-300 ease-in-out shadow-xs overflow-hidden select-none',
      isExpanded 
        ? 'w-full rounded-xl border-border/80 pr-10 pl-18 focus-within:border-primary/50 focus-within:ring-2 focus-within:ring-primary/20' 
        : 'w-11 rounded-full border-border/60 hover:bg-muted/40 hover:border-border cursor-pointer'
    ]"
    dir="rtl"
  >
    <!-- Search Icon (Centered when collapsed, anchored right when expanded) -->
    <div 
      class="absolute right-3.5 top-1/2 -translate-y-1/2 flex items-center justify-center pointer-events-none text-muted-foreground transition-colors duration-200 size-4"
      :class="{ 'text-primary': isExpanded && query }"
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
      :class="[
        'w-full bg-transparent border-0 outline-none text-xs font-bold text-foreground placeholder:text-muted-foreground/60 transition-opacity duration-200 py-2.5 font-sans',
        isExpanded ? 'opacity-100 pointer-events-auto' : 'opacity-0 pointer-events-none'
      ]"
    />

    <!-- Action Buttons Container (Absolute Left) -->
    <div 
      class="absolute left-2.5 top-1/2 -translate-y-1/2 flex items-center gap-1 transition-all duration-200"
      :class="isExpanded ? 'opacity-100 pointer-events-auto' : 'opacity-0 pointer-events-none'"
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

      <!-- Collapse/Back Button -->
      <button
        @click="handleCollapse"
        class="size-7 rounded-lg flex items-center justify-center hover:bg-muted/60 text-muted-foreground hover:text-primary active:scale-95 transition-all cursor-pointer border border-transparent"
        title="بستن جستجو"
      >
        <!-- ArrowRight pointing right representing back action in RTL -->
        <ArrowRightIcon class="size-4 shrink-0" />
      </button>
    </div>
  </div>
</template>
