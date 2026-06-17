<script setup lang="ts">
import { ref } from 'vue'
import { Button } from '@/components/ui/button'
import { Input } from '@/components/ui/input'
import { Label } from '@/components/ui/label'
import { Checkbox } from '@/components/ui/checkbox'
import { Calendar } from '@/components/ui/calendar'
import { Popover, PopoverContent, PopoverTrigger } from '@/components/ui/popover'
import { InputOTP, InputOTPGroup, InputOTPSlot } from '@/components/ui/input-otp'
import { 
  SparklesIcon, 
  Trash2Icon, 
  PlusIcon, 
  SettingsIcon, 
  CalendarIcon, 
  LockIcon, 
  CheckCircle2Icon, 
  InfoIcon,
  LayoutGridIcon,
  RulerIcon,
  TypeIcon,
  // 20 Popular Lucide Icons
  HomeIcon,
  UserIcon,
  MailIcon,
  SearchIcon,
  CheckIcon,
  XIcon,
  AlertTriangleIcon,
  HelpCircleIcon,
  ArrowLeftIcon,
  ArrowRightIcon,
  ChevronDownIcon,
  ChevronUpIcon,
  MenuIcon,
  BellIcon,
  ActivityIcon,
  HeartIcon,
  StarIcon,
  ShieldIcon,
  Share2Icon,
  EyeIcon
} from '@lucide/vue'
import { getLocalTimeZone, today } from '@internationalized/date'

// Tabs management
const activeTab = ref<'components' | 'tokens'>('components')

// State for showcase interactive items
const inputVal = ref('')
const checkboxVal = ref(false)
const otpVal = ref('')
const dateVal = ref<any>(today(getLocalTimeZone()))
const isPending = ref(false)

const triggerSimulatedPending = () => {
  isPending.value = true
  setTimeout(() => {
    isPending.value = false
  }, 2000)
}

// Spacing System data
const spacings = [
  { value: '1', rem: '0.25rem', px: '4px' },
  { value: '2', rem: '0.5rem', px: '8px' },
  { value: '3', rem: '0.75rem', px: '12px' },
  { value: '4', rem: '1rem', px: '16px' },
  { value: '6', rem: '1.5rem', px: '24px' },
  { value: '8', rem: '2rem', px: '32px' },
  { value: '12', rem: '3rem', px: '48px' },
  { value: '16', rem: '4rem', px: '64px' },
]

// Theme colors list
const colors = [
  { name: 'Background', class: 'bg-background text-foreground border border-border/80', desc: 'پس‌زمینه اصلی پلتفرم', variable: '--background' },
  { name: 'Foreground', class: 'bg-foreground text-background', desc: 'متون و عناصر متضاد اصلی', variable: '--foreground' },
  { name: 'Primary', class: 'bg-primary text-primary-foreground', desc: 'رنگ اصلی پلتفرم و کنش‌ها', variable: '--primary' },
  { name: 'Secondary', class: 'bg-secondary text-secondary-foreground border border-border/40', desc: 'رنگ عناصر ثانویه و مکمل', variable: '--secondary' },
  { name: 'Muted', class: 'bg-muted text-muted-foreground', desc: 'متون کم‌اهمیت و فرعی', variable: '--muted' },
  { name: 'Accent', class: 'bg-accent text-accent-foreground', desc: 'رنگ برجسته‌سازی و حالت فعال', variable: '--accent' },
  { name: 'Destructive', class: 'bg-destructive text-destructive-foreground', desc: 'رنگ پیام‌های هشدار و حذف', variable: '--destructive' },
  { name: 'Card', class: 'bg-card text-card-foreground border border-border/80', desc: 'باکس‌ها و کارت‌ها', variable: '--card' },
  { name: 'Popover', class: 'bg-popover text-popover-foreground border border-border/80', desc: 'پاپ‌آپ‌های شناور', variable: '--popover' },
]

const systemBorders = [
  { name: 'Border', class: 'bg-border', variable: '--border', desc: 'مرزبندی‌ها' },
  { name: 'Input', class: 'bg-input', variable: '--input', desc: 'فیلدهای ورودی' },
  { name: 'Ring', class: 'bg-ring', variable: '--ring', desc: 'حلقه فوکوس' },
]

const colorShades = [
  { name: 'Primary (اصلی)', baseClass: 'primary', desc: 'رنگ عملیات‌ها و دکمه‌های اصلی' },
  { name: 'Secondary (ثانویه)', baseClass: 'secondary', desc: 'رنگ‌های مکمل و فرعی با کنتراست پایین' },
  { name: 'Accent (برجسته)', baseClass: 'accent', desc: 'تینت فوکوس، هاور و عناصر انتخابی' },
  { name: 'Muted (خاموش)', baseClass: 'muted', desc: 'متون کم‌اهمیت و فیلدهای غیرفعال' },
  { name: 'Destructive (مخرب)', baseClass: 'destructive', desc: 'پیام‌های خطا، حذف و وضعیت‌های منفی' },
]

// Font Sizes Scale data
const fontSizes = [
  { name: 'text-xs', size: '0.75rem', px: '12px', class: 'text-xs' },
  { name: 'text-sm', size: '0.875rem', px: '14px', class: 'text-sm' },
  { name: 'text-base', size: '1rem', px: '16px', class: 'text-base' },
  { name: 'text-lg', size: '1.125rem', px: '18px', class: 'text-lg' },
  { name: 'text-xl', size: '1.25rem', px: '20px', class: 'text-xl' },
  { name: 'text-2xl', size: '1.5rem', px: '24px', class: 'text-2xl' },
  { name: 'text-3xl', size: '1.875rem', px: '30px', class: 'text-3xl' },
  { name: 'text-4xl', size: '2.25rem', px: '36px', class: 'text-4xl' },
  { name: 'text-5xl', size: '3rem', px: '48px', class: 'text-5xl' },
]

// 20 Popular Lucide Icons
const baseIcons = [
  { name: 'Home', icon: HomeIcon },
  { name: 'User', icon: UserIcon },
  { name: 'Settings', icon: SettingsIcon },
  { name: 'Mail', icon: MailIcon },
  { name: 'Lock', icon: LockIcon },
  { name: 'Calendar', icon: CalendarIcon },
  { name: 'Search', icon: SearchIcon },
  { name: 'Plus', icon: PlusIcon },
  { name: 'Trash2', icon: Trash2Icon },
  { name: 'Check', icon: CheckIcon },
  { name: 'X', icon: XIcon },
  { name: 'AlertTriangle', icon: AlertTriangleIcon },
  { name: 'Info', icon: InfoIcon },
  { name: 'HelpCircle', icon: HelpCircleIcon },
  { name: 'ArrowLeft', icon: ArrowLeftIcon },
  { name: 'ArrowRight', icon: ArrowRightIcon },
  { name: 'ChevronDown', icon: ChevronDownIcon },
  { name: 'ChevronUp', icon: ChevronUpIcon },
  { name: 'Menu', icon: MenuIcon },
  { name: 'Bell', icon: BellIcon },
  { name: 'Activity', icon: ActivityIcon },
  { name: 'Heart', icon: HeartIcon },
  { name: 'Star', icon: StarIcon },
  { name: 'Shield', icon: ShieldIcon },
  { name: 'Share2', icon: Share2Icon },
  { name: 'Eye', icon: EyeIcon },
]
</script>

<template>
  <div class="w-full flex justify-center" dir="rtl">
    <div class="w-full max-w-6xl py-10 px-6 flex flex-col gap-12">
      
      <!-- Header (Centered alignment) -->
      <div class="border-b border-border/60 pb-6 flex flex-col md:flex-row md:items-end md:justify-between gap-6 w-full text-right">
        <div class="flex flex-col gap-2">
          <div class="flex items-center gap-2 text-primary justify-start">
            <SparklesIcon class="size-4" />
            <span class="text-[10px] font-bold uppercase tracking-wider text-primary/80">محیط ۲ - نمایشگاه دیزاین سیستم</span>
          </div>
          <h1 class="text-xl font-bold tracking-tight text-foreground">آلبوم ویژگی‌ها و متغیرها (Showcase)</h1>
          <p class="text-muted-foreground text-xs mt-0.5">تست تم رنگی، تایپوگرافی فونت متغیر، فواصل و کامپوننت‌های پایه.</p>
        </div>

        <!-- Tab Selectors -->
        <div class="flex bg-muted/60 p-1 rounded-lg border border-border/40 shrink-0 self-start md:self-auto">
          <button 
            @click="activeTab = 'components'"
            :class="[
              'px-4 py-1.5 rounded-md text-xs font-medium transition-all outline-none cursor-pointer',
              activeTab === 'components' ? 'bg-background text-foreground shadow-xs font-bold' : 'text-muted-foreground hover:text-foreground'
            ]"
          >
            کامپوننت‌های پایه (Primitives)
          </button>
          <button 
            @click="activeTab = 'tokens'"
            :class="[
              'px-4 py-1.5 rounded-md text-xs font-medium transition-all outline-none cursor-pointer',
              activeTab === 'tokens' ? 'bg-background text-foreground shadow-xs font-bold' : 'text-muted-foreground hover:text-foreground'
            ]"
          >
            رنگ‌ها، تایپوگرافی و فواصل (Tokens)
          </button>
        </div>
      </div>

      <!-- TAB 1: COMPONENTS (PRIMITIVES) -->
      <div v-if="activeTab === 'components'" class="grid grid-cols-1 lg:grid-cols-2 gap-12 w-full">
        
        <!-- Buttons Panel -->
        <div class="bg-card/30 border border-border/60 rounded-xl p-6 shadow-xs flex flex-col gap-6">
          <div class="flex flex-col gap-1">
            <h2 class="text-sm font-bold text-foreground">دکمه‌ها (Button Variants)</h2>
            <p class="text-[11px] text-muted-foreground">تست انواع واریانت‌ها و ابعاد مختلف دکمه با انیمیشن‌های ترنزیشن پیش‌فرض.</p>
          </div>

          <div class="flex flex-col gap-6">
            <div class="flex flex-col gap-2.5">
              <h3 class="text-[10px] font-bold uppercase tracking-wider text-muted-foreground/80">انواع استایل‌ها (Variants):</h3>
              <div class="flex flex-wrap gap-3">
                <Button>پیش‌فرض</Button>
                <Button variant="secondary">ثانویه</Button>
                <Button variant="outline">دورخط‌دار</Button>
                <Button variant="destructive" class="flex items-center gap-1.5">
                  <Trash2Icon class="size-3.5" data-icon="inline-start" />
                  مخرب
                </Button>
                <Button variant="ghost">شیشه‌ای</Button>
                <Button variant="link">لینک</Button>
              </div>
            </div>

            <div class="flex flex-col gap-2.5">
              <h3 class="text-[10px] font-bold uppercase tracking-wider text-muted-foreground/80">سایزها و آیکون‌ها (Sizes & Icons):</h3>
              <div class="flex flex-wrap items-center gap-3">
                <Button size="lg" class="flex items-center gap-1.5">
                  بزرگ (LG)
                  <SettingsIcon class="size-4" data-icon="inline-end" />
                </Button>
                <Button size="default">معمولی</Button>
                <Button size="sm">کوچک (SM)</Button>
                <Button size="xs">خیلی کوچک</Button>
                <Button size="icon" variant="outline">
                  <PlusIcon class="size-4" />
                </Button>
              </div>
            </div>

            <div class="flex flex-col gap-2.5">
              <h3 class="text-[10px] font-bold uppercase tracking-wider text-muted-foreground/80">وضعیت‌های رفتاری (States):</h3>
              <div class="flex flex-wrap gap-3">
                <Button disabled>غیرفعال (Disabled)</Button>
                <Button variant="outline" disabled>غیرفعال دورخط‌دار</Button>
                <Button :disabled="isPending" @click="triggerSimulatedPending" class="relative">
                  <span v-if="isPending" class="flex items-center gap-1.5">
                    <span class="size-3 border-2 border-primary-foreground/30 border-t-primary-foreground rounded-full animate-spin" />
                    در حال ثبت...
                  </span>
                  <span v-else>کلیک برای تست بارگذاری</span>
                </Button>
              </div>
            </div>
          </div>
        </div>

        <!-- Inputs Panel -->
        <div class="bg-card/30 border border-border/60 rounded-xl p-6 shadow-xs flex flex-col gap-6">
          <div class="flex flex-col gap-1">
            <h2 class="text-sm font-bold text-foreground">فیلدهای ورودی (Inputs)</h2>
            <p class="text-[11px] text-muted-foreground">نمونه ورودی‌های متنی، پسورد و وضعیت‌های ولیدیشن.</p>
          </div>

          <div class="flex flex-col gap-4">
            <div class="flex flex-col gap-2">
              <Label for="inp-def">ورودی متنی پیش‌فرض</Label>
              <Input id="inp-def" v-model="inputVal" placeholder="متنی بنویسید..." />
              <span class="text-[10px] text-muted-foreground mt-0.5 block">متن وارد شده: <code class="bg-muted px-1 rounded">{{ inputVal || 'خالی' }}</code></span>
            </div>

            <div class="flex flex-col gap-2">
              <Label for="inp-pass" class="flex items-center gap-1.5">
                <LockIcon class="size-3.5 text-muted-foreground" />
                رمز عبور (Password)
              </Label>
              <Input id="inp-pass" type="password" placeholder="••••••••" />
            </div>

            <div class="grid grid-cols-1 sm:grid-cols-2 gap-6">
              <div class="flex flex-col gap-2">
                <Label for="inp-dis" class="text-muted-foreground">غیرفعال (Disabled)</Label>
                <Input id="inp-dis" disabled placeholder="امکان ویرایش وجود ندارد" />
              </div>
              <div class="flex flex-col gap-2">
                <Label for="inp-err" class="text-destructive">فیلد دارای خطا (Error State)</Label>
                <Input id="inp-err" aria-invalid="true" placeholder="ایمیل نامعتبر است..." class="border-destructive focus-visible:ring-destructive/30" />
                <p class="text-[10px] text-destructive flex items-center gap-1.5">
                  <InfoIcon class="size-3.5" />
                  آدرس ایمیل وارد شده در قالب صحیح نیست.
                </p>
              </div>
            </div>
          </div>
        </div>

        <!-- Checkbox & OTP Panel -->
        <div class="bg-card/30 border border-border/60 rounded-xl p-6 shadow-xs flex flex-col gap-6">
          <div class="flex flex-col gap-1">
            <h2 class="text-sm font-bold text-foreground">انتخاب‌ها و کدهای تایید (Selection & OTP)</h2>
            <p class="text-[11px] text-muted-foreground">تعامل با چک‌باکس و فیلد دریافت کد OTP.</p>
          </div>

          <div class="grid grid-cols-1 sm:grid-cols-2 gap-8">
            <div class="flex flex-col gap-3">
              <h3 class="text-[10px] font-bold uppercase tracking-wider text-muted-foreground/80">چک‌باکس با برچسب:</h3>
              <div class="flex items-center gap-2.5 p-3 bg-background/50 border border-border/40 rounded-lg">
                <Checkbox id="terms" v-model:checked="checkboxVal" />
                <div class="flex flex-col gap-0.5 leading-none">
                  <Label for="terms" class="cursor-pointer">پذیرش شرایط و قوانین</Label>
                  <p class="text-[10px] text-muted-foreground font-medium">پذیرش خط‌مشی محرمانگی</p>
                </div>
              </div>
              <p class="text-[10px] text-muted-foreground">
                وضعیت: 
                <span v-if="checkboxVal" class="text-green-600 dark:text-green-400 font-bold flex items-center gap-0.5">
                  <CheckCircle2Icon class="size-3.5" /> پذیرفته شده
                </span>
                <span v-else class="text-destructive font-bold">پذیرفته نشده</span>
              </p>
            </div>

            <div class="flex flex-col gap-3">
              <h3 class="text-[10px] font-bold uppercase tracking-wider text-muted-foreground/80">کد تایید پیامکی (OTP):</h3>
              <div dir="ltr" class="flex justify-end">
                <InputOTP v-model="otpVal" :maxlength="4">
                  <InputOTPGroup>
                    <InputOTPSlot :index="0" />
                    <InputOTPSlot :index="1" />
                    <InputOTPSlot :index="2" />
                    <InputOTPSlot :index="3" />
                  </InputOTPGroup>
                </InputOTP>
              </div>
              <p class="text-[10px] text-muted-foreground text-right">کد تایید ۴ رقمی پیامک شده. <span class="bg-muted px-1 rounded font-mono">{{ otpVal }}</span></p>
            </div>
          </div>
        </div>

        <!-- Popover & Calendar Panel -->
        <div class="bg-card/30 border border-border/60 rounded-xl p-6 shadow-xs flex flex-col gap-6">
          <div class="flex flex-col gap-1">
            <h2 class="text-sm font-bold text-foreground">تقویم و پاپ‌اور (Calendar & Popover)</h2>
            <p class="text-[11px] text-muted-foreground">نمونه نمایش پاپ‌اور حاوی تقویم برای انتخاب تاریخ.</p>
          </div>

          <div class="flex flex-col gap-4">
            <div class="flex flex-col items-start gap-2.5">
              <Label>انتخاب تاریخ قرار ملاقات:</Label>
              
              <Popover>
                <PopoverTrigger as-child>
                  <Button variant="outline" class="w-[240px] justify-start text-right font-normal">
                    <CalendarIcon class="ml-2 size-4 text-muted-foreground" />
                    <span>{{ dateVal ? dateVal.toString() : 'انتخاب تاریخ...' }}</span>
                  </Button>
                </PopoverTrigger>
                <PopoverContent class="w-auto p-0" align="start">
                  <Calendar v-model="dateVal" initial-focus />
                </PopoverContent>
              </Popover>
            </div>
            <p class="text-[10px] text-muted-foreground">تقویم مورد استفاده منطبق بر کتابخانه هسته reka-ui با پشتیبانی از استایل‌دهی تیره/روشن است.</p>
          </div>
        </div>

      </div>

      <!-- TAB 2: SYSTEM TOKENS (COLORS, TYPOGRAPHY, SPACING & ICONS) -->
      <div v-else class="flex flex-col gap-12 w-full">
        
        <!-- Theme Colors Grid -->
        <div class="bg-card/30 border border-border/60 rounded-xl p-6 shadow-xs flex flex-col gap-6 w-full">
          <div class="flex items-center gap-2 text-foreground justify-start">
            <LayoutGridIcon class="size-4 text-primary" />
            <h2 class="text-sm font-bold">پالت رنگ‌های سیستم (Theme Colors)</h2>
          </div>
          <p class="text-xs text-muted-foreground">متغیرهای رسمی استایل‌دهی شدسن بر اساس تیره و روشن (تم رنگ متغیر OKLCH).</p>
          
          <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-4 border-t border-border/40 pt-4">
            <div 
              v-for="color in colors" 
              :key="color.name" 
              class="flex items-center gap-3 p-3 bg-background/50 border border-border/40 rounded-lg hover:border-border transition-colors text-right"
            >
              <!-- Visual Color Swatch -->
              <div :class="['size-12 rounded-lg flex items-center justify-center font-bold text-xs shrink-0', color.class]">
                Aa
              </div>
              <div class="flex flex-col gap-0.5 text-right w-full min-w-0">
                <div class="flex justify-between items-center gap-2">
                  <span class="font-bold text-xs text-foreground">{{ color.name }}</span>
                  <code class="text-[9px] bg-muted/80 px-1 py-0.5 rounded text-muted-foreground font-mono select-all">{{ color.variable }}</code>
                </div>
                <p class="text-[10px] text-muted-foreground truncate leading-normal">{{ color.desc }}</p>
              </div>
            </div>
          </div>

          <div class="border-t border-border/20 pt-4 flex flex-col gap-3">
            <h3 class="text-[10px] font-bold text-muted-foreground uppercase text-right">رنگ‌های ویژه مرزبندی و فوکوس (Borders & Interaction):</h3>
            <div class="grid grid-cols-1 sm:grid-cols-3 gap-4">
              <div 
                v-for="b in systemBorders" 
                :key="b.name" 
                class="flex items-center gap-3 p-2.5 bg-background/30 border border-border/30 rounded-lg text-right"
              >
                <div :class="['w-8 h-8 rounded shrink-0 border border-border/50', b.class]" />
                <div class="flex flex-col text-right">
                  <p class="font-bold text-xs text-foreground">{{ b.name }}</p>
                  <code class="text-[9px] text-muted-foreground font-mono">{{ b.variable }}</code>
                </div>
              </div>
            </div>
          </div>
        </div>

        <!-- Semantic Tints & Shades -->
        <div class="bg-card/30 border border-border/60 rounded-xl p-6 shadow-xs flex flex-col gap-6 w-full animate-in fade-in-50">
          <div class="flex items-center gap-2 text-foreground justify-start">
            <LayoutGridIcon class="size-4 text-primary" />
            <h2 class="text-sm font-bold">طیف‌های فرعی و غلظت رنگ‌ها (Tints & Shades)</h2>
          </div>
          <p class="text-xs text-muted-foreground">تست رنگ‌های اصلی سیستم به همراه طیف‌های مختلف شید و تینت با استفاده از ضرایب اپسیتی (Opacity/Alpha modifiers) مرسوم در شدسن.</p>
          
          <div class="border-t border-border/40 pt-4 flex flex-col gap-6">
            <div 
              v-for="shade in colorShades" 
              :key="shade.name" 
              class="flex flex-col gap-3 pb-6 border-b border-border/20 last:border-0 last:pb-0"
            >
              <div class="flex flex-col sm:flex-row justify-between sm:items-center gap-1 text-right">
                <span class="font-bold text-xs text-foreground">{{ shade.name }}</span>
                <span class="text-[10px] text-muted-foreground">{{ shade.desc }}</span>
              </div>
              
              <div class="grid grid-cols-2 sm:grid-cols-4 md:grid-cols-7 gap-2">
                <!-- 100% (Solid) -->
                <div class="flex flex-col gap-1 p-1 bg-background/50 border border-border/40 rounded-lg">
                  <div :class="['h-8 w-full rounded', `bg-${shade.baseClass}`]" />
                  <div class="flex flex-col text-center font-mono text-[9px] text-muted-foreground leading-normal">
                    <span class="font-bold text-foreground">Solid</span>
                    <span>100%</span>
                  </div>
                </div>
                <!-- 90% -->
                <div class="flex flex-col gap-1 p-1 bg-background/50 border border-border/40 rounded-lg">
                  <div :class="['h-8 w-full rounded', `bg-${shade.baseClass}/90`]" />
                  <div class="flex flex-col text-center font-mono text-[9px] text-muted-foreground leading-normal">
                    <span class="font-bold text-foreground">/90</span>
                    <span>90%</span>
                  </div>
                </div>
                <!-- 80% -->
                <div class="flex flex-col gap-1 p-1 bg-background/50 border border-border/40 rounded-lg">
                  <div :class="['h-8 w-full rounded', `bg-${shade.baseClass}/80`]" />
                  <div class="flex flex-col text-center font-mono text-[9px] text-muted-foreground leading-normal">
                    <span class="font-bold text-foreground">/80</span>
                    <span>80%</span>
                  </div>
                </div>
                <!-- 50% -->
                <div class="flex flex-col gap-1 p-1 bg-background/50 border border-border/40 rounded-lg">
                  <div :class="['h-8 w-full rounded', `bg-${shade.baseClass}/50`]" />
                  <div class="flex flex-col text-center font-mono text-[9px] text-muted-foreground leading-normal">
                    <span class="font-bold text-foreground">/50</span>
                    <span>50%</span>
                  </div>
                </div>
                <!-- 30% -->
                <div class="flex flex-col gap-1 p-1 bg-background/50 border border-border/40 rounded-lg">
                  <div :class="['h-8 w-full rounded', `bg-${shade.baseClass}/30`]" />
                  <div class="flex flex-col text-center font-mono text-[9px] text-muted-foreground leading-normal">
                    <span class="font-bold text-foreground">/30</span>
                    <span>30%</span>
                  </div>
                </div>
                <!-- 20% -->
                <div class="flex flex-col gap-1 p-1 bg-background/50 border border-border/40 rounded-lg">
                  <div :class="['h-8 w-full rounded', `bg-${shade.baseClass}/20`]" />
                  <div class="flex flex-col text-center font-mono text-[9px] text-muted-foreground leading-normal">
                    <span class="font-bold text-foreground">/20</span>
                    <span>20%</span>
                  </div>
                </div>
                <!-- 10% -->
                <div class="flex flex-col gap-1 p-1 bg-background/50 border border-border/40 rounded-lg">
                  <div :class="['h-8 w-full rounded', `bg-${shade.baseClass}/10`]" />
                  <div class="flex flex-col text-center font-mono text-[9px] text-muted-foreground leading-normal">
                    <span class="font-bold text-foreground">/10</span>
                    <span>10%</span>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>

        <!-- Typography / Font Sizes Scale -->
        <div class="bg-card/30 border border-border/60 rounded-xl p-6 shadow-xs flex flex-col gap-6 w-full">
          <div class="flex items-center gap-2 text-foreground justify-start">
            <TypeIcon class="size-4 text-primary" />
            <h2 class="text-sm font-bold">مقیاس سایزبندی قلم‌ها (Font Size Scale)</h2>
          </div>
          <p class="text-xs text-muted-foreground">اندازه‌های مختلف فونت مورد استفاده در طراحی با کلاس‌های استاندارد Tailwind CSS.</p>
          
          <div class="border-t border-border/40 pt-4 flex flex-col gap-1 divide-y divide-border/20">
            <div 
              v-for="sz in fontSizes" 
              :key="sz.name" 
              class="py-3 flex flex-col sm:flex-row sm:items-center justify-between gap-4 text-right"
            >
              <div class="w-36 shrink-0 flex flex-col">
                <span class="font-bold text-xs text-foreground font-mono">{{ sz.name }}</span>
                <span class="text-muted-foreground text-[9px] font-mono mt-0.5">{{ sz.size }} / {{ sz.px }}</span>
              </div>
              <p :class="['text-foreground w-full text-right leading-none', sz.class]">
                خلاقیت با طراحی مینیمال و هوشمند
              </p>
            </div>
          </div>
        </div>

        <!-- Variable Font weights display -->
        <div class="bg-card/30 border border-border/60 rounded-xl p-6 shadow-xs flex flex-col gap-6 w-full">
          <div class="flex items-center gap-2 text-foreground justify-start">
            <TypeIcon class="size-4 text-primary" />
            <h2 class="text-sm font-bold">وزن‌های فونت متغیر ابر (Abar-VF weights)</h2>
          </div>
          <p class="text-xs text-muted-foreground">بررسی رندر ضخامت و پایداری فونت متغیر طراحی شده در وزن‌های مختلف در مرورگر.</p>
          
          <div class="border-t border-border/40 pt-4 flex flex-col gap-1 divide-y divide-border/20">
            <div class="py-3 flex flex-col md:flex-row md:items-center justify-between gap-2 text-right">
              <span class="w-32 shrink-0 text-xs font-bold text-muted-foreground font-mono">Weight 100 (Thin)</span>
              <p class="text-lg text-foreground w-full text-right" style="font-weight: 100">خلاقیت با طراحی مینیمال و هوشمند - وزن ۱۰۰</p>
            </div>
            
            <div class="py-3 flex flex-col md:flex-row md:items-center justify-between gap-2 text-right">
              <span class="w-32 shrink-0 text-xs font-bold text-muted-foreground font-mono">Weight 300 (Light)</span>
              <p class="text-lg text-foreground w-full text-right" style="font-weight: 300">خلاقیت با طراحی مینیمال و هوشمند - وزن ۳۰۰</p>
            </div>

            <div class="py-3 flex flex-col md:flex-row md:items-center justify-between gap-2 text-right">
              <span class="w-32 shrink-0 text-xs font-bold text-muted-foreground font-mono">Weight 400 (Regular)</span>
              <p class="text-lg text-foreground w-full text-right" style="font-weight: 400">خلاقیت با طراحی مینیمال و هوشمند - وزن ۴۰۰</p>
            </div>

            <div class="py-3 flex flex-col md:flex-row md:items-center justify-between gap-2 text-right">
              <span class="w-32 shrink-0 text-xs font-bold text-muted-foreground font-mono">Weight 500 (Medium)</span>
              <p class="text-lg text-foreground w-full text-right" style="font-weight: 500">خلاقیت با طراحی مینیمال و هوشمند - وزن ۵۰۰</p>
            </div>

            <div class="py-3 flex flex-col md:flex-row md:items-center justify-between gap-2 text-right">
              <span class="w-32 shrink-0 text-xs font-bold text-muted-foreground font-mono">Weight 700 (Bold)</span>
              <p class="text-lg text-foreground w-full text-right" style="font-weight: 700">خلاقیت با طراحی مینیمال و هوشمند - وزن ۷۰۰</p>
            </div>

            <div class="py-3 flex flex-col md:flex-row md:items-center justify-between gap-2 text-right">
              <span class="w-32 shrink-0 text-xs font-bold text-muted-foreground font-mono">Weight 800 (Extra Bold)</span>
              <p class="text-lg text-foreground w-full text-right" style="font-weight: 800">خلاقیت با طراحی مینیمال و هوشمند - وزن ۸۰۰</p>
            </div>

            <div class="py-3 flex flex-col md:flex-row md:items-center justify-between gap-2 text-right">
              <span class="w-32 shrink-0 text-xs font-bold text-muted-foreground font-mono">Weight 900 (Black)</span>
              <p class="text-lg text-foreground w-full text-right" style="font-weight: 900">خلاقیت با طراحی مینیمال و هوشمند - وزن ۹۰۰</p>
            </div>
          </div>
        </div>

        <!-- Spacing System Scale -->
        <div class="bg-card/30 border border-border/60 rounded-xl p-6 shadow-xs flex flex-col gap-6 w-full">
          <div class="flex items-center gap-2 text-foreground justify-start">
            <RulerIcon class="size-4 text-primary" />
            <h2 class="text-sm font-bold">سیستم فاصله‌گذاری و ابعاد (Spacing System)</h2>
          </div>
          <p class="text-xs text-muted-foreground">تست مقادیر پیش‌فرض فاصله‌گذاری (Padding / Margin / Gaps) در پلتفرم به صورت پیکسل معادل.</p>
          
          <div class="border-t border-border/40 pt-4 flex flex-col gap-4">
            <div 
              v-for="space in spacings" 
              :key="space.value" 
              class="flex items-center gap-4 text-right"
            >
              <!-- Label -->
              <div class="w-24 shrink-0 flex items-center justify-between text-xs">
                <span class="font-bold text-foreground font-mono">size-{{ space.value }}</span>
                <span class="text-muted-foreground text-[10px] font-mono">{{ space.px }}</span>
              </div>
              
              <!-- Spacing Indicator Bar -->
              <div class="flex-1 bg-muted/40 h-6 rounded border border-border/30 flex items-center px-1 overflow-hidden">
                <div 
                  :style="{ width: space.px }" 
                  class="h-4 bg-primary/20 hover:bg-primary/40 border border-primary/30 rounded transition-colors" 
                  :title="`size-${space.value}: ${space.px} / ${space.rem}`"
                />
              </div>
              
              <!-- Details -->
              <div class="w-16 shrink-0 text-left text-[10px] text-muted-foreground font-mono">
                {{ space.rem }}
              </div>
            </div>
          </div>
        </div>

        <!-- Lucide Icons Showcase Grid -->
        <div class="bg-card/30 border border-border/60 rounded-xl p-6 shadow-xs flex flex-col gap-6 w-full">
          <div class="flex items-center gap-2 text-foreground justify-start">
            <SettingsIcon class="size-4 text-primary" />
            <h2 class="text-sm font-bold">مجموعه آیکون‌های پایه (Lucide Icons Showcase)</h2>
          </div>
          <p class="text-xs text-muted-foreground">۲۶ آیکون پایه و پراستفاده پکیج Lucide که در بخش‌های مختلف پروژه کاربرد دارند.</p>
          
          <div class="border-t border-border/40 pt-4">
            <div class="grid grid-cols-2 sm:grid-cols-4 md:grid-cols-5 gap-4">
              <div 
                v-for="item in baseIcons" 
                :key="item.name" 
                class="flex flex-col items-center justify-center p-4 bg-background/50 border border-border/40 rounded-xl hover:border-primary/40 hover:bg-primary/5 transition-all group cursor-pointer"
              >
                <component :is="item.icon" class="size-6 text-muted-foreground group-hover:text-primary transition-colors mb-2" />
                <span class="text-[10px] text-foreground font-mono font-semibold">{{ item.name }}</span>
              </div>
            </div>
          </div>
        </div>

      </div>

    </div>
  </div>
</template>
