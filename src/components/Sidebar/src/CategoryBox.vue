<template>
  <div class="sidebar-box">
    <SubTitle :title="'titles.category_list'" icon="category" />
    <ul class="flex justify-event flex-wrap gap-2 pt-2 cursor-pointer">
      <template v-if="categories.length > 0">
        <CategoryItem
          v-for="category in categories"
          :key="category.slug"
          :name="category.name"
          :slug="category.slug"
          :count="category.count"
          size="xs"
        />
      </template>
      <template v-else>
        <ob-skeleton tag="li" :count="10" height="20px" width="3rem" />
      </template>
    </ul>
  </div>
</template>

<script lang="ts">
import { computed, defineComponent, onMounted, ref } from 'vue'
import { SubTitle } from '@/components/Title'
import { useCategoryStore } from '@/stores/category'
import { useAppStore } from '@/stores/app'
import { Categories } from '@/models/Post.class'
import { CategoryItem } from '@/components/Category'
export default defineComponent({
  name: 'ObArticleBox',
  components: { SubTitle, CategoryItem },
  setup() {
    const appStore = useAppStore()
    const categoryStore = useCategoryStore()
    const loading = ref(true)

    const fetchData = async () => {
      await categoryStore.fetchCategories()
      loading.value = false
    }

    onMounted(fetchData)
    const test_alert = (v: Categories) => {
      console.log(v)
    }
    return {
      loading,
      categories: computed(() => categoryStore.categories),
      gradientBackground: computed(() => {
        return { background: appStore.themeConfig.theme.header_gradient_css }
      }),
      test_alert
    }
  }
})
</script>
