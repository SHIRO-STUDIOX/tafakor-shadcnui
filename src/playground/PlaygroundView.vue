<script setup lang="ts">
import { ref, reactive } from 'vue'
import UserProfileCard from '@/components/UserProfileCard.vue'
import { Button } from '@/components/ui/button'
import { Label } from '@/components/ui/label'
import { 
  FlaskConicalIcon,
  SmartphoneIcon, 
  TabletIcon, 
  LaptopIcon, 
  PlayIcon,
  RefreshCwIcon,
  InfoIcon,
  TerminalIcon,
  CheckCircle2Icon
} from '@lucide/vue'

// Simulator configs
const viewportWidth = ref<'100%' | '768px' | '375px'>('100%')
const activeState = ref<'idle' | 'loading' | 'error' | 'empty'>('idle')

// Mock user data structure
interface UserProfile {
  name: string
  email: string
  role: string
  status: 'active' | 'inactive'
}

const initialUser: UserProfile = {
  name: 'شایان مهدوی',
  email: 'shayan@example.com',
  role: 'طراح رابط کاربری (UI/UX)',
  status: 'active'
}
const mockUser = reactive<UserProfile>({ ...initialUser })

// Event logging system
interface SystemEvent {
  id: number
  time: string
  name: string
  data: string
}
const eventLog = ref<SystemEvent[]>([])
let logId = 0

const logEvent = (name: string, data: any) => {
  const now = new Date()
  const timeStr = now.toTimeString().split(' ')[0] ?? ''
  eventLog.value.unshift({
    id: ++logId,
    time: timeStr,
    name,
    data: JSON.stringify(data) ?? ''
  })
}

// Event Handlers
const handleSave = (updatedUser: UserProfile) => {
  mockUser.name = updatedUser.name
  mockUser.email = updatedUser.email
  mockUser.status = updatedUser.status
  logEvent('Save Button Clicked', updatedUser)
}

const handleStatusToggle = () => {
  mockUser.status = mockUser.status === 'active' ? 'inactive' : 'active'
  logEvent('Status Toggle Clicked', { newStatus: mockUser.status })
}

const resetData = () => {
  Object.assign(mockUser, initialUser)
  activeState.value = 'idle'
  eventLog.value = []
  logEvent('Simulator Reset', {})
}
</script>

<template>
  <div class="w-full flex justify-center text-right" dir="rtl">
    <div class="w-full max-w-6xl py-10 px-6 flex flex-col gap-10">
      
      <!-- Header (Centered alignment, smaller font size) -->
      <div class="border-b border-border/60 pb-6 flex flex-col md:flex-row md:items-end md:justify-between gap-6 w-full text-right">
        <div class="flex flex-col gap-2">
          <div class="flex items-center gap-2 text-primary justify-start">
            <FlaskConicalIcon class="size-4" />
            <span class="text-[10px] font-bold uppercase tracking-wider text-primary/80">محیط ۳ - آزمایشگاه تست کامپوننت</span>
          </div>
          <h1 class="text-xl font-bold tracking-tight text-foreground">شبیه‌ساز و زمین بازی (Playground)</h1>
          <p class="text-muted-foreground text-xs mt-0.5">تست رفتار ریسپانسیو و کنترل کامل وضعیت‌های مختلف کامپوننت‌های بزرگتر.</p>
        </div>
        
        <!-- Reset button -->
        <Button variant="outline" size="sm" @click="resetData" class="self-start md:self-auto flex items-center gap-1.5 cursor-pointer">
          <RefreshCwIcon class="size-3.5" />
          ریست شبیه‌ساز
        </Button>
      </div>

      <!-- Playground Workspace Panel (Increased Gap) -->
      <div class="grid grid-cols-1 lg:grid-cols-4 gap-10 w-full">
        
        <!-- Control Panel (Sidebar - Clean flex gap spacing) -->
        <div class="lg:col-span-1 bg-card/30 border border-border/60 rounded-xl p-5 shadow-xs flex flex-col gap-6 self-start">
          <h2 class="text-xs font-bold border-r-4 border-primary pr-2 leading-none text-foreground/90">تنظیمات شبیه‌ساز</h2>

          <!-- Viewport controls -->
          <div class="flex flex-col gap-3">
            <Label class="text-[11px] font-semibold text-muted-foreground block">اندازه پنجره نمایشگر:</Label>
            <div class="grid grid-cols-3 gap-1.5 bg-muted/60 p-1 rounded-lg border border-border/40">
              <button 
                @click="viewportWidth = '375px'"
                :class="[
                  'flex flex-col items-center justify-center py-2 rounded-md text-[10px] font-medium transition-colors outline-none cursor-pointer',
                  viewportWidth === '375px' ? 'bg-background text-foreground shadow-xs font-bold' : 'text-muted-foreground hover:text-foreground'
                ]"
              >
                <SmartphoneIcon class="size-4 mb-1" />
                موبایل
              </button>
              <button 
                @click="viewportWidth = '768px'"
                :class="[
                  'flex flex-col items-center justify-center py-2 rounded-md text-[10px] font-medium transition-colors outline-none cursor-pointer',
                  viewportWidth === '768px' ? 'bg-background text-foreground shadow-xs font-bold' : 'text-muted-foreground hover:text-foreground'
                ]"
              >
                <TabletIcon class="size-4 mb-1" />
                تبلت
              </button>
              <button 
                @click="viewportWidth = '100%'"
                :class="[
                  'flex flex-col items-center justify-center py-2 rounded-md text-[10px] font-medium transition-colors outline-none cursor-pointer',
                  viewportWidth === '100%' ? 'bg-background text-foreground shadow-xs font-bold' : 'text-muted-foreground hover:text-foreground'
                ]"
              >
                <LaptopIcon class="size-4 mb-1" />
                دسکتاپ
              </button>
            </div>
          </div>

          <!-- Component state controls -->
          <div class="flex flex-col gap-2.5">
            <Label class="text-[11px] font-semibold text-muted-foreground block">وضعیت کامپوننت:</Label>
            <div class="flex flex-col gap-1.5">
              <button 
                v-for="state in (['idle', 'loading', 'error', 'empty'] as const)"
                :key="state"
                @click="activeState = state"
                :class="[
                  'w-full text-right px-3 py-2.5 text-xs font-medium rounded-lg border transition-all flex items-center justify-between cursor-pointer',
                  activeState === state 
                    ? 'bg-primary/5 text-primary border-primary/30 font-bold' 
                    : 'bg-background/20 border-border/40 text-foreground hover:bg-muted/40'
                ]"
              >
                <span>
                  {{ 
                    state === 'idle' ? 'حالت فعال (Idle)' :
                    state === 'loading' ? 'بارگذاری (Loading)' :
                    state === 'error' ? 'نمایش خطا (Error)' : 'کامپوننت خالی (Empty)' 
                  }}
                </span>
                <span class="size-2 rounded-full" 
                  :class="[
                    state === 'idle' ? 'bg-green-500' :
                    state === 'loading' ? 'bg-amber-500 animate-pulse' :
                    state === 'error' ? 'bg-destructive' : 'bg-muted-foreground'
                  ]" 
                />
              </button>
            </div>
          </div>

          <!-- Dev Note -->
          <div class="p-3 bg-muted/40 border border-border/40 rounded-lg text-[10px] leading-relaxed text-muted-foreground flex gap-2">
            <InfoIcon class="size-3.5 text-primary shrink-0 mt-0.5" />
            <span>تغییر وضعیت یا تغییر ابعاد، رفتار کامپوننت را در یک محدوده عایق و ایزوله شبیه‌سازی می‌کند.</span>
          </div>
        </div>

        <!-- Main Testing Sandbox -->
        <div class="lg:col-span-3 flex flex-col gap-6">
          
          <!-- Live Preview Sandbox -->
          <div class="bg-card/30 border border-border/60 rounded-xl p-6 shadow-xs flex flex-col items-center">
            <div class="w-full flex justify-between items-center text-[10px] text-muted-foreground mb-4">
              <span class="font-mono">Width: {{ viewportWidth === '100%' ? 'Desktop (Auto)' : viewportWidth }}</span>
              <span class="flex items-center gap-1">
                <span class="size-1.5 rounded-full bg-emerald-500 animate-ping" />
                پیش‌نمایش زنده کامپوننت
              </span>
            </div>

            <!-- Viewport Frame Wrapper with Resizable styling -->
            <div 
              :style="{ maxWidth: viewportWidth }" 
              class="w-full border border-dashed border-border bg-background/50 rounded-xl p-4 transition-all duration-300 ease-in-out shadow-xs overflow-hidden flex justify-center"
            >
              <div class="w-full max-w-md">
                <UserProfileCard 
                  :state="activeState" 
                  :user="mockUser" 
                  @save="handleSave"
                  @status-toggle="handleStatusToggle"
                />
              </div>
            </div>
          </div>

          <!-- Interactive Event Log Console -->
          <div class="bg-card/30 border border-border/60 rounded-xl p-5 shadow-xs flex flex-col gap-3">
            <div class="flex items-center justify-between">
              <h3 class="text-xs font-bold text-foreground flex items-center gap-1.5">
                <TerminalIcon class="size-3.5 text-primary" />
                کنسول ثبت رویدادها (Events Log)
              </h3>
              <span class="text-[9px] bg-secondary/80 px-2 py-0.5 rounded border border-border/50 font-mono text-muted-foreground">Reactive Logs</span>
            </div>

            <div class="bg-background/80 border border-border/60 rounded-lg p-3 h-36 font-mono text-[10px] overflow-y-auto flex flex-col gap-2 scrollbar-thin">
              <div v-if="eventLog.length === 0" class="text-muted-foreground text-center py-8">
                هیچ رویدادی ثبت نشده است. روی دکمه‌های کامپوننت کلیک کنید.
              </div>
              <div 
                v-for="event in eventLog" 
                :key="event.id" 
                class="flex items-start justify-between border-b border-border/20 pb-1.5 last:border-0"
              >
                <div class="flex items-start gap-2 text-right">
                  <span class="text-emerald-500 font-bold">▶</span>
                  <div>
                    <span class="text-primary font-bold ml-1">[{{ event.name }}]</span>
                    <span class="text-muted-foreground leading-normal break-all">{{ event.data }}</span>
                  </div>
                </div>
                <span class="text-[9px] text-muted-foreground shrink-0 font-mono">{{ event.time }}</span>
              </div>
            </div>
          </div>

        </div>

      </div>

    </div>
  </div>
</template>
