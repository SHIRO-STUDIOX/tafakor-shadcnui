<script setup lang="ts">
import { ref, reactive, computed } from 'vue'
import UserProfileCard from '@/components/UserProfileCard.vue'
import LoginCard from '@/components/LoginCard.vue'
import PlatformHeader from '@/components/PlatformHeader.vue'
import PlatformNavigation from '@/components/PlatformNavigation.vue'
import TopicCard from '@/components/TopicCard.vue'
import MinimalSearch from '@/components/MinimalSearch.vue'
import ProfileOptionCard from '@/components/ProfileOptionCard.vue'
import ProfileCard from '@/components/ProfileCard.vue'
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
  CheckCircle2Icon,
  UserIcon,
  KeyRoundIcon,
  SignalIcon,
  WifiIcon,
  BatteryIcon,
  SendIcon,
  HomeIcon,
  SearchIcon,
  ArrowRightIcon,
  SettingsIcon,
  LinkIcon,
  SparklesIcon,
  PencilIcon
} from '@lucide/vue'

// Simulator configs
const currentComponent = ref<'profile' | 'login' | 'header' | 'nav' | 'topic' | 'search' | 'profile-option' | 'profile-card-header'>('profile')
const viewportWidth = ref<'100%' | '768px' | '375px'>('100%')
const activeState = ref<'idle' | 'loading' | 'error' | 'empty' | 'success'>('idle')

// Compute available states based on the active component
const availableStates = computed(() => {
  if (currentComponent.value === 'profile') {
    return ['idle', 'loading', 'error', 'empty'] as const
  } else {
    return ['idle', 'loading', 'error', 'success'] as const
  }
})

const handleComponentChange = (comp: 'profile' | 'login' | 'header' | 'nav' | 'topic' | 'search' | 'profile-option' | 'profile-card-header') => {
  currentComponent.value = comp
  activeState.value = 'idle'
  logEvent('Component Changed', { component: comp })
}

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
  simulatorIsInnerPage.value = false
  simulatorActiveTab.value = 'home'
  simulatorSearchQuery.value = ''
  simulatorSearchExpanded.value = false
  simulatorSettingsBadge.value = ''
  simulatorConnectionsBadge.value = '۳ فعال'
  simulatorAiBadge.value = 'جدید'
  simulatorProfileCardName.value = 'شایان مهدوی'
  simulatorProfileCardPhone.value = '۰۹۱۲۳۴۵۶۷۸۹'
  simulatorProfileCardStatus.value = 'success'
  mockTopics.value = [
    { id: 1, title: 'گروه توسعه تفکر', description: 'شایان: طرح هدر پلتفرم را اصلاح کردم و انیمیشن بک به درستی کار می‌کند.', unreadCount: 5, time: '۱۲:۳۰', labels: ['فناوری', 'توسعه'], pinned: true },
    { id: 2, title: 'کانال عمومی اخبار پلتفرم', description: 'اطلاعیه: نسخه بتای آزمایشگاه تست شبیه‌سازها هم‌اکنون به صورت آزمایشی در دسترس قرار گرفت.', unreadCount: 12, time: '۱۰:۱۵', labels: ['اخبار', 'تفکر'], pinned: false },
    { id: 3, title: 'تیم طراحی رابط کاربری', description: 'امیر: لطفا آیکون‌ها و رنگ‌های شیشه‌ای را مجدداً هماهنگ کنید.', unreadCount: 0, time: 'دیروز', labels: ['طراحی', 'UI/UX'], pinned: false }
  ]
  eventLog.value = []
  logEvent('Simulator Reset', {})
}

// Login card specific handlers
const handleLoginPhoneSubmit = (phone: string) => {
  logEvent('Login: Phone Submitted', { phone })
}

const handleLoginOtpSubmit = (otp: string) => {
  logEvent('Login: OTP Code Submitted', { otp })
}

const handleLoginStateChange = (newState: string) => {
  logEvent('Login: Component Internal State', { state: newState })
  if (newState === 'success') {
    activeState.value = 'success'
  } else if (newState === 'idle') {
    activeState.value = 'idle'
  }
}

// Platform Header simulation variables
const simulatorIsInnerPage = ref(false)
const simulatorInnerTitle = ref('پشتیبانی تفکر')

const mockChats = [
  { id: 1, name: 'پشتیبانی تفکر', message: 'سلام! انیمیشن تفکیک دکمه...', time: '۱۲:۳۰', avatar: 'پ' },
  { id: 2, name: 'کانال اطلاعیه‌ها', message: 'نسخه جدید دشبورد منتشر شد.', time: '۱۰:۱۵', avatar: 'ا' },
  { id: 3, name: 'امیرحسین رضایی', message: 'طرح کارت لاگین رو بررسی کردی؟', time: 'دیروز', avatar: 'ا' }
]

const enterChat = (chat: typeof mockChats[0]) => {
  simulatorInnerTitle.value = chat.name
  simulatorIsInnerPage.value = true
  logEvent('Header: Navigation to Chat', { chatName: chat.name })
}

const handleHeaderBack = () => {
  simulatorIsInnerPage.value = false
  logEvent('Header: Back Button Clicked', { action: 'Go back to Home' })
}

const handleHeaderAction = (actionName: string) => {
  logEvent(`Header: Action Clicked [${actionName}]`, {})
}

// Platform Navigation simulation variables
const simulatorActiveTab = ref<'home' | 'profile'>('home')

const handleNavChange = (tab: 'home' | 'profile') => {
  logEvent('Nav: Active Tab Changed', { tab })
}

// Topic Card simulation state
const mockTopics = ref([
  { id: 1, title: 'گروه توسعه تفکر', description: 'شایان: طرح هدر پلتفرم را اصلاح کردم و انیمیشن بک به درستی کار می‌کند.', unreadCount: 5, time: '۱۲:۳۰', labels: ['فناوری', 'توسعه'], pinned: true },
  { id: 2, title: 'کانال عمومی اخبار پلتفرم', description: 'اطلاعیه: نسخه بتای آزمایشگاه تست شبیه‌سازها هم‌اکنون به صورت آزمایشی در دسترس قرار گرفت.', unreadCount: 12, time: '۱۰:۱۵', labels: ['اخبار', 'تفکر'], pinned: false },
  { id: 3, title: 'تیم طراحی رابط کاربری', description: 'امیر: لطفا آیکون‌ها و رنگ‌های شیشه‌ای را مجدداً هماهنگ کنید.', unreadCount: 0, time: 'دیروز', labels: ['طراحی', 'UI/UX'], pinned: false }
])

const handleTopicClick = (topicId: number) => {
  const topic = mockTopics.value.find(t => t.id === topicId)
  if (topic) {
    topic.unreadCount = 0 // clear unread count on click
    logEvent('Topic: Clicked Card (Marked Read)', { id: topicId, title: topic.title })
  }
}

const addSimulatedMessage = () => {
  // Add a random unread count to a topic
  const randomIndex = Math.floor(Math.random() * mockTopics.value.length)
  const topic = mockTopics.value[randomIndex]
  if (topic) {
    topic.unreadCount += 1
    topic.time = new Date().toTimeString().split(' ')[0]?.slice(0, 5) ?? 'حالا'
    topic.description = 'پیام آزمایشی جدید در ثانیه جاری ارسال شد.'
    logEvent('Topic: Simulated Message Received', { title: topic.title })
  }
}

// Minimal Search simulation variables
const simulatorSearchQuery = ref('')
const simulatorSearchExpanded = ref(false)

const handleSearchEvent = (event: string, payload: any) => {
  logEvent(`Search: ${event}`, payload)
}

const filteredTopics = computed(() => {
  if (!simulatorSearchQuery.value.trim()) {
    return mockTopics.value
  }
  const searchLower = simulatorSearchQuery.value.toLowerCase()
  return mockTopics.value.filter(t => 
    t.title.toLowerCase().includes(searchLower) || 
    t.description.toLowerCase().includes(searchLower) ||
    t.labels.some(l => l.toLowerCase().includes(searchLower))
  )
})

// Profile menu options simulation variables
const simulatorSettingsBadge = ref('')
const simulatorConnectionsBadge = ref('۳ فعال')
const simulatorAiBadge = ref('جدید')

const handleProfileOptionClick = (option: string) => {
  logEvent('Profile Option Clicked', { option })
}

// ProfileCard simulation variables
const simulatorProfileCardName = ref('شایان مهدوی')
const simulatorProfileCardPhone = ref('۰۹۱۲۳۴۵۶۷۸۹')
const simulatorProfileCardStatus = ref<'success' | 'warning' | 'info' | 'muted'>('success')

const handleProfileCardEdit = () => {
  logEvent('ProfileCard: Edit Button Clicked', { name: simulatorProfileCardName.value })
}
</script>

<template>
  <div class="w-full flex justify-center text-right" dir="rtl">
    <div class="w-full max-w-6xl py-10 px-6 flex flex-col gap-10">
      
      <!-- Header -->
      <div class="border-b border-border/60 pb-6 flex flex-col md:flex-row md:items-end md:justify-between gap-6 w-full text-right">
        <div class="flex flex-col gap-2">
          <div class="flex items-center gap-2 text-primary justify-start">
            <FlaskConicalIcon class="size-4" />
            <span class="text-[10px] font-bold uppercase tracking-wider text-primary/80">محیط ۳ - آزمایشگاه تست کامپوننت</span>
          </div>
          <h1 class="text-xl font-extrabold tracking-tight text-foreground">شبیه‌ساز و زمین بازی (Playground)</h1>
          <p class="text-muted-foreground text-xs mt-0.5">تست رفتار ریسپانسیو و کنترل کامل وضعیت‌های مختلف کامپوننت‌های بزرگتر.</p>
        </div>
        
        <!-- Reset button -->
        <Button variant="outline" size="sm" @click="resetData" class="self-start md:self-auto flex items-center gap-1.5 cursor-pointer">
          <RefreshCwIcon class="size-3.5" />
          ریست شبیه‌ساز
        </Button>
      </div>

      <!-- Playground Workspace Panel -->
      <div class="grid grid-cols-1 lg:grid-cols-4 gap-10 w-full">
        
        <!-- Control Panel (Sidebar) -->
        <div class="lg:col-span-1 bg-card/30 border border-border/60 rounded-lg p-6 shadow-xs flex flex-col gap-6 self-start">
          <h2 class="text-xs font-extrabold border-r-4 border-primary pr-2 leading-none text-foreground/90">تنظیمات شبیه‌ساز</h2>

          <!-- Component Selection -->
          <div class="flex flex-col gap-3">
            <Label class="text-[11px] font-semibold text-muted-foreground block">انتخاب کامپوننت:</Label>
            <div class="flex flex-col gap-1.5">
              <button 
                @click="handleComponentChange('profile')"
                :class="[
                  'w-full text-right px-3 py-2.5 text-xs font-medium rounded-lg border transition-all flex items-center justify-between cursor-pointer',
                  currentComponent === 'profile' 
                    ? 'bg-primary/5 text-primary border-primary/30 font-bold' 
                    : 'bg-background/20 border-border/40 text-foreground hover:bg-muted/40'
                ]"
              >
                <span class="flex items-center gap-2">
                  <UserIcon class="size-3.5" />
                  کارت مشخصات کاربر
                </span>
                <CheckCircle2Icon v-if="currentComponent === 'profile'" class="size-3.5 text-primary" />
              </button>
              <button 
                @click="handleComponentChange('login')"
                :class="[
                  'w-full text-right px-3 py-2.5 text-xs font-medium rounded-lg border transition-all flex items-center justify-between cursor-pointer',
                  currentComponent === 'login' 
                    ? 'bg-primary/5 text-primary border-primary/30 font-bold' 
                    : 'bg-background/20 border-border/40 text-foreground hover:bg-muted/40'
                ]"
              >
                <span class="flex items-center gap-2">
                  <KeyRoundIcon class="size-3.5" />
                  کارت ورود (تفکر)
                </span>
                <CheckCircle2Icon v-if="currentComponent === 'login'" class="size-3.5 text-primary" />
              </button>
              <button 
                @click="handleComponentChange('header')"
                :class="[
                  'w-full text-right px-3 py-2.5 text-xs font-medium rounded-lg border transition-all flex items-center justify-between cursor-pointer',
                  currentComponent === 'header' 
                    ? 'bg-primary/5 text-primary border-primary/30 font-bold' 
                    : 'bg-background/20 border-border/40 text-foreground hover:bg-muted/40'
                ]"
              >
                <span class="flex items-center gap-2">
                  <SmartphoneIcon class="size-3.5" />
                  هدر پلتفرم (تفکر)
                </span>
                <CheckCircle2Icon v-if="currentComponent === 'header'" class="size-3.5 text-primary" />
              </button>
              <button 
                @click="handleComponentChange('nav')"
                :class="[
                  'w-full text-right px-3 py-2.5 text-xs font-medium rounded-lg border transition-all flex items-center justify-between cursor-pointer',
                  currentComponent === 'nav' 
                    ? 'bg-primary/5 text-primary border-primary/30 font-bold' 
                    : 'bg-background/20 border-border/40 text-foreground hover:bg-muted/40'
                ]"
              >
                <span class="flex items-center gap-2">
                  <HomeIcon class="size-3.5" />
                  منوی نویگیشن شناور
                </span>
                <CheckCircle2Icon v-if="currentComponent === 'nav'" class="size-3.5 text-primary" />
              </button>
              <button 
                @click="handleComponentChange('topic')"
                :class="[
                  'w-full text-right px-3 py-2.5 text-xs font-medium rounded-lg border transition-all flex items-center justify-between cursor-pointer',
                  currentComponent === 'topic' 
                    ? 'bg-primary/5 text-primary border-primary/30 font-bold' 
                    : 'bg-background/20 border-border/40 text-foreground hover:bg-muted/40'
                ]"
              >
                <span class="flex items-center gap-2">
                  <TerminalIcon class="size-3.5" />
                  کارت‌های تاپیک (لیست تلگرام)
                </span>
                <CheckCircle2Icon v-if="currentComponent === 'topic'" class="size-3.5 text-primary" />
              </button>
              <button 
                @click="handleComponentChange('search')"
                :class="[
                  'w-full text-right px-3 py-2.5 text-xs font-medium rounded-lg border transition-all flex items-center justify-between cursor-pointer',
                  currentComponent === 'search' 
                    ? 'bg-primary/5 text-primary border-primary/30 font-bold' 
                    : 'bg-background/20 border-border/40 text-foreground hover:bg-muted/40'
                ]"
              >
                <span class="flex items-center gap-2">
                  <SearchIcon class="size-3.5" />
                  جستجوی مینیمال (سرچ عریض)
                </span>
                <CheckCircle2Icon v-if="currentComponent === 'search'" class="size-3.5 text-primary" />
              </button>
              <button 
                @click="handleComponentChange('profile-option')"
                :class="[
                  'w-full text-right px-3 py-2.5 text-xs font-medium rounded-lg border transition-all flex items-center justify-between cursor-pointer',
                  currentComponent === 'profile-option' 
                    ? 'bg-primary/5 text-primary border-primary/30 font-bold' 
                    : 'bg-background/20 border-border/40 text-foreground hover:bg-muted/40'
                ]"
              >
                <span class="flex items-center gap-2">
                  <UserIcon class="size-3.5" />
                  گزینه‌های پروفایل (منوی افقی)
                </span>
                <CheckCircle2Icon v-if="currentComponent === 'profile-option'" class="size-3.5 text-primary" />
              </button>
              <button 
                @click="handleComponentChange('profile-card-header')"
                :class="[
                  'w-full text-right px-3 py-2.5 text-xs font-medium rounded-lg border transition-all flex items-center justify-between cursor-pointer',
                  currentComponent === 'profile-card-header' 
                    ? 'bg-primary/5 text-primary border-primary/30 font-bold' 
                    : 'bg-background/20 border-border/40 text-foreground hover:bg-muted/40'
                ]"
              >
                <span class="flex items-center gap-2">
                  <UserIcon class="size-3.5" />
                  کارت پروفایل (جدید)
                </span>
                <CheckCircle2Icon v-if="currentComponent === 'profile-card-header'" class="size-3.5 text-primary" />
              </button>
            </div>
          </div>

          <!-- Viewport width controls -->
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
                v-for="state in availableStates"
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
                    state === 'error' ? 'نمایش خطا (Error)' : 
                    state === 'empty' ? 'کامپوننت خالی (Empty)' : 'ورود موفق (Success)' 
                  }}
                </span>
                <span class="size-2 rounded-full" 
                  :class="[
                    state === 'idle' ? 'bg-green-500' :
                    state === 'loading' ? 'bg-amber-500 animate-pulse' :
                    state === 'error' ? 'bg-destructive' : 
                    state === 'success' ? 'bg-emerald-500' : 'bg-muted-foreground'
                  ]" 
                />
              </button>
            </div>
          </div>

          <!-- Header Specific Controls -->
          <div v-if="currentComponent === 'header'" class="flex flex-col gap-3 border-t border-border/40 pt-4 mt-1">
            <Label class="text-[11px] font-semibold text-muted-foreground block">موقعیت صفحه فعال:</Label>
            <div class="flex flex-col gap-1.5">
              <button 
                @click="simulatorIsInnerPage = false"
                :class="[
                  'w-full text-right px-3 py-2 text-xs font-medium rounded-lg border transition-all flex items-center justify-between cursor-pointer',
                  !simulatorIsInnerPage 
                    ? 'bg-primary/5 text-primary border-primary/30 font-bold' 
                    : 'bg-background/20 border-border/40 text-foreground hover:bg-muted/40'
                ]"
              >
                <span>صفحه اصلی (تفکر)</span>
                <span class="text-[9px] bg-secondary px-1.5 py-0.5 rounded text-muted-foreground">Home</span>
              </button>
              <button 
                @click="simulatorIsInnerPage = true"
                :class="[
                  'w-full text-right px-3 py-2 text-xs font-medium rounded-lg border transition-all flex items-center justify-between cursor-pointer',
                  simulatorIsInnerPage 
                    ? 'bg-primary/5 text-primary border-primary/30 font-bold' 
                    : 'bg-background/20 border-border/40 text-foreground hover:bg-muted/40'
                ]"
              >
                <span>صفحه داخلی (بک فعال)</span>
                <span class="text-[9px] bg-secondary px-1.5 py-0.5 rounded text-muted-foreground">Inner</span>
              </button>
            </div>
          </div>

          <!-- Navigation Specific Controls -->
          <div v-if="currentComponent === 'nav'" class="flex flex-col gap-3 border-t border-border/40 pt-4 mt-1">
            <Label class="text-[11px] font-semibold text-muted-foreground block">شبیه‌سازی منوی فعال:</Label>
            <div class="flex flex-col gap-1.5">
              <button 
                @click="simulatorActiveTab = 'home'; logEvent('Nav: Toggled via Sidebar', { tab: 'home' })"
                :class="[
                  'w-full text-right px-3 py-2 text-xs font-medium rounded-lg border transition-all flex items-center justify-between cursor-pointer',
                  simulatorActiveTab === 'home' 
                    ? 'bg-primary/5 text-primary border-primary/30 font-bold' 
                    : 'bg-background/20 border-border/40 text-foreground hover:bg-muted/40'
                ]"
              >
                <span>صفحه خانه (خانه)</span>
                <HomeIcon class="size-3.5 text-muted-foreground" />
              </button>
              <button 
                @click="simulatorActiveTab = 'profile'; logEvent('Nav: Toggled via Sidebar', { tab: 'profile' })"
                :class="[
                  'w-full text-right px-3 py-2 text-xs font-medium rounded-lg border transition-all flex items-center justify-between cursor-pointer',
                  simulatorActiveTab === 'profile' 
                    ? 'bg-primary/5 text-primary border-primary/30 font-bold' 
                    : 'bg-background/20 border-border/40 text-foreground hover:bg-muted/40'
                ]"
              >
                <span>صفحه پروفایل (کاربر)</span>
                <UserIcon class="size-3.5 text-muted-foreground" />
              </button>
            </div>
          </div>

          <!-- Topic Specific Controls -->
          <div v-if="currentComponent === 'topic'" class="flex flex-col gap-3 border-t border-border/40 pt-4 mt-1">
            <Label class="text-[11px] font-semibold text-muted-foreground block">شبیه‌سازی پیام‌ها:</Label>
            <div class="flex flex-col gap-1.5">
              <Button 
                size="sm" 
                @click="addSimulatedMessage"
                class="w-full text-xs font-semibold cursor-pointer"
              >
                شبیه‌سازی دریافت پیام جدید
              </Button>
              <Button 
                size="sm" 
                variant="outline"
                @click="mockTopics.forEach(t => t.unreadCount = 0); logEvent('Topic: All Marked as Read', {})"
                class="w-full text-xs font-semibold cursor-pointer"
              >
                خوانده شدن همه کارت‌ها
              </Button>
            </div>
          </div>

          <!-- Search Specific Controls -->
          <div v-if="currentComponent === 'search'" class="flex flex-col gap-3 border-t border-border/40 pt-4 mt-1">
            <Label class="text-[11px] font-semibold text-muted-foreground block">شبیه‌سازی وضعیت جستجو:</Label>
            <div class="flex flex-col gap-1.5">
              <button 
                @click="simulatorSearchExpanded = !simulatorSearchExpanded"
                class="w-full text-right px-3 py-2 text-xs font-medium rounded-lg border transition-all flex items-center justify-between bg-background/20 border-border/40 text-foreground hover:bg-muted/40 cursor-pointer"
              >
                <span>وضعیت باز/بسته (Expand)</span>
                <span class="text-[9px] bg-secondary px-1.5 py-0.5 rounded text-muted-foreground">
                  {{ simulatorSearchExpanded ? 'باز شده' : 'بسته شده' }}
                </span>
              </button>
              <Button 
                size="sm" 
                variant="outline"
                @click="simulatorSearchQuery = 'توسعه'; simulatorSearchExpanded = true; logEvent('Search: Force Query = توسعه', {})"
                class="w-full text-xs font-semibold cursor-pointer"
              >
                جستجوی کلمه «توسعه»
              </Button>
              <Button 
                size="sm" 
                variant="outline"
                @click="simulatorSearchQuery = ''; logEvent('Search: Query Cleared via Sandbox', {})"
                class="w-full text-xs font-semibold cursor-pointer"
              >
                پاک کردن متن جستجو
              </Button>
            </div>
          </div>

          <!-- Profile Option Specific Controls -->
          <div v-if="currentComponent === 'profile-option'" class="flex flex-col gap-3 border-t border-border/40 pt-4 mt-1">
            <Label class="text-[11px] font-semibold text-muted-foreground block">شبیه‌سازی نشان‌گر گزینه‌ها:</Label>
            <div class="flex flex-col gap-1.5">
              <button 
                @click="simulatorSettingsBadge = simulatorSettingsBadge ? '' : 'بروزرسانی'"
                class="w-full text-right px-3 py-2 text-xs font-medium rounded-lg border transition-all flex items-center justify-between bg-background/20 border-border/40 text-foreground hover:bg-muted/40 cursor-pointer"
              >
                <span>نشان‌گر تنظیمات (Settings)</span>
                <span class="text-[9px] bg-secondary px-1.5 py-0.5 rounded text-muted-foreground">
                  {{ simulatorSettingsBadge || 'بدون نشان‌گر' }}
                </span>
              </button>
              <button 
                @click="simulatorConnectionsBadge = simulatorConnectionsBadge ? '' : '۳ فعال'"
                class="w-full text-right px-3 py-2 text-xs font-medium rounded-lg border transition-all flex items-center justify-between bg-background/20 border-border/40 text-foreground hover:bg-muted/40 cursor-pointer"
              >
                <span>نشان‌گر اتصال‌ها (Connections)</span>
                <span class="text-[9px] bg-secondary px-1.5 py-0.5 rounded text-muted-foreground">
                  {{ simulatorConnectionsBadge || 'بدون نشان‌گر' }}
                </span>
              </button>
              <button 
                @click="simulatorAiBadge = simulatorAiBadge ? '' : 'جدید'"
                class="w-full text-right px-3 py-2 text-xs font-medium rounded-lg border transition-all flex items-center justify-between bg-background/20 border-border/40 text-foreground hover:bg-muted/40 cursor-pointer"
              >
                <span>نشان‌گر هوش مصنوعی (AI)</span>
                <span class="text-[9px] bg-secondary px-1.5 py-0.5 rounded text-muted-foreground">
                  {{ simulatorAiBadge || 'بدون نشان‌گر' }}
                </span>
              </button>
            </div>
          </div>

          <!-- Profile Card Specific Controls -->
          <div v-if="currentComponent === 'profile-card-header'" class="flex flex-col gap-3 border-t border-border/40 pt-4 mt-1">
            <Label class="text-[11px] font-semibold text-muted-foreground block">تنظیمات کارت پروفایل:</Label>
            <div class="flex flex-col gap-3">
              <div class="flex flex-col gap-1">
                <Label class="text-[9.5px] text-muted-foreground">نام و نام خانوادگی:</Label>
                <Input v-model="simulatorProfileCardName" class="h-8 text-xs font-semibold" placeholder="نام کاربر" />
              </div>
              <div class="flex flex-col gap-1">
                <Label class="text-[9.5px] text-muted-foreground">وضعیت فعالیت:</Label>
                <div class="grid grid-cols-2 gap-1 bg-muted/65 p-1 rounded-lg border border-border/40">
                  <button 
                    v-for="statusType in (['success', 'warning', 'info', 'muted'] as const)"
                    :key="statusType"
                    @click="simulatorProfileCardStatus = statusType"
                    :class="[
                      'py-1 rounded text-[9px] font-bold transition-all cursor-pointer outline-none border border-transparent',
                      simulatorProfileCardStatus === statusType 
                        ? 'bg-background text-foreground shadow-xs font-black' 
                        : 'text-muted-foreground hover:text-foreground'
                    ]"
                  >
                    {{ 
                      statusType === 'success' ? 'فعال' :
                      statusType === 'warning' ? 'مشغول' :
                      statusType === 'info' ? 'غایب' : 'آفلاین'
                    }}
                  </button>
                </div>
              </div>
            </div>
          </div>

          <!-- Dev Note -->
          <div class="p-3 bg-muted/40 border border-border/40 rounded-lg text-[10px] leading-relaxed text-muted-foreground flex gap-2">
            <InfoIcon class="size-3.5 text-primary shrink-0 mt-0.5" />
            <span>تغییر وضعیت یا تغییر ابعاد، رفتار کامپوننت را در یک محدوده عایق و ایزوله شبیه‌ساز شبیه‌سازی می‌کند.</span>
          </div>
        </div>

        <!-- Main Testing Sandbox -->
        <div class="lg:col-span-3 flex flex-col gap-6">
          
          <!-- Live Preview Sandbox -->
          <div class="bg-card/30 border border-border/60 rounded-lg p-6 shadow-xs flex flex-col items-center">
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
              :class="[
                'w-full border border-dashed border-border bg-background/50 rounded-lg transition-all duration-300 ease-in-out shadow-xs overflow-hidden flex justify-center',
                currentComponent === 'header' ? 'p-0' : 'p-4'
              ]"
            >
              <div :class="['w-full flex justify-center transition-all duration-300', currentComponent === 'header' ? 'max-w-none' : 'max-w-md']">
                <UserProfileCard 
                  v-if="currentComponent === 'profile'"
                  :state="activeState" 
                  :user="mockUser" 
                  @save="handleSave"
                  @status-toggle="handleStatusToggle"
                />
                <LoginCard
                  v-else-if="currentComponent === 'login'"
                  :state="activeState === 'empty' ? 'idle' : (activeState as any)"
                  @submitPhone="handleLoginPhoneSubmit"
                  @submitOtp="handleLoginOtpSubmit"
                  @stateChange="handleLoginStateChange"
                />

                <!-- PlatformHeader Preview (Directly in sandbox, full-width, dynamic back button) -->
                <div 
                  v-else-if="currentComponent === 'header'"
                  class="w-full flex flex-col"
                  dir="rtl"
                >
                  <!-- Header Component wrapper (No horizontal padding, flush to screen left/right borders) -->
                  <div class="px-4 py-3 w-full border-b border-border/40 bg-card/10">
                    <PlatformHeader
                      :isInnerPage="simulatorIsInnerPage"
                      :innerTitle="simulatorInnerTitle"
                      :loading="activeState === 'loading'"
                      :error="activeState === 'error'"
                      @back="handleHeaderBack"
                      @actionClick="handleHeaderAction"
                    />
                  </div>
                  
                  <!-- Interactive Content Panel (Padded) -->
                  <div class="w-full p-4 flex flex-col gap-4">
                    <div class="w-full bg-card/40 border border-border/40 rounded-xl p-5 flex flex-col gap-4 text-right">
                      <div class="flex items-center justify-between border-b border-border/40 pb-3">
                        <h3 class="text-xs font-extrabold text-foreground">
                          شبیه‌ساز محتوای پلتفرم (تفکر)
                        </h3>
                        <span class="text-[9px] bg-primary/10 text-primary px-2 py-0.5 rounded font-mono font-bold select-none">
                          {{ simulatorIsInnerPage ? 'Inner Page' : 'Home' }}
                        </span>
                      </div>

                      <div v-if="!simulatorIsInnerPage" class="flex flex-col gap-2.5">
                        <p class="text-[11px] text-muted-foreground leading-relaxed">
                          این شبیه‌سازی وضعیت صفحه اصلی برنامه است. با کلیک بر روی هر یک از گفتگوهای آزمایشی زیر، وارد صفحه چت شده و انیمیشن جدا شدن و فاصله‌گیری ۴ پیکسلی دکمه بازگشت از بدنه اصلی هدر را مشاهده خواهید کرد:
                        </p>
                        
                        <div class="grid grid-cols-1 md:grid-cols-3 gap-3 mt-1">
                          <div 
                            v-for="chat in mockChats" 
                            :key="chat.id"
                            @click="enterChat(chat)"
                            class="flex items-center gap-3 p-3 rounded-lg bg-background/60 hover:bg-muted/60 border border-border/45 cursor-pointer transition-all active:scale-[0.98]"
                          >
                            <div class="size-8 rounded-full bg-primary/10 border border-primary/20 flex items-center justify-center text-primary text-[10px] font-extrabold shrink-0">
                              {{ chat.avatar }}
                            </div>
                            <div class="flex-1 flex flex-col text-right overflow-hidden">
                              <span class="text-[10px] font-bold text-foreground truncate">{{ chat.name }}</span>
                              <span class="text-[8.5px] text-muted-foreground mt-0.5 truncate">{{ chat.message }}</span>
                            </div>
                          </div>
                        </div>
                      </div>

                      <div v-else class="flex flex-col gap-3">
                        <p class="text-[11px] text-muted-foreground leading-relaxed">
                          شما هم‌اکنون در صفحه گفتگو با <code class="bg-muted px-1.5 py-0.5 rounded font-bold text-primary">{{ simulatorInnerTitle }}</code> هستید. هدر به صورت خودکار تغییر سایز داده و دکمه بازگشت در سمت راست جدا شده است. برای بازگشت به صفحه اصلی، روی دکمه بازگشت (فلش راست) در هدر کلیک کنید یا دکمه شبیه‌ساز زیر را بزنید:
                        </p>
                        
                        <div class="self-start mt-1">
                          <Button 
                            size="sm" 
                            variant="outline"
                            @click="handleHeaderBack"
                            class="text-xs font-semibold cursor-pointer"
                          >
                            شبیه‌سازی کلیک روی دکمه بازگشت
                          </Button>
                        </div>
                      </div>
                    </div>
                  </div>
                </div>

                <!-- PlatformNavigation Preview -->
                <div 
                  v-else-if="currentComponent === 'nav'"
                  class="w-full h-[250px] bg-muted/10 border border-border/40 rounded-xl relative overflow-hidden flex flex-col font-sans"
                  dir="rtl"
                >
                  <div class="flex-1 p-6 flex flex-col items-center justify-center text-center">
                    <transition name="fade-slide" mode="out-in">
                      <div v-if="simulatorActiveTab === 'home'" class="flex flex-col items-center gap-2">
                        <div class="size-10 rounded-full bg-primary/10 flex items-center justify-center text-primary">
                          <HomeIcon class="size-5" />
                        </div>
                        <h4 class="text-xs font-extrabold text-foreground mt-1">پیشخوان پلتفرم تفکر (صفحه خانه)</h4>
                        <p class="text-[10px] text-muted-foreground max-w-xs leading-relaxed">
                          به بخش کاربری تفکر خوش آمدید. نویگیشن بار شناور در پایین صفحه قرار گرفته و به شما اجازه جابجایی می‌دهد.
                        </p>
                      </div>
                      <div v-else class="flex flex-col items-center gap-2">
                        <div class="size-10 rounded-full bg-primary/10 flex items-center justify-center text-primary">
                          <UserIcon class="size-5" />
                        </div>
                        <h4 class="text-xs font-extrabold text-foreground mt-1">تنظیمات حساب کاربری (صفحه پروفایل)</h4>
                        <p class="text-[10px] text-muted-foreground max-w-xs leading-relaxed">
                          در این بخش اطلاعات کاربری و سطح دسترسی شما نمایش داده می‌شود. وضعیت فعال منو با انیمیشن در پایین مشخص است.
                        </p>
                      </div>
                    </transition>
                  </div>

                  <div class="absolute bottom-4 left-1/2 -translate-x-1/2 z-10 w-auto">
                    <PlatformNavigation 
                      v-model="simulatorActiveTab"
                      @change="handleNavChange"
                    />
                  </div>
                </div>

                <!-- TopicCard Vertical List Preview (Telegram Chat List Style) -->
                <div 
                  v-else-if="currentComponent === 'topic'"
                  class="w-full flex flex-col gap-2 bg-card/10 border border-border/40 rounded-xl p-3.5"
                  dir="rtl"
                >
                  <div class="flex items-center justify-between border-b border-border/40 pb-3 mb-1 px-1">
                    <h3 class="text-xs font-extrabold text-foreground">گفتگوهای فعال</h3>
                    <span class="text-[9px] bg-secondary/80 px-2 py-0.5 rounded border border-border/50 text-muted-foreground font-mono">Telegram Style</span>
                  </div>

                  <!-- Vertical list of borderless cards (No borders/lines, separated purely by whitespace and hover effect) -->
                  <div class="flex flex-col gap-1.5 w-full">
                    <TopicCard
                      v-for="topic in mockTopics"
                      :key="topic.id"
                      :title="topic.title"
                      :description="topic.description"
                      :unreadCount="topic.unreadCount"
                      :time="topic.time"
                      :labels="topic.labels"
                      :pinned="topic.pinned"
                      @click="handleTopicClick(topic.id)"
                    />
                  </div>
                </div>

                <!-- MinimalSearch Preview Section -->
                <div 
                  v-else-if="currentComponent === 'search'"
                  class="w-full flex flex-col gap-6"
                  dir="rtl"
                >
                  <!-- 1. Standalone Showcase -->
                  <div class="w-full bg-card/45 border border-border/40 rounded-xl p-5 flex flex-col gap-3.5 text-right">
                    <div class="border-b border-border/40 pb-2">
                      <h3 class="text-xs font-extrabold text-foreground">۱. نمایش مستقل کامپوننت جستجو (تست ابعاد)</h3>
                      <p class="text-[9.5px] text-muted-foreground mt-1">با کلیک روی آیکون سرچ، کامپوننت به آرامی و با انیمیشن در کل عرض باز می‌شود.</p>
                    </div>
                    
                    <div class="flex justify-center py-2">
                      <div class="w-full max-w-sm flex justify-start">
                        <MinimalSearch 
                          v-model="simulatorSearchQuery"
                          v-model:expanded="simulatorSearchExpanded"
                          @search="(q) => handleSearchEvent('SubmitQuery', { query: q })"
                          @clear="handleSearchEvent('Clear', {})"
                          @expand="handleSearchEvent('Expand', {})"
                          @collapse="handleSearchEvent('Collapse', {})"
                        />
                      </div>
                    </div>
                  </div>

                  <!-- 2. Integrated Header Mockup & Live Topic Filtering -->
                  <div class="w-full bg-card/45 border border-border/40 rounded-xl p-5 flex flex-col gap-4 text-right">
                    <div class="border-b border-border/40 pb-2 flex items-center justify-between">
                      <div>
                        <h3 class="text-xs font-extrabold text-foreground">۲. شبیه‌سازی هدر تلگرام با جستجوی اکسپندشونده</h3>
                        <p class="text-[9.5px] text-muted-foreground mt-1">در حالت باز، عنوان هدر پنهان شده و سرچ کل عرض را پر می‌کند.</p>
                      </div>
                      <span class="text-[9px] bg-emerald-500/10 text-emerald-500 px-2 py-0.5 rounded font-bold">زنده / تعاملی</span>
                    </div>

                    <!-- Mock Header Container -->
                    <div class="w-full h-16 bg-background/80 border border-border/60 rounded-xl p-3.5 flex items-center justify-between relative overflow-hidden transition-all duration-300">
                      <!-- Right Side: Header Title (fades/slides out when expanded) -->
                      <transition name="header-title-fade">
                        <div v-if="!simulatorSearchExpanded" class="flex flex-col text-right shrink-0">
                          <span class="text-xs font-bold text-foreground">پیام‌رسان تفکر</span>
                          <span class="text-[8.5px] text-muted-foreground mt-0.5">شبیه‌ساز هدر چت</span>
                        </div>
                      </transition>

                      <!-- Left Side: Expandable Search Bar container -->
                      <div 
                        class="transition-all duration-300 ease-in-out flex justify-end animate-search"
                        :class="simulatorSearchExpanded ? 'w-full' : 'w-11'"
                      >
                        <MinimalSearch 
                          v-model="simulatorSearchQuery"
                          v-model:expanded="simulatorSearchExpanded"
                          @search="(q) => handleSearchEvent('SubmitQuery', { query: q })"
                          @clear="handleSearchEvent('Clear', {})"
                          @expand="handleSearchEvent('Expand', {})"
                          @collapse="handleSearchEvent('Collapse', {})"
                        />
                      </div>
                    </div>

                    <!-- Live search filtering list below -->
                    <div class="flex flex-col gap-2 mt-1">
                      <div class="flex items-center justify-between px-1">
                        <span class="text-[10px] font-bold text-foreground">نتایج فیلتر شده بر اساس جستجو ({{ filteredTopics.length }} مورد)</span>
                        <span v-if="simulatorSearchQuery" class="text-[9px] bg-primary/10 text-primary px-1.5 py-0.5 rounded">فیلتر فعال: {{ simulatorSearchQuery }}</span>
                      </div>

                      <div class="flex flex-col gap-1.5 bg-background/20 border border-border/40 rounded-xl p-2 min-h-24">
                        <div v-if="filteredTopics.length === 0" class="text-center py-8 text-[10px] text-muted-foreground">
                          هیچ گفتگویی با کلمه «{{ simulatorSearchQuery }}» یافت نشد.
                        </div>
                        <transition-group name="list-fade" v-else>
                          <TopicCard
                            v-for="topic in filteredTopics"
                            :key="topic.id"
                            :title="topic.title"
                            :description="topic.description"
                            :unreadCount="topic.unreadCount"
                            :time="topic.time"
                            :labels="topic.labels"
                            :pinned="topic.pinned"
                            @click="handleTopicClick(topic.id)"
                          />
                        </transition-group>
                      </div>
                    </div>
                  </div>
                </div>

                <!-- Profile Option Preview -->
                <div 
                  v-else-if="currentComponent === 'profile-option'"
                  class="w-full flex flex-col gap-4 bg-card/10 border border-border/40 rounded-xl p-4"
                  dir="rtl"
                >
                  <div class="flex items-center justify-between border-b border-border/40 pb-3 mb-1 px-1">
                    <h3 class="text-xs font-extrabold text-foreground">منوی کاربری (گزینه‌ها)</h3>
                    <span class="text-[9px] bg-secondary/80 px-2 py-0.5 rounded border border-border/50 text-muted-foreground font-mono">Profile Menu Items</span>
                  </div>

                  <!-- Vertical menu list (menu stack of ProfileOptionCards) -->
                  <div class="flex flex-col gap-2.5 w-full">
                    <ProfileOptionCard
                      :title="'تنظیمات سیستم'"
                      :description="'تنظیمات زبان، ظاهر، حریم خصوصی و اعلان‌ها'"
                      :icon="SettingsIcon"
                      :badgeText="simulatorSettingsBadge || undefined"
                      :badgeVariant="'destructive'"
                      @click="handleProfileOptionClick('Settings')"
                    />
                    
                    <ProfileOptionCard
                      :title="'مدیریت اتصال‌ها'"
                      :description="'مدیریت اتصال به ربات‌ها، کانال‌ها و پلتفرم‌های خارجی'"
                      :icon="LinkIcon"
                      :badgeText="simulatorConnectionsBadge || undefined"
                      :badgeVariant="'default'"
                      @click="handleProfileOptionClick('Connections')"
                    />

                    <ProfileOptionCard
                      :title="'دستیار هوش مصنوعی'"
                      :description="'تنظیمات مدل هوشمند تفکر، خلاقیت و کلیدهای API فعال'"
                      :icon="SparklesIcon"
                      :badgeText="simulatorAiBadge || undefined"
                      :badgeVariant="'default'"
                      @click="handleProfileOptionClick('AI')"
                    />
                  </div>
                </div>

                <!-- ProfileCard Preview -->
                <div 
                  v-else-if="currentComponent === 'profile-card-header'"
                  class="w-full flex flex-col gap-4 bg-card/10 border border-border/40 rounded-xl p-4"
                  dir="rtl"
                >
                  <div class="flex items-center justify-between border-b border-border/40 pb-3 mb-1 px-1">
                    <h3 class="text-xs font-extrabold text-foreground">کارت هدر پروفایل</h3>
                    <span class="text-[9px] bg-secondary/80 px-2 py-0.5 rounded border border-border/50 text-muted-foreground font-mono">Profile Header Card</span>
                  </div>

                  <div class="w-full max-w-xs mx-auto py-6">
                    <ProfileCard
                      :name="simulatorProfileCardName"
                      :phone="simulatorProfileCardPhone"
                      :statusVariant="simulatorProfileCardStatus"
                      :statusText="
                        simulatorProfileCardStatus === 'success' ? 'فعال' :
                        simulatorProfileCardStatus === 'warning' ? 'مشغول' :
                        simulatorProfileCardStatus === 'info' ? 'غایب' : 'آفلاین'
                      "
                      @edit="handleProfileCardEdit"
                    />
                  </div>
                </div>

              </div>
            </div>
          </div>

          <!-- Interactive Event Log Console -->
          <div class="bg-card/30 border border-border/60 rounded-lg p-6 shadow-xs flex flex-col gap-3">
            <div class="flex items-center justify-between">
              <h3 class="text-xs font-extrabold text-foreground flex items-center gap-1.5">
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

<style scoped>
/* Fade-slide transition for mockup screen navigation */
.fade-slide-enter-active,
.fade-slide-leave-active {
  transition: all 0.35s cubic-bezier(0.4, 0, 0.2, 1);
}
.fade-slide-enter-from {
  opacity: 0;
  transform: translateX(-15px);
}
.fade-slide-leave-to {
  opacity: 0;
  transform: translateX(15px);
}

/* Slide/fade transition for header title during search expansion */
.header-title-fade-enter-active,
.header-title-fade-leave-active {
  transition: all 0.25s cubic-bezier(0.4, 0, 0.2, 1);
}
.header-title-fade-enter-from,
.header-title-fade-leave-to {
  opacity: 0;
  transform: translateX(15px);
}

/* Fade transition for reactive list filtering */
.list-fade-enter-active,
.list-fade-leave-active {
  transition: all 0.2s ease;
}
.list-fade-enter-from,
.list-fade-leave-to {
  opacity: 0;
  transform: translateY(5px);
}
</style>
