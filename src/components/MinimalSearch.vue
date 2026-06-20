<script setup lang="ts">
import { ref, nextTick } from 'vue'
import { SearchIcon, XIcon } from '@lucide/vue'
import { Input } from '@/components/ui/input'

// Props
const props = withDefaults(defineProps<{
  compact?: boolean
}>(), {
  compact: false
})

// V-model bindings
const query = defineModel<string>({ default: '' })
const isExpanded = defineModel<boolean>('expanded', { default: true })

const emit = defineEmits<{
  (e: 'search', q: string): void
  (e: 'clear'): void
}>()

// Input element reference for autofocus (inline and modal)
const inlineInputRef = ref<InstanceType<typeof Input> | null>(null)
const modalInputRef = ref<InstanceType<typeof Input> | null>(null)

// Modal open state
const isModalOpen = ref(false)

// Open search modal
const openModal = async () => {
  isModalOpen.value = true
  await nextTick()
  if (modalInputRef.value) {
    const el = (modalInputRef.value as any).$el || modalInputRef.value
    el.focus?.()
  }
}

// Close search modal
const closeModal = () => {
  isModalOpen.value = false
}

// Clear search input
const handleClear = () => {
  query.value = ''
  if (!props.compact && inlineInputRef.value) {
    const el = (inlineInputRef.value as any).$el || inlineInputRef.value
    el.focus?.()
  } else if (props.compact && modalInputRef.value) {
    const el = (modalInputRef.value as any).$el || modalInputRef.value
    el.focus?.()
  }
  emit('clear')
}

// Handle keypress inside input
const handleKeyDown = (e: KeyboardEvent) => {
  if (e.key === 'Enter') {
    emit('search', query.value)
    if (props.compact) {
      closeModal()
    }
  }
}
</script>

<template>
  <div class="w-full" dir="rtl">
    <!-- COMPACT STATE: Search icon button only -->
    <div v-if="props.compact" class="flex justify-end">
      <button
        type="button"
        @click="openModal"
        class="size-11 rounded-xl bg-background/60 backdrop-blur-md border border-border/80 flex items-center justify-center hover:bg-muted/40 hover:border-primary/20 text-muted-foreground hover:text-foreground active:scale-95 transition-all cursor-pointer shadow-xs"
        title="جستجو"
      >
        <SearchIcon class="size-4.5 shrink-0" />
      </button>

      <!-- Search Modal Overlay -->
      <transition name="modal-fade">
        <div 
          v-if="isModalOpen" 
          class="fixed inset-0 z-50 flex items-start justify-center p-4 pt-24 bg-black/40 backdrop-blur-xs"
          @click.self="closeModal"
        >
          <!-- Modal Container -->
          <div 
            class="w-full max-w-[480px] bg-card/95 border border-border/60 rounded-xl shadow-xl overflow-hidden animate-in zoom-in-95 duration-200 flex flex-col p-4.5 gap-4.5 text-right"
            @click.stop
          >
            <!-- Header -->
            <div class="flex items-center justify-between border-b border-border/40 pb-2">
              <span class="text-xs font-extrabold text-foreground">جستجو در پلتفرم</span>
              <button 
                @click="closeModal"
                class="size-7 rounded-lg flex items-center justify-center text-muted-foreground hover:text-foreground hover:bg-muted/80 transition-colors cursor-pointer"
                title="بستن"
              >
                <XIcon class="size-4 shrink-0" />
              </button>
            </div>

            <!-- Modal Search Input -->
            <div class="relative w-full">
              <Input
                ref="modalInputRef"
                v-slot:default
                v-model="query"
                type="text"
                placeholder="جستجو..."
                class="h-11 w-full bg-background/60 border border-border/80 rounded-xl pr-10 pl-10 focus-visible:border-primary/50 focus-visible:ring-2 focus-visible:ring-primary/20 text-xs font-bold text-foreground placeholder:text-muted-foreground/60 shadow-xs font-sans"
                @keydown="handleKeyDown"
              />

              <!-- Search Icon inside Modal -->
              <div 
                class="absolute right-3.5 top-1/2 -translate-y-1/2 flex items-center justify-center pointer-events-none text-muted-foreground transition-colors duration-200 size-4 z-10"
                :class="{ 'text-primary': query }"
              >
                <SearchIcon class="size-4 shrink-0" />
              </div>

              <!-- Clear Action inside Modal -->
              <div class="absolute left-2.5 top-1/2 -translate-y-1/2 flex items-center gap-1 z-10">
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
          </div>
        </div>
      </transition>
    </div>

    <!-- STANDARD STATE: Full inline input -->
    <div v-else class="relative w-full">
      <Input
        ref="inlineInputRef"
        v-model="query"
        type="text"
        placeholder="جستجو..."
        class="h-11 w-full bg-background/60 backdrop-blur-md border border-border/80 rounded-xl pr-10 pl-10 focus-visible:border-primary/50 focus-visible:ring-2 focus-visible:ring-primary/20 text-xs font-bold text-foreground placeholder:text-muted-foreground/60 shadow-xs font-sans"
        @keydown="handleKeyDown"
      />

      <!-- Search Icon -->
      <div 
        class="absolute right-3.5 top-1/2 -translate-y-1/2 flex items-center justify-center pointer-events-none text-muted-foreground transition-colors duration-200 size-4 z-10"
        :class="{ 'text-primary': query }"
      >
        <SearchIcon class="size-4 shrink-0" />
      </div>

      <!-- Action Buttons Container (Absolute Left) -->
      <div class="absolute left-2.5 top-1/2 -translate-y-1/2 flex items-center gap-1 z-10">
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
  </div>
</template>

<style scoped>
.modal-fade-enter-active,
.modal-fade-leave-active {
  transition: opacity 0.2s ease;
}

.modal-fade-enter-from,
.modal-fade-leave-to {
  opacity: 0;
}
</style>
