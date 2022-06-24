<template>
  <div
    class="
      flex flex-row
      items-center
      hover:opacity-50
      mr-2
      mb-2
      cursor-pointer
      transition-all
    "
  >
    <router-link
      class="
        bg-ob-deep-900
        text-center
        px-3
        py-1
        rounded-tl-md rounded-bl-md
        text-sm
      "
      :to="{ name: 'categories-search', query: { slug: slug } }"
      :style="stylingCategory()"
    >
      <em class="opacity-50">#</em>
      {{ name }}
    </router-link>
    <span
      class="
        bg-ob-deep-900
        text-ob-secondary text-center
        px-2
        py-1
        rounded-tr-md rounded-br-md
        text-sm
        opacity-70
      "
      :style="stylingCategory()"
    >
      {{ count }}
    </span>
  </div>
</template>

<script lang="ts">
import { defineComponent, toRefs } from 'vue'

export default defineComponent({
  name: 'ObCategoryItem',
  props: {
    name: String,
    slug: String,
    count: {
      type: Number,
      default: 0
    },
    size: {
      type: String,
      default: 'base'
    }
  },
  setup(props) {
    const categorySize = toRefs(props).size

    const stylingCategory = () => {
      if (categorySize.value === 'xs') {
        return {
          fontSize: '0.75rem',
          lineHeight: '1rem'
        }
      }

      if (categorySize.value === 'sm') {
        return {
          fontSize: '0.875rem',
          lineHeight: '1.25rem'
        }
      }

      if (categorySize.value === 'lg') {
        return {
          fontSize: '1.125rem',
          lineHeight: '1.75rem'
        }
      }

      if (categorySize.value === 'xl') {
        return {
          fontSize: '1.25rem',
          lineHeight: '1.75rem'
        }
      }

      if (categorySize.value === '2xl') {
        return {
          fontSize: '1.5rem',
          lineHeight: '2rem'
        }
      }

      return {
        fontSize: '1rem',
        lineHeight: '1.5rem'
      }
    }

    return { stylingCategory }
  }
})
</script>
