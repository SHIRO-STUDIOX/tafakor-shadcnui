<script setup lang="ts">
import { ref, computed } from 'vue'
import { useRouter } from 'vue-router'
import PlatformHeader from '@/components/PlatformHeader.vue'
import PlatformNavigation from '@/components/PlatformNavigation.vue'
import MinimalSearch from '@/components/MinimalSearch.vue'
import PostCard from '@/components/PostCard.vue'
import TopicCard from '@/components/TopicCard.vue'
import ProfileCard from '@/components/ProfileCard.vue'
import ProfileOptionCard from '@/components/ProfileOptionCard.vue'
import CreateTopicModal from '@/components/CreateTopicModal.vue'
import { SparklesIcon, CheckCircle2Icon, LinkIcon, LogOutIcon, PlusIcon } from '@lucide/vue'

// Tab state ('home' | 'profile')
const currentTab = ref<'home' | 'profile'>('home')

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
  triggerToast(`گفتگوی «${newTopic.title}» با موفقیت ایجاد شد.`)
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
    title: 'گفتگو با پشتیبانی فنی',
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
</script>

<template>
  <div class="w-full max-w-[500px] flex flex-col gap-5 pb-24 text-right mx-auto relative animate-in fade-in slide-in-from-bottom-3 duration-300" dir="rtl">
    
    <!-- Header (Only shown in Home tab) -->
    <PlatformHeader 
      v-if="currentTab === 'home'"
      :is-inner-page="selectedTopic !== null" 
      :inner-title="selectedTopic?.title" 
      title="تفکر" 
      @back="selectedTopic = null"
    />

    <!-- TAB 1: HOME (TOPIC LIST & DETAILS) -->
    <div v-if="currentTab === 'home'" class="flex flex-col gap-5 w-full">
      
      <!-- List View: Search and vertical TopicCards -->
      <div v-if="selectedTopic === null" class="flex flex-col gap-5 w-full animate-in fade-in duration-200">
        <!-- Search Input -->
        <MinimalSearch v-model="searchQuery" />

        <!-- Vertical TopicCards -->
        <div class="flex flex-col gap-3 w-full">
          <TopicCard 
            v-for="topic in filteredTopics" 
            :key="topic.title"
            v-bind="topic"
            class="bg-card/45 border border-border/60 rounded-2xl p-4 shadow-2xs hover:bg-card/85 transition-all duration-200"
            @click="selectedTopic = topic"
          />
          
          <div v-if="filteredTopics.length === 0" class="text-center py-12 bg-card/20 rounded-2xl border border-border/60 text-xs text-muted-foreground font-medium">
            هیچ گفتگویی مطابق با جستجوی شما یافت نشد.
          </div>
        </div>
      </div>

      <!-- Detail View: Topic Posts Feed -->
      <div v-else class="flex flex-col gap-4 w-full animate-in fade-in slide-in-from-bottom-4 duration-300">
        <!-- Posts belonging to this topic -->
        <PostCard 
          v-for="post in filteredCurrentTopicPosts" 
          :key="post.id"
          v-bind="post"
          @like="handlePostLike"
          @share="handlePostShare"
          @add-comment="handleAddComment"
        />
        
        <div v-if="filteredCurrentTopicPosts.length === 0" class="text-center py-12 bg-card/20 rounded-2xl border border-border/60 text-xs text-muted-foreground font-medium">
          هیچ مطلبی در این گفتگو ثبت نشده است.
        </div>
      </div>

    </div>

    <!-- TAB 2: PROFILE EDIT -->
    <div v-else class="w-full flex flex-col gap-4.5 animate-in fade-in duration-200">
      <!-- Profile Card -->
      <ProfileCard 
        v-bind="userProfile"
        @edit="handleProfileEdit"
      />

      <!-- Options -->
      <div class="flex flex-col gap-3">
        <ProfileOptionCard 
          title="اتصال‌ها"
          description="مدیریت حساب‌های متصل و سرویس‌های خارجی"
          :icon="LinkIcon"
          badge-text="۳ فعال"
          badge-variant="secondary"
          @click="triggerToast('ورود به بخش مدیریت اتصال‌ها')"
        />

        <ProfileOptionCard 
          title="خروج از حساب کاربری"
          description="خروج امن از سیستم و بازگشت به صفحه ورود"
          :icon="LogOutIcon"
          badge-variant="destructive"
          @click="handleLogout"
        />
      </div>
    </div>

    <!-- Bottom Navigation tabs -->
    <div v-if="selectedTopic === null" class="fixed bottom-6 inset-x-0 flex justify-center z-40 pointer-events-none px-6">
      <div class="pointer-events-auto">
        <PlatformNavigation v-model="currentTab" />
      </div>
    </div>

    <!-- Floating Plus Button to Create Topic (Bottom Right Corner) -->
    <button 
      v-if="currentTab === 'home' && selectedTopic === null"
      @click="isCreateModalOpen = true"
      class="fixed bottom-6 right-6 z-45 size-12 rounded-full bg-primary text-primary-foreground flex items-center justify-center hover:scale-105 active:scale-95 transition-all shadow-lg outline-none cursor-pointer"
      title="ایجاد گفتگوی جدید"
    >
      <PlusIcon class="size-6" />
    </button>

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
</style>
