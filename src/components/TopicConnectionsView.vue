<script setup lang="ts">
import { ref } from 'vue'
import PlatformHeader from './PlatformHeader.vue'
import { Button } from '@/components/ui/button'
import { Switch } from '@/components/ui/switch'
import { Checkbox } from '@/components/ui/checkbox'
import { Label } from '@/components/ui/label'
import { 
  SendIcon, 
  MessageSquareIcon, 
  AlertCircleIcon, 
  SaveIcon, 
  RefreshCwIcon, 
  CheckCircle2Icon,
  ChevronRightIcon 
} from '@lucide/vue'

const props = defineProps<{
  topic: {
    title: string
    autoRepost?: boolean
    selectedChannels?: string[]
    [key: string]: any
  }
  telegramConnected: boolean
  baleConnected: boolean
}>()

const emit = defineEmits<{
  (e: 'back'): void
  (e: 'save', data: { autoRepost: boolean; selectedChannels: string[] }): void
  (e: 'goToConnections'): void
}>()

const autoRepost = ref(props.topic.autoRepost ?? false)
const selectedChannels = ref<string[]>(props.topic.selectedChannels ?? ['tg_1', 'bale_1'])

const isSaving = ref(false)
const saveSuccess = ref(false)

// Mock channels lists
const telegramChannels = [
  { id: 'tg_1', name: 'کانال رسمی تفکر', type: 'channel', handle: '@tafakor_official' },
  { id: 'tg_2', name: 'گروه توسعه تفکر', type: 'group', handle: '@tafakor_dev' }
]

const baleChannels = [
  { id: 'bale_1', name: 'کانال بله تفکر', type: 'channel', handle: 'tafakor_bale' },
  { id: 'bale_2', name: 'گروه هماهنگی بله', type: 'group', handle: 'tafakor_coord' }
]

const toggleChannel = (channelId: string) => {
  const index = selectedChannels.value.indexOf(channelId)
  if (index === -1) {
    selectedChannels.value.push(channelId)
  } else {
    selectedChannels.value.splice(index, 1)
  }
}

const handleSave = () => {
  isSaving.value = true
  // Simulate network save delay
  setTimeout(() => {
    isSaving.value = false
    saveSuccess.value = true
    emit('save', {
      autoRepost: autoRepost.value,
      selectedChannels: selectedChannels.value
    })
    
    setTimeout(() => {
      saveSuccess.value = false
      emit('back')
    }, 1000)
  }, 1200)
}
</script>

<template>
  <div class="w-full flex flex-col gap-5 text-right font-sans" dir="rtl">
    <!-- Header -->
    <PlatformHeader 
      is-inner-page
      inner-title="تنظیمات اتصالات"
      :show-options-button="false"
      @back="emit('back')"
    />

    <!-- Main Card wrapper -->
    <div class="w-full bg-card/25 dark:bg-card/15 border border-border/50 rounded-3xl p-5 shadow-xs flex flex-col gap-5">
      
      <!-- Auto Repost Main Switch -->
      <div class="flex items-center justify-between p-4 bg-background/40 dark:bg-card/30 rounded-2xl border border-border/50 transition-all duration-300">
        <div class="flex flex-col text-right pl-4">
          <span class="text-xs font-extrabold text-foreground">بازنشر خودکار مطالب</span>
          <span class="text-[9.5px] text-muted-foreground mt-1 leading-normal">
            ارسال و انتشار خودکار مطالب و پیام‌های جدید این تاپیک به کانال‌ها و گروه‌های متصل در پیام‌رسان‌ها
          </span>
        </div>
        <Switch :checked="autoRepost" @update:checked="autoRepost = $event" />
      </div>

      <!-- Channels List Section (Visible only when auto-repost is active) -->
      <transition name="expand-fade">
        <div v-if="autoRepost" class="flex flex-col gap-4.5 overflow-hidden">
          
          <div class="h-[1px] bg-border/40" />
          
          <div class="flex flex-col gap-1.5">
            <h4 class="text-[11px] font-extrabold text-foreground/80 pr-1">کانال‌ها و گروه‌های هدف برای بازنشر:</h4>
            <p class="text-[9px] text-muted-foreground pr-1 leading-normal">
              کانال‌هایی که می‌خواهید مطالب تاپیک به آن‌ها فرستاده شود را انتخاب کنید.
            </p>
          </div>

          <!-- Telegram Connections Group -->
          <div class="flex flex-col gap-2.5">
            <div class="flex items-center gap-1.5 text-xs font-bold text-sky-500 pr-1">
              <span class="size-1.5 rounded-full bg-sky-500 shrink-0" />
              <span>پیام‌رسان تلگرام</span>
            </div>

            <!-- Telegram not connected -->
            <div 
              v-if="!telegramConnected" 
              class="flex flex-col gap-3 p-3.5 rounded-2xl bg-destructive/5 border border-destructive/15 text-right font-sans"
            >
              <div class="flex items-start gap-2.5">
                <AlertCircleIcon class="size-4.5 text-destructive shrink-0 mt-0.5" />
                <div class="flex flex-col gap-1">
                  <span class="text-[11px] font-bold text-destructive">ربات تلگرام متصل نیست</span>
                  <span class="text-[9.5px] text-muted-foreground leading-normal">
                    برای تعریف مقاصد بازنشر در تلگرام، ابتدا باید ربات تلگرام خود را در پروفایل متصل کنید.
                  </span>
                </div>
              </div>
              <Button 
                variant="outline" 
                size="xs" 
                class="self-start text-[10px] h-7 rounded-lg border-destructive/20 text-destructive hover:bg-destructive/10 cursor-pointer pr-2.5 pl-2 gap-1"
                @click="emit('goToConnections')"
              >
                <span>اتصال ربات تلگرام</span>
                <ChevronRightIcon class="size-3.5 rotate-180" />
              </Button>
            </div>

            <!-- Telegram Channels list -->
            <div v-else class="flex flex-col gap-2 bg-background/30 rounded-2xl p-1.5 border border-border/40">
              <div 
                v-for="ch in telegramChannels" 
                :key="ch.id"
                @click="toggleChannel(ch.id)"
                class="flex items-center justify-between p-2.5 rounded-xl hover:bg-muted/40 cursor-pointer transition-colors"
              >
                <div class="flex items-center gap-3">
                  <div class="size-8 rounded-lg bg-sky-500/10 flex items-center justify-center text-sky-500 shrink-0">
                    <SendIcon v-if="ch.type === 'channel'" class="size-4 rotate-45 -translate-x-0.5 translate-y-0.5" />
                    <MessageSquareIcon v-else class="size-4" />
                  </div>
                  <div class="flex flex-col text-right">
                    <span class="text-[11px] font-bold text-foreground">{{ ch.name }}</span>
                    <span class="text-[9px] text-muted-foreground mt-0.5">{{ ch.handle }}</span>
                  </div>
                </div>
                <!-- Checkbox -->
                <Checkbox :checked="selectedChannels.includes(ch.id)" @update:checked="toggleChannel(ch.id)" class="shrink-0" />
              </div>
            </div>
          </div>

          <!-- Bale Connections Group -->
          <div class="flex flex-col gap-2.5">
            <div class="flex items-center gap-1.5 text-xs font-bold text-emerald-500 pr-1">
              <span class="size-1.5 rounded-full bg-emerald-500 shrink-0" />
              <span>پیام‌رسان بله</span>
            </div>

            <!-- Bale not connected -->
            <div 
              v-if="!baleConnected" 
              class="flex flex-col gap-3 p-3.5 rounded-2xl bg-destructive/5 border border-destructive/15 text-right font-sans"
            >
              <div class="flex items-start gap-2.5">
                <AlertCircleIcon class="size-4.5 text-destructive shrink-0 mt-0.5" />
                <div class="flex flex-col gap-1">
                  <span class="text-[11px] font-bold text-destructive">ربات بله متصل نیست</span>
                  <span class="text-[9.5px] text-muted-foreground leading-normal">
                    برای تعریف مقاصد بازنشر در بله، ابتدا باید ربات بله خود را در پروفایل متصل کنید.
                  </span>
                </div>
              </div>
              <Button 
                variant="outline" 
                size="xs" 
                class="self-start text-[10px] h-7 rounded-lg border-destructive/20 text-destructive hover:bg-destructive/10 cursor-pointer pr-2.5 pl-2 gap-1"
                @click="emit('goToConnections')"
              >
                <span>اتصال ربات بله</span>
                <ChevronRightIcon class="size-3.5 rotate-180" />
              </Button>
            </div>

            <!-- Bale Channels list -->
            <div v-else class="flex flex-col gap-2 bg-background/30 rounded-2xl p-1.5 border border-border/40">
              <div 
                v-for="ch in baleChannels" 
                :key="ch.id"
                @click="toggleChannel(ch.id)"
                class="flex items-center justify-between p-2.5 rounded-xl hover:bg-muted/40 cursor-pointer transition-colors"
              >
                <div class="flex items-center gap-3">
                  <div class="size-8 rounded-lg bg-emerald-500/10 flex items-center justify-center text-emerald-500 shrink-0">
                    <SendIcon v-if="ch.type === 'channel'" class="size-4 rotate-45 -translate-x-0.5 translate-y-0.5" />
                    <MessageSquareIcon v-else class="size-4" />
                  </div>
                  <div class="flex flex-col text-right">
                    <span class="text-[11px] font-bold text-foreground">{{ ch.name }}</span>
                    <span class="text-[9px] text-muted-foreground mt-0.5">{{ ch.handle }}</span>
                  </div>
                </div>
                <!-- Checkbox -->
                <Checkbox :checked="selectedChannels.includes(ch.id)" @update:checked="toggleChannel(ch.id)" class="shrink-0" />
              </div>
            </div>
          </div>

        </div>
      </transition>

      <div class="h-[1px] bg-border/40 my-1" />

      <!-- Save Button -->
      <Button 
        @click="handleSave"
        :disabled="isSaving || saveSuccess"
        class="w-full h-11 rounded-xl font-bold transition-all relative overflow-hidden active:scale-[0.98] cursor-pointer"
        :variant="saveSuccess ? 'secondary' : 'default'"
      >
        <transition name="btn-content" mode="out-in">
          <div v-if="isSaving" class="flex items-center gap-2 justify-center">
            <RefreshCwIcon class="size-4 animate-spin" />
            <span>در حال ذخیره‌سازی...</span>
          </div>
          <div v-else-if="saveSuccess" class="flex items-center gap-2 justify-center text-emerald-600 dark:text-emerald-400">
            <CheckCircle2Icon class="size-4" />
            <span>اتصالات با موفقیت به روز رسانی شد</span>
          </div>
          <div v-else class="flex items-center gap-2 justify-center">
            <SaveIcon class="size-4" />
            <span>ذخیره تنظیمات اتصالات</span>
          </div>
        </transition>
      </Button>

    </div>
  </div>
</template>

<style scoped>
.btn-content-enter-active,
.btn-content-leave-active {
  transition: all 0.2s ease-out;
}
.btn-content-enter-from {
  opacity: 0;
  transform: translateY(4px);
}
.btn-content-leave-to {
  opacity: 0;
  transform: translateY(-4px);
}

/* Slide expand transition for auto-repost list */
.expand-fade-enter-active {
  transition: all 0.35s cubic-bezier(0.4, 0, 0.2, 1);
  max-height: 500px;
  opacity: 1;
}
.expand-fade-leave-active {
  transition: all 0.25s cubic-bezier(0.4, 0, 0.2, 1);
  max-height: 500px;
  opacity: 1;
}
.expand-fade-enter-from,
.expand-fade-leave-to {
  max-height: 0;
  opacity: 0;
  margin-top: 0;
  padding-top: 0;
  padding-bottom: 0;
}
</style>
