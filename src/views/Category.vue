<template>
  <div class="flex flex-col">
    <div class="post-header">
      <Breadcrumbs :current="t('menu.categories')" />
      <h1 class="post-title text-white uppercase">
        {{ t('menu.categories') }}
      </h1>
    </div>

    <div class="bg-ob-deep-800 px-14 py-16 rounded-2xl shadow-xl block">
      <CategoryList>
        <template v-if="categories && categories.length > 0">
          <CategoryItem
            v-for="category in categories"
            :key="category.slug"
            :name="category.name"
            :slug="category.slug"
            :count="category.count"
            size="xl"
          />
        </template>
        <template v-else-if="categories">
          <ob-skeleton tag="li" :count="10" height="20px" width="3rem" />
        </template>
        <template v-else>
          <div class="flex flex-row justify-center items-center">
            <svg-icon class="stroke-ob-bright mr-2" icon-class="warning" />
            {{ t('settings.empty-category') }}
          </div>
        </template>
      </CategoryList>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent, onBeforeMount, onUnmounted, computed } from 'vue'
import { Sidebar } from '@/components/Sidebar'
import Breadcrumbs from '@/components/Breadcrumbs.vue'
import { useI18n } from 'vue-i18n'
import { useCommonStore } from '@/stores/common'
import { useCategoryStore } from '@/stores/category'
import { CategoryList, CategoryItem } from '@/components/Category'
export default defineComponent({
  name: 'Category',
  components: { Sidebar, Breadcrumbs, CategoryList, CategoryItem },
  setup() {
    // const { t } = useI18n()
    // return { t }
    const commonStore = useCommonStore()
    const { t } = useI18n()
    const categoryStore = useCategoryStore()

    const fetchData = async () => {
      categoryStore.fetchCategories()
      commonStore.setHeaderImage(`${require('@/assets/default-cover.jpg')}`)
    }

    onBeforeMount(fetchData)
    onUnmounted(() => {
      commonStore.resetHeaderImage()
    })

    return {
      categories: computed(() => {
        console.log('categoryStore.categories', categoryStore.categories)

        if (categoryStore.isLoaded && categoryStore.categories.length === 0)
          return null
        return categoryStore.categories
      }),
      t
    }
  }
})
</script>

<style lang="scss" scoped></style>
