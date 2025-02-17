<script setup>
import AppLink from './Link.vue';
import { ref } from 'vue';
import { isExternal } from '../../../utils/validate';
import { getNormalPath } from '../../../utils/ruoyi';
const props = defineProps({
  /**
   * 路由对象
   */
  item: {
    type: Object,
    required: true
  },
  /**
   * 是否是嵌套路由
   */
  isNest: {
    type: Boolean,
    default: false
  },
  /**
   * 根路径
   */
  basePath: {
    type: String,
    default: ''
  }
})

/**
 * 只有一个子节点
 */
const onlyOneChild = ref({});

/**
 * 是否需要展示chilren
 */
const hasOneShowingChild = (children = [], parent) => {
  if (!children) {
    children = [];
  }
  const showingChildren = children.filter(item => {
    if (item.hidden) {
      return false
    } else {
      // Temp set(will be used if only has one showing child)
      onlyOneChild.value = item
      return true
    }
  })

  // When there is only one child router, the child router is displayed by default
  if (showingChildren.length === 1) {
    return true
  }

  // Show parent if there are no child router to display
  if (showingChildren.length === 0) {
    onlyOneChild.value = { ...parent, path: '', noShowingChildren: true }
    return true
  }

  return false
};

/**
 * 返回路径
 */
const resolvePath = (routePath, routeQuery) => {
  if (isExternal(routePath)) {
    return routePath
  }
  if (isExternal(props.basePath)) {
    return props.basePath
  }
  if (routeQuery) {
    let query = JSON.parse(routeQuery);
    return { path: getNormalPath(props.basePath + '/' + routePath), query: query }
  }
  return getNormalPath(props.basePath + '/' + routePath)
}

</script>

<template>
  <div v-if="!props.item.hidden">
    <template
      v-if="hasOneShowingChild(props.item.children, props.item) && (!onlyOneChild.children || onlyOneChild.noShowingChildren) && !props.item.alwaysShow"
    >
      <app-link v-if="onlyOneChild.meta" :to="resolvePath(onlyOneChild.path, onlyOneChild.query)">
        <el-menu-item
          :index="resolvePath(onlyOneChild.path)"
          :class="{ 'submenu-title-noDropdown': !props.isNest }"
        >
          <svg-icon
            :icon-class="onlyOneChild.meta.icon || (props.item.meta && props.item.meta.icon)"
          />
          <template #title>{{ onlyOneChild.meta.title }}</template>
        </el-menu-item>
      </app-link>
    </template>

    <el-sub-menu v-else ref="subMenu" :index="resolvePath(item.path)" popper-append-to-body>
      <template v-if="props.item.meta" #title>
        <svg-icon :icon-class="props.item.meta && props.item.meta.icon" />
        <span>{{ props.item.meta.title }}</span>
      </template>

      <sidebar-item
        v-for="child in props.item.children"
        :key="child.path"
        :is-nest="true"
        :item="child"
        :base-path="resolvePath(child.path)"
        class="nest-menu"
      ></sidebar-item>
    </el-sub-menu>
  </div>
</template>

<style lang="scss" scoped>
</style>