<template>
  <div class="vn-collapsible-panel flex-grid">
    <div class="flex-row">
      <div
        :class="{ 'flex-col-12': !hasSectionDescription, 'remove-cursor': !toggle }"
        class="section-header header-name-container"
        @click="togglePanel"
        @keydown.enter.space.prevent="togglePanel"
      >
        <button
          v-if="caret && toggle"
          :aria-hidden="!toggle"
          :aria-expanded="expanded+''"
          :aria-controls="sectionId"
          :aria-label="ariaLabel"
          :class="{ 'header-button-normal': !toggle, 'header-button-toggle': toggle }"
          class="header-button"
        >
          <span
            :class="{ 'expanded': expanded }"
            class="caret"
            aria-hidden="true"
          ></span>
        </button>
        <slot
          :content="content"
          :content-expanded="expanded"
          :content-aria-controls="sectionId"
          name="title"
        ></slot>
      </div>
      <div
        v-if="hasSectionDescription"
        class="header-description-container wrap"
      >
        <slot
          :content="content"
          name="description"
        ></slot>
      </div>
    </div> <!-- /sectionHeader -->
    <div
      v-show="expanded"
      :id="sectionId"
      class="section-body"
    >
      <slot
        :content="content"
        name="body"
      ></slot>
    </div> <!-- /sectionBody -->
  </div>
</template>

<script>
import { default as _has } from 'lodash/has';
import { default as _get } from 'lodash/get';
import { generateUniqueId } from 'src/lib/helpers/dom.helper';

export default {
  name: 'VnClosablePanel',
  props: {
    /**
     * Content to show in the collapsible panel
     */
    content: {
      type: Object,
      required: true,
    },
    /**
     * Whether to show the caret, default to show it
     */
    caret: {
      type: Boolean,
      default: true,
    },
    /**
     * Panel state, if true, panel is collapsed; if false, panel is expanded
     */
    collapsed: {
      type: Boolean,
      default: true,
    },
    /**
     * Turn on or off the expand/collapse functionality, default to on
     */
    toggle: {
      type: Boolean,
      default: true,
    },
    /**
     * Whether to be expanded when component is first rendered
     */
    initiallyExpanded: {
      type: Boolean,
      default: false,
    },
  },
  data() {
    return {
      expanded: this.initiallyExpanded,
    };
  },
  computed: {
    hasSectionDescription() {
      return _has(this.content, 'section.description');
    },
    sectionId() {
      return generateUniqueId('section-body');
    },
    ariaLabel() {
      return _get(this, 'content.section.name', this.content.title);
    },
  },
  watch: {
    collapsed(isCollapsed) {
      if (!this.toggle) {
        return; // no-op
      }

      this.expanded = !isCollapsed;
    },
  },
  mounted() {
    this.expanded = this.initiallyExpanded || !this.collapsed;
  },
  methods: {
    /**
     * Toggle between expand and collapse state
     */
    togglePanel() {
      if (!this.toggle) {
        return; // no-op
      }

      this.expanded = !this.expanded;
      this.$emit('on-toggle-expanded', this.expanded);
    },
  },

};
</script>


