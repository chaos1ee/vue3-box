<template>
  <nav :style="{ backgroundColor }">
    <el-menu
      class="el-menu-vertical"
      :collapse="isCollapse"
      :unique-opened="true"
      :background-color="backgroundColor"
      :text-color="textColor"
      :active-text-color="activeTextColor"
      :router="true"
      :default-active="activePath"
      :collapse-transition="true"
    >
      <kg-nav-items :routes="routes"></kg-nav-items>
    </el-menu>
  </nav>
</template>

<script lang="ts">
import { defineComponent, computed } from 'vue'
import { useRoute, useRouter } from 'vue-router'
import { useStore } from 'vuex'
import KgNavItems from './nav-items.vue'

export default defineComponent({
  name: 'KgMenu',
  components: { KgNavItems },
  props: {
    textColor: {
      type: String,
      default: '#111827',
    },
    backgroundColor: {
      type: String,
      default: '#ffffff',
    },
    activeTextColor: {
      type: String,
      default: '#FF5A00',
    },
  },
  setup() {
    const store = useStore()
    const route = useRoute()
    const router = useRouter()

    const routes = router.options.routes.filter(
      route => route.path !== '/' && !route.path.startsWith('/:patchMatch'),
    )

    return {
      routes,
      activePath: computed(() => route.path),
      isCollapse: computed(() => store.state.nav.isCollapse),
    }
  },
})
</script>

<style lang="scss" scoped>
::v-deep(.el-menu) {
  border-right: none;
}

.el-menu-vertical:not(.el-menu--collapse) {
  width: 256px;
}
</style>
