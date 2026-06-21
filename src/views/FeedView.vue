<script setup lang="ts">
import { ref, computed, onMounted } from 'vue'
import { useRouter } from 'vue-router'
import PlatformHeader from '@/components/PlatformHeader.vue'
import PlatformNavigation from '@/components/PlatformNavigation.vue'
import PostCard from '@/components/PostCard.vue'
import TopicCard from '@/components/TopicCard.vue'
import ProfileCard from '@/components/ProfileCard.vue'
import ProfileOptionCard from '@/components/ProfileOptionCard.vue'
import CreateTopicModal from '@/components/CreateTopicModal.vue'
import BotConnectionCard from '@/components/BotConnectionCard.vue'
import TopicEditView from '@/components/TopicEditView.vue'
import TopicConnectionsView from '@/components/TopicConnectionsView.vue'
import { Button } from '@/components/ui/button'
import { Switch } from '@/components/ui/switch'
import { SparklesIcon, CheckCircle2Icon, LinkIcon, LogOutIcon, PlusIcon, ChevronRightIcon, SunIcon, MoonIcon } from '@lucide/vue'

// Tab state ('home' | 'profile')
const currentTab = ref<'home' | 'profile'>('home')

// Subview connection state
const showConnections = ref(false)

// Active topic subview ('feed' | 'edit' | 'connections')
const activeTopicSubView = ref<'feed' | 'edit' | 'connections'>('feed')

// Header computed states for platform and connections view
const headerIsInnerPage = computed(() => {
  if (currentTab.value === 'home') {
    return selectedTopic.value !== null
  } else {
    return showConnections.value
  }
})

const headerInnerTitle = computed(() => {
  if (currentTab.value === 'home') {
    return selectedTopic.value?.title || ''
  } else {
    return 'مدیریت اتصال‌ها'
  }
})

const headerTitle = computed(() => {
  if (currentTab.value === 'profile') {
    return 'پروفایل'
  }
  return 'تفکر'
})

const handleHeaderBack = () => {
  if (currentTab.value === 'home') {
    if (activeTopicSubView.value !== 'feed') {
      activeTopicSubView.value = 'feed'
    } else {
      selectedTopic.value = null
    }
  } else {
    showConnections.value = false
  }
}

const telegramBotStatus = ref<'disconnected' | 'connecting' | 'connected' | 'conflict'>('connected')
const telegramBotToken = ref('123456789:ABCdefGhIJKlmNoPQRsTUVwxyZ')
const baleBotStatus = ref<'disconnected' | 'connecting' | 'connected' | 'conflict'>('disconnected')
const baleBotToken = ref('')

const handleTelegramConnect = (token: string) => {
  telegramBotStatus.value = 'connecting'
  setTimeout(() => {
    telegramBotToken.value = token
    telegramBotStatus.value = 'connected'
    triggerToast('ربات تلگرام با موفقیت متصل شد.')
  }, 1000)
}

const handleTelegramDisconnect = () => {
  telegramBotStatus.value = 'disconnected'
  telegramBotToken.value = ''
  triggerToast('اتصال ربات تلگرام قطع شد.')
}

const handleBaleConnect = (token: string) => {
  baleBotStatus.value = 'connecting'
  setTimeout(() => {
    baleBotToken.value = token
    baleBotStatus.value = 'connected'
    triggerToast('ربات بله با موفقیت متصل شد.')
  }, 1000)
}

const handleBaleDisconnect = () => {
  baleBotStatus.value = 'disconnected'
  baleBotToken.value = ''
  triggerToast('اتصال ربات بله قطع شد.')
}

const activeConnectionsCount = computed(() => {
  let count = 0
  if (telegramBotStatus.value === 'connected') count++
  if (baleBotStatus.value === 'connected') count++
  return count
})

const connectionsBadgeText = computed(() => {
  const count = activeConnectionsCount.value
  if (count === 0) return 'بدون اتصال'
  return `${count} فعال`
})

const connectionsBadgeVariant = computed(() => {
  return activeConnectionsCount.value > 0 ? 'secondary' : 'default'
})

// Selected topic for subpage view (null means home topic list)
const selectedTopic = ref<any>(null)

// Posts belonging to the currently selected topic
const currentTopicPosts = computed(() => {
  if (!selectedTopic.value) return []
  const title = selectedTopic.value.title
  if (title.includes('پشتیبانی')) {
    return posts.value.filter(p => p.id === 3)
  } else {
    return posts.value.filter(p => p.id === 1 || p.id === 2)
  }
})

// Search query binding
const searchQuery = ref('')

// Create Topic Modal State
const isCreateModalOpen = ref(false)

const handleCreateTopic = (newTopic: { title: string; description: string; image?: string }) => {
  topics.value.push({
    title: newTopic.title,
    description: newTopic.description,
    image: newTopic.image,
    unreadCount: 0,
    time: 'هم‌اکنون',
    labels: ['جدید'],
    pinned: false
  })
  isCreateModalOpen.value = false
  triggerToast(`تاپیک «${newTopic.title}» با موفقیت ایجاد شد.`)
}

// Toast Notification state
const toastMsg = ref('')
const isToastActive = ref(false)

const triggerToast = (message: string) => {
  toastMsg.value = message
  isToastActive.value = true
  setTimeout(() => {
    isToastActive.value = false
  }, 2500)
}

// User Profile Data state (matching ProfileCard props)
const userProfile = ref({
  name: 'شایان مهدوی',
  phone: '۰۹۱۲۳۴۵۶۷۸۹',
  avatar: 'https://images.unsplash.com/photo-1534528741775-53994a69daeb?w=100&auto=format&fit=crop&q=80',
  statusText: 'فعال',
  statusVariant: 'success' as 'success' | 'warning' | 'info' | 'muted'
})

const handleProfileEdit = () => {
  triggerToast('ویرایش مشخصات کاربری به زودی فعال می‌شود.')
}

const router = useRouter()

const handleLogout = () => {
  triggerToast('در حال خروج از سیستم...')
  setTimeout(() => {
    selectedTopic.value = null
    currentTab.value = 'home'
    router.push('/')
  }, 1000)
}

// Topics list data
const topics = ref([
  {
    title: 'پشتیبانی فنی پلتفرم',
    description: 'درخواست راهنمایی در مورد دیزاین سیستم پلتفرم',
    image: 'https://images.unsplash.com/photo-1534528741775-53994a69daeb?w=100&auto=format&fit=crop&q=80',
    unreadCount: 2,
    time: '۵ دقیقه پیش',
    labels: ['پشتیبانی', 'فوری'],
    pinned: true
  },
  {
    title: 'اطلاعیه‌های پلتفرم تفکر',
    description: 'آخرین بروزرسانی‌ها و اطلاعیه‌های رسمی سیستم',
    unreadCount: 0,
    time: '۱ ساعت پیش',
    labels: ['رسمی'],
    pinned: true
  }
])

// Posts list data
const posts = ref([
  {
    id: 1,
    author: {
      name: 'سارا رضایی',
      avatarUrl: 'https://images.unsplash.com/photo-1494790108377-be9c29b29330?w=100&auto=format&fit=crop&q=80',
      role: 'نویسنده ارشد پلتفرم'
    },
    type: 'image' as const,
    content: 'تفکر جدید در طراحی رابط‌های کاربری مینیمال با استفاده از پالت‌های رنگی بهینه‌شده oklch و تایپوگرافی پینار.',
    mediaUrl: 'https://images.unsplash.com/photo-1618005182384-a83a8bd57fbe?w=600&auto=format&fit=crop&q=80',
    timestamp: '۱۰ دقیقه پیش',
    likesCount: 42,
    liked: true,
    comments: [
      {
        id: 101,
        authorName: 'شایان مهدوی',
        text: 'بسیار ترکیب رنگی تمیز و شیکی دارد، واقعا کار چشم‌نواز شده است!',
        timestamp: '۸ دقیقه پیش'
      }
    ]
  },
  {
    id: 2,
    author: {
      name: 'علی کریمی',
      avatarUrl: 'https://images.unsplash.com/photo-1507003211169-0a1dd7228f2d?w=100&auto=format&fit=crop&q=80',
      role: 'مدیر محصول'
    },
    type: 'audio' as const,
    content: 'پادکست اختصاصی تفکر - بررسی تخصصی آینده دیزاین سیستم‌های فارسی در وب.',
    mediaUrl: 'https://www.soundhelix.com/examples/mp3/SoundHelix-Song-1.mp3',
    duration: '۰۶:۱۲',
    timestamp: '۱ ساعت پیش',
    likesCount: 89,
    liked: false,
    comments: []
  },
  {
    id: 3,
    author: {
      name: 'فرید مهدوی',
      avatarUrl: '',
      role: 'توسعه‌دهنده ارشد فرانت‌اند'
    },
    type: 'text' as const,
    content: 'طراحی درست دیزاین سیستم همواره باید در جهت تسهیل کار کاربر نهایی باشد. استفاده از فونت متغیر پینار به همراه نقطه‌های گرد کامل، خوانایی متون طولانی را در صفحه موبایل به شکل فوق‌العاده‌ای بهبود بخشیده است.',
    timestamp: '۳ ساعت پیش',
    likesCount: 15,
    liked: false,
    comments: []
  }
])

// Filtering computed logic based on search
const filteredTopics = computed(() => {
  if (!searchQuery.value.trim()) return topics.value
  const q = searchQuery.value.toLowerCase().trim()
  return topics.value.filter(t => 
    t.title.toLowerCase().includes(q) || 
    t.description.toLowerCase().includes(q)
  )
})

const filteredPosts = computed(() => {
  if (!searchQuery.value.trim()) return posts.value
  const q = searchQuery.value.toLowerCase().trim()
  return posts.value.filter(p => 
    (p.content && p.content.toLowerCase().includes(q)) || 
    p.author.name.toLowerCase().includes(q) ||
    p.author.role?.toLowerCase().includes(q)
  )
})

const filteredCurrentTopicPosts = computed(() => {
  if (!searchQuery.value.trim()) return currentTopicPosts.value
  const q = searchQuery.value.toLowerCase().trim()
  return currentTopicPosts.value.filter(p => 
    (p.content && p.content.toLowerCase().includes(q)) || 
    p.author.name.toLowerCase().includes(q)
  )
})

const handlePostLike = (postId: string | number, isLiked: boolean) => {
  const post = posts.value.find(p => p.id === postId)
  if (post) {
    post.liked = isLiked
    post.likesCount = isLiked ? (post.likesCount || 0) + 1 : (post.likesCount || 1) - 1
    triggerToast(isLiked ? 'پست پسندیده شد.' : 'پسندیدن پست لغو شد.')
  }
}

const handlePostShare = (postId: string | number) => {
  triggerToast(`لینک پست شماره ${postId} در حافظه موقت کپی شد.`);
}
const handleAddComment = (postId: string | number, comment: any) => {
  const post = posts.value.find(p => p.id === postId)
  if (post) {
    post.comments = [...(post.comments || []), comment]
    triggerToast('دیدگاه شما با موفقیت ثبت شد.');
  }
}

const handleEditTopic = () => {
  if (!selectedTopic.value) return
  activeTopicSubView.value = 'edit'
}

const handleDeleteTopic = () => {
  if (!selectedTopic.value) return
  const title = selectedTopic.value.title
  const index = topics.value.findIndex(t => t.title === title)
  if (index !== -1) {
    topics.value.splice(index, 1)
    selectedTopic.value = null
    triggerToast(`تاپیک «${title}» با موفقیت حذف شد.`)
  }
}

const handleTopicConnections = () => {
  if (!selectedTopic.value) return
  activeTopicSubView.value = 'connections'
}

const handleSaveTopicEdit = (updatedTopic: { title: string; description: string; image: string }) => {
  if (!selectedTopic.value) return
  selectedTopic.value.title = updatedTopic.title
  selectedTopic.value.description = updatedTopic.description
  selectedTopic.value.image = updatedTopic.image
  triggerToast('مشخصات تاپیک با موفقیت ویرایش شد.')
}

const handleSaveTopicConnections = (data: { autoRepost: boolean; selectedChannels: string[] }) => {
  if (!selectedTopic.value) return
  selectedTopic.value.autoRepost = data.autoRepost
  selectedTopic.value.selectedChannels = data.selectedChannels
  triggerToast('تنظیمات اتصالات تاپیک با موفقیت به روز رسانی شد.')
}

const handleGoToConnections = () => {
  activeTopicSubView.value = 'feed'
  selectedTopic.value = null
  currentTab.value = 'profile'
  showConnections.value = true
}

// Theme switcher logic (Light/Dark mode)
const isDark = ref(false)

const toggleTheme = () => {
  isDark.value = !isDark.value
  if (isDark.value) {
    document.documentElement.classList.add('dark')
    localStorage.setItem('theme', 'dark')
  } else {
    document.documentElement.classList.remove('dark')
    localStorage.setItem('theme', 'light')
  }
}

onMounted(() => {
  const savedTheme = localStorage.getItem('theme')
  const prefersDark = window.matchMedia('(prefers-color-scheme: dark)').matches
  if (savedTheme === 'dark' || (!savedTheme && prefersDark)) {
    document.documentElement.classList.add('dark')
    isDark.value = true
  } else {
    document.documentElement.classList.remove('dark')
    isDark.value = false
  }
})
</script>

<template>
  <div class="w-full max-w-[500px] flex flex-col gap-5 pb-24 text-right mx-auto relative animate-in fade-in slide-in-from-bottom-3 duration-300" dir="rtl">
    
    <!-- Header - Sticky (Shown on main views, custom views render their own headers) -->
    <div 
      v-if="currentTab === 'profile' || (currentTab === 'home' && selectedTopic === null) || (currentTab === 'home' && selectedTopic !== null && activeTopicSubView === 'feed')"
      class="sticky top-0 z-40 bg-background/85 dark:bg-background/80 backdrop-blur-md pt-4 pb-2.5"
    >
      <PlatformHeader 
        :is-inner-page="headerIsInnerPage" 
        :inner-title="headerInnerTitle" 
        :title="headerTitle" 
        :loading="telegramBotStatus === 'connecting' || baleBotStatus === 'connecting'"
        :show-options-button="currentTab === 'home' && selectedTopic !== null"
        :show-search-button="currentTab === 'home' && selectedTopic === null"
        v-model:search-query="searchQuery"
        @back="handleHeaderBack"
        @edit-topic="handleEditTopic"
        @delete-topic="handleDeleteTopic"
        @topic-connections="handleTopicConnections"
      />
    </div>

    <!-- MAIN CONTENT AREA -->
    <div class="w-full flex-1">
      <transition name="fade-slide" mode="out-in">
        <!-- TAB 1: HOME (TOPIC LIST & DETAILS & SUBVIEWS) -->
        <div v-if="currentTab === 'home'" key="home" class="flex flex-col gap-5 w-full">
          
          <transition name="fade-slide" mode="out-in">
            <!-- List View: Search and vertical TopicCards -->
            <div v-if="selectedTopic === null" key="list" class="flex flex-col gap-5 w-full">
              <!-- Grouped TopicCards Container (Borderless list grouped inside a single glass card wrapper) -->
              <div class="flex flex-col gap-1 w-full bg-card/25 dark:bg-card/15 border border-border/50 rounded-3xl p-2.5 shadow-xs">
                <TopicCard 
                  v-for="topic in filteredTopics" 
                  :key="topic.title"
                  v-bind="topic"
                  @click="selectedTopic = topic; activeTopicSubView = 'feed'"
                />
                
                <div v-if="filteredTopics.length === 0" class="text-center py-12 text-xs text-muted-foreground font-medium">
                  هیچ تاپیکی مطابق با جستجوی شما یافت نشد.
                </div>
              </div>
            </div>

            <!-- Detail View: Topic Posts Feed -->
            <div v-else-if="activeTopicSubView === 'feed'" key="feed" class="flex flex-col gap-4 w-full">
              <!-- Posts belonging to this topic -->
              <PostCard 
                v-for="post in filteredCurrentTopicPosts" 
                :key="post.id"
                v-bind="post"
                @like="handlePostLike"
                @share="handlePostShare"
                @add-comment="handleAddComment"
              />
              
              <div v-if="filteredCurrentTopicPosts.length === 0" class="text-center py-12 bg-card/20 rounded-xl border border-border/60 text-xs text-muted-foreground font-medium">
                هیچ پست یا مطلبی در این تاپیک ثبت نشده است.
              </div>
            </div>

            <!-- Topic Edit View -->
            <div v-else-if="activeTopicSubView === 'edit'" key="edit" class="w-full">
              <TopicEditView 
                :topic="selectedTopic"
                @back="activeTopicSubView = 'feed'"
                @save="handleSaveTopicEdit"
              />
            </div>

            <!-- Topic Connections View -->
            <div v-else-if="activeTopicSubView === 'connections'" key="connections" class="w-full">
              <TopicConnectionsView 
                :topic="selectedTopic"
                :telegram-connected="telegramBotStatus === 'connected'"
                :bale-connected="baleBotStatus === 'connected'"
                @back="activeTopicSubView = 'feed'"
                @goToConnections="handleGoToConnections"
                @save="handleSaveTopicConnections"
              />
            </div>
          </transition>

        </div>

        <!-- TAB 2: PROFILE EDIT -->
        <div v-else key="profile" class="w-full flex flex-col gap-4.5">
          
          <transition name="fade-slide" mode="out-in">
            <!-- Subview: Connections -->
            <div v-if="showConnections" key="connections-list" class="flex flex-col gap-4.5">
              <!-- Connection Cards list -->
              <div class="flex flex-col gap-4.5">
                <BotConnectionCard 
                  platform="telegram"
                  :initialStatus="telegramBotStatus"
                  :initialToken="telegramBotToken"
                  @connect="handleTelegramConnect"
                  @disconnect="handleTelegramDisconnect"
                  @update:status="(st) => telegramBotStatus = st"
                />
                <BotConnectionCard 
                  platform="bale"
                  :initialStatus="baleBotStatus"
                  :initialToken="baleBotToken"
                  @connect="handleBaleConnect"
                  @disconnect="handleBaleDisconnect"
                  @update:status="(st) => baleBotStatus = st"
                />
              </div>
            </div>

            <!-- Main Profile Menu -->
            <div v-else key="profile-main" class="flex flex-col gap-4.5">
              <!-- Profile Card -->
              <ProfileCard 
                v-bind="userProfile"
                :topics-count="topics.length"
                :posts-count="posts.length"
                :connections-count="activeConnectionsCount"
                @edit="handleProfileEdit"
              />

              <!-- Options -->
              <div class="flex flex-col gap-3">
                <ProfileOptionCard 
                  title="اتصال‌ها"
                  description="مدیریت حساب‌های متصل و سرویس‌های خارجی"
                  :icon="LinkIcon"
                  :badge-text="connectionsBadgeText"
                  :badge-variant="connectionsBadgeVariant"
                  @click="showConnections = true"
                />

                <!-- Light/Dark Mode Switcher -->
                <div
                  @click="toggleTheme"
                  class="w-full flex items-center justify-start p-3.5 rounded-2xl bg-card/45 backdrop-blur-md border border-border/40 hover:bg-muted/40 hover:border-primary/20 transition-all duration-200 cursor-pointer select-none text-right relative active:scale-[0.995] group gap-3.5"
                  dir="rtl"
                >
                  <!-- Right side: Icon Container -->
                  <div class="size-10 rounded-xl bg-primary/[0.03] flex items-center justify-center text-primary shrink-0 transition-transform duration-200 group-hover:scale-102">
                    <transition name="icon-pop" mode="out-in">
                      <component :is="isDark ? MoonIcon : SunIcon" class="size-5 shrink-0" :key="isDark ? 'moon' : 'sun'" />
                    </transition>
                  </div>

                  <!-- Middle: Title & Description -->
                  <div class="flex-1 flex flex-col text-right gap-1 min-w-0">
                    <span class="text-xs font-bold text-foreground truncate leading-none">
                      حالت تاریک
                    </span>
                    <span class="text-[10px] text-muted-foreground truncate leading-relaxed">
                      تغییر تم رنگی برنامه به حالت شب یا روز
                    </span>
                  </div>

                  <!-- Left side: Switch toggle -->
                  <div class="flex items-center gap-2 shrink-0" @click.stop>
                    <Switch :checked="isDark" @update:checked="toggleTheme" />
                  </div>
                </div>

                <ProfileOptionCard 
                  title="خروج از حساب کاربری"
                  description="خروج امن از سیستم و بازگشت به صفحه ورود"
                  :icon="LogOutIcon"
                  badge-variant="destructive"
                  @click="handleLogout"
                />
              </div>
            </div>
          </transition>

        </div>
      </transition>
    </div>

    <!-- Bottom Navigation tabs -->
    <div v-if="selectedTopic === null" class="fixed bottom-6 inset-x-0 flex justify-center z-40 pointer-events-none px-6">
      <div class="pointer-events-auto">
        <PlatformNavigation v-model="currentTab" />
      </div>
    </div>

    <!-- Floating Plus Button to Create Topic (Bottom Right Corner - Raised above floating tab bar, aligned inside page grid) -->
    <div 
      v-if="currentTab === 'home' && selectedTopic === null"
      class="fixed bottom-26 inset-x-0 max-w-[500px] mx-auto w-full z-45 pointer-events-none px-6 flex justify-start"
      dir="rtl"
    >
      <Button 
        @click="isCreateModalOpen = true"
        class="pointer-events-auto size-12 rounded-full bg-primary text-primary-foreground flex items-center justify-center hover:scale-105 active:scale-95 transition-all shadow-lg outline-none cursor-pointer p-0"
        title="ایجاد تاپیک جدید"
      >
        <PlusIcon class="size-6" />
      </Button>
    </div>

    <!-- Create Topic Modal -->
    <CreateTopicModal 
      :is-open="isCreateModalOpen" 
      @close="isCreateModalOpen = false" 
      @create="handleCreateTopic"
    />

    <!-- Dynamic Premium Toast Notification -->
    <transition name="toast-slide">
      <div 
        v-if="isToastActive" 
        class="fixed top-6 inset-x-0 flex justify-center z-50 px-6 pointer-events-none"
      >
        <div class="bg-primary text-primary-foreground text-xs font-bold px-4 py-2.5 rounded-xl shadow-lg border border-primary/20 flex items-center gap-2 pointer-events-auto">
          <CheckCircle2Icon class="size-4 text-primary-foreground" />
          <span>{{ toastMsg }}</span>
        </div>
      </div>
    </transition>

  </div>
</template>

<style scoped>
/* Toast Animation */
.toast-slide-enter-active,
.toast-slide-leave-active {
  transition: all 0.3s cubic-bezier(0.16, 1, 0.3, 1);
}

.toast-slide-enter-from {
  transform: translateY(-20px);
  opacity: 0;
}

.toast-slide-leave-to {
  transform: translateY(-10px);
  opacity: 0;
}

/* Minimal Fade & Slide transition */
.fade-slide-enter-active,
.fade-slide-leave-active {
  transition: all 0.25s cubic-bezier(0.4, 0, 0.2, 1);
}

.fade-slide-enter-from {
  opacity: 0;
  transform: translateY(8px);
}

.fade-slide-leave-to {
  opacity: 0;
  transform: translateY(-8px);
}

/* Icon Pop Transition */
.icon-pop-enter-active,
.icon-pop-leave-active {
  transition: all 0.35s cubic-bezier(0.34, 1.56, 0.64, 1);
}

.icon-pop-enter-from {
  transform: scale(0.6) rotate(-90deg);
  opacity: 0;
}

.icon-pop-leave-to {
  transform: scale(0.6) rotate(90deg);
  opacity: 0;
}
</style>
