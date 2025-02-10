<template>
  <div class="instagram-grid" @scroll.passive="handleScroll" ref="scrollContainer">
    <div
      v-for="(image, index) in images"
      :key="index"
      :class="`grid-item grid-item-${getGridItemClass(index)}`"
    >
      <img :src="image.url" alt="Random Image" class="image" />
      <div class="hashtags">
        <span v-for="(tag, tagIndex) in image.tags" :key="tagIndex" class="hashtag"
          >#{{ tag }}</span
        >
      </div>
    </div>
    <div v-if="loading" class="loading">加载中...</div>
  </div>
</template>

<script>
import { ref, onMounted, onBeforeUnmount } from 'vue'

export default {
  name: 'InstagramGrid',
  setup() {
    const images = ref([])
    const loading = ref(false)
    const page = ref(1)
    const scrollContainer = ref(null)

    // 生成随机标签
    const generateTags = () => {
      const randomTags = [
        'nature',
        'travel',
        'food',
        'art',
        'fashion',
        'architecture',
        'animals',
        'landscape',
      ]
      return Array.from(
        { length: 3 },
        () => randomTags[Math.floor(Math.random() * randomTags.length)],
      )
    }

    // 获取图片数据（模拟分页）
    const fetchImages = async (pageNum = 1) => {
      const perPage = 9 // 每页加载9张图片
      const newImages = []

      for (let i = 0; i < perPage; i++) {
        const width = Math.floor(Math.random() * 400) + 200
        const height = Math.floor(Math.random() * 400) + 200
        newImages.push({
          url: `https://picsum.photos/${width}/${height}?random=${pageNum}_${i}`,
          tags: generateTags(),
        })
      }

      // 模拟API延迟
      await new Promise((resolve) => setTimeout(resolve, 800))
      return newImages
    }

    // 加载更多图片
    const loadMore = async () => {
      if (loading.value) return
      loading.value = true
      try {
        const newImages = await fetchImages(page.value)
        images.value = [...images.value, ...newImages]
        page.value++
      } finally {
        loading.value = false
      }
    }

    // 滚动处理
    const handleScroll = () => {
      const container = scrollContainer.value
      if (!container) return

      const { scrollTop, scrollHeight, clientHeight } = container
      const threshold = 100 // 距离底部100px时触发加载

      if (scrollHeight - (scrollTop + clientHeight) < threshold) {
        loadMore()
      }
    }

    // 初始化加载
    onMounted(async () => {
      await loadMore()
      // 添加滚动监听
      if (scrollContainer.value) {
        scrollContainer.value.addEventListener('scroll', handleScroll)
      }
    })

    // 移除监听
    onBeforeUnmount(() => {
      if (scrollContainer.value) {
        scrollContainer.value.removeEventListener('scroll', handleScroll)
      }
    })

    // 动态布局分类
    const getGridItemClass = (index) => {
      const positionInPage = index % 9
      if (index === 1) return 'large'
      if ([0, 2].includes(positionInPage)) return 'medium'
      return 'small'
    }

    return {
      images,
      loading,
      scrollContainer,
      getGridItemClass,
      handleScroll,
    }
  },
}
</script>

<style scoped>
.instagram-grid {
  display: grid;
  grid-template-columns: repeat(6, 1fr);
  gap: 10px;
  padding: 20px;
  max-height: 100vh;
  overflow-y: auto;
}

.grid-item {
  position: relative;
  border-radius: 8px;
  overflow: hidden;
  background: #f0f0f0;
  min-height: 200px;
}

.image {
  width: 100%;
  height: 100%;
  object-fit: cover;
  transition: transform 0.3s ease;
}

.image:hover {
  transform: scale(1.05);
}

.hashtags {
  position: absolute;
  bottom: 10px;
  left: 10px;
  right: 10px;
  display: flex;
  flex-wrap: wrap;
  gap: 5px;
}

.hashtag {
  background: rgba(0, 0, 0, 0.7);
  color: white;
  padding: 4px 8px;
  border-radius: 4px;
  font-size: 12px;
  cursor: pointer;
  transition: background 0.2s;
}

.hashtag:hover {
  background: rgba(0, 0, 0, 0.9);
}

/* 布局尺寸 */
.grid-item-large {
  grid-column: span 4;
  grid-row: span 2;
}

.grid-item-medium {
  grid-column: span 2;
  grid-row: span 1;
}

.grid-item-small {
  grid-column: span 2;
  grid-row: span 1;
}

.loading {
  grid-column: 1 / -1;
  text-align: center;
  padding: 20px;
  color: #666;
}
</style>
