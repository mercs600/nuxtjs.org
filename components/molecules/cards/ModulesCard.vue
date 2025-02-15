<template>
  <div
    class="
      group
      relative
      hover:bg-gray-50 hover:dark:bg-sky-darkest
      dark:bg-sky-darker
      rounded-md
      shadow-md
      h-64
      lg:h-40
      p-4
      flex flex-col
      justify-between
    "
  >
    <Link :to="module.website" :aria-label="module.website" target="_blank" class="absolute inset-0" />
    <div
      class="
        transition-opacity
        duration-200
        ease-in-out
        opacity-0
        group-hover:opacity-100
        absolute
        top-4
        right-4
        cursor-pointer
      "
    >
      <nuxt-img alt="external_link" src="/img/icons/ext.svg" width="24" height="24" />
    </div>
    <div class="flex flex-col lg:flex-row space-y-4 lg:space-y-0 lg:space-x-3">
      <img loading="lazy" :src="iconUrl" :alt="module.name" class="w-10 h-10 mt-2 object-contain" />
      <div class="flex flex-col space-y-1">
        <span class="font-medium">{{ module.name }}</span>
        <div class="max-h-20 text-sm overflow-y-auto">
          {{ module.description }}
        </div>
      </div>
    </div>
    <div class="lg:flex lg:flex-row-reverse justify-between w-full z-0">
      <ul class="flex space-x-1 pb-4 lg:pb-0">
        <li
          v-for="maintainer in module.maintainers"
          :key="maintainer.name"
          v-tooltip="{
            content: maintainer.name,
            classes: [
              'bg-sky-darker dark:bg-white',
              'text-white dark:text-sky-darker',
              'px-2',
              'py-1',
              'rounded',
              'text-sm'
            ]
          }"
        >
          <Link :to="`https://github.com/${maintainer.github}`" target="blank">
            <img loading="lazy" :src="maintainer.avatar" :alt="maintainer.name" class="rounded-full w-6 h-6" />
          </Link>
        </li>
      </ul>
      <div class="flex items-center space-x-4 w-1/2 text-xs font-medium">
        <Link :to="module.github" class="group flex space-x-2 items-center" target="blank">
          <IconStar
            alt="Star icon"
            class="text-sky-dark group-hover:text-sky-darker dark:text-cloud-surface dark:group-hover:text-white"
          />
          <span class="truncate pt-0.5">{{ numberFormat(module.stars) }} star{{ module.stars !== 1 ? 's' : '' }}</span>
        </Link>
        <Link :to="npmUrl" class="group flex space-x-2 items-center" target="blank">
          <IconDownload
            alt="Download icon"
            class="
              w-4
              h-4
              text-sky-dark
              group-hover:text-sky-darker
              dark:text-cloud-surface dark:group-hover:text-white
            "
          />
          <span class="truncate pt-0.5"
            >{{ numberFormat(module.downloads) }} download{{ module.downloads !== 1 ? 's' : '' }}</span
          >
        </Link>
      </div>
    </div>
  </div>
</template>
<script lang="ts">
import { defineComponent } from '@nuxtjs/composition-api'
import millify from 'millify'

export default defineComponent({
  props: {
    module: {
      type: Object,
      required: true
    }
  },
  computed: {
    iconUrl() {
      const { icon, category } = this.module

      if (icon && icon.startsWith('https://')) {
        return icon
      }

      if (icon) {
        return `https://modules.nuxtjs.org/icons/${icon}`
      }

      return `/img/modules/categories/${(category || '').toLowerCase()}.svg`
    },

    npmUrl() {
      return `https://npmjs.com/package/${this.module.npm}`
    },

    githubUrl() {
      return `https://github.com/${this.module.github}`
    }
  },
  methods: {
    numberFormat(num: any, options = { precision: 1 }) {
      return millify(num || 0, options)
    }
  }
})
</script>
