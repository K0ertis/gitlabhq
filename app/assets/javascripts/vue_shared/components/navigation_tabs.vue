<script>
import $ from 'jquery';

/**
 * Given an array of tabs, renders non linked bootstrap tabs.
 * When a tab is clicked it will trigger an event and provide the clicked scope.
 *
 * This component is used in apps that handle the API call.
 * If you only need to change the URL this component should not be used.
 *
 * @example
 * <navigation-tabs
 *   :tabs="[
 *   {
 *      name: String,
 *      scope: String,
 *      count: Number || Undefined || Null,
 *      isActive: Boolean,
 *    },
 *   ]"
 *   @onChangeTab="onChangeTab"
 *   />
 */
export default {
  name: 'NavigationTabs',
  props: {
    tabs: {
      type: Array,
      required: true,
    },
    scope: {
      type: String,
      required: false,
      default: '',
    },
  },
  mounted() {
    $(document).trigger('init.scrolling-tabs');
  },
  methods: {
    shouldRenderBadge(count) {
      // 0 is valid in a badge, but evaluates to false, we need to check for undefined or null
      return !(count === undefined || count === null);
    },

    onTabClick(tab) {
      this.$emit('onChangeTab', tab.scope);
    },
  },
};
</script>
<template>
  <ul class="nav-links scrolling-tabs separator">
    <li
      v-for="(tab, i) in tabs"
      :key="i"
      :class="{
        active: tab.isActive,
      }"
    >
      <a
        :class="`js-${scope}-tab-${tab.scope}`"
        role="button"
        @click="onTabClick(tab)"
      >
        {{ tab.name }}

        <span
          v-if="shouldRenderBadge(tab.count)"
          class="badge badge-pill"
        >
          {{ tab.count }}
        </span>
      </a>
    </li>
  </ul>
</template>
