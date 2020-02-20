<template>
  <div
    class="vn-floating-label"
    :class="containerClasses"
    :style="inputContainerStyle"
  >
    <div
      v-if="settings.hasClearButton"
      class="icon clear__icon"
      @mousedown.stop.prevent="clear"
    >
      <svg
        width="10px"
        height="11px"
        viewBox="3 3 10 11"
        version="1.1"
        xmlns="http://www.w3.org/2000/svg"
        xmlns:xlink="http://www.w3.org/1999/xlink"
      >
        <defs></defs>
        <path
          id="Combined-Shape"
          d="M8,6.58578644 L5.17157288,3.75735931 L3.75735931,5.17157288 L6.58578644,
          8 L3.75735931,10.8284271 L5.17157288,12.2426407 L8,9.41421356 L10.8284271,12.2426407 L12.2426407,10.8284271 L9.41421356,8 L12.2426407,5.17157288 L10.8284271,3.75735931 L8,6.58578644 Z"
          stroke="none"
          fill="#FFFFFF"
          fill-rule="evenodd"
        ></path>
      </svg>
    </div>
    <div
      v-if="settings.line"
      class="accessibility__icon"
      :style="accessibilityStyle"
    ></div>
    <div
      ref="input-container"
      class="slot-container"
    >
      <input
        name="wrapper"
        type="text"
      >
    </div>
    <label
      class="label__placeholder"
      :for="labelName"
    >{{ config.label }}</label>
    <label
      class="label__active"
      :class="activeLabelClasses"
      :style="activeLabelStyle"
      :for="labelName"
    >{{ config.label }}</label>
  </div>
</template>

<script>
export default {
  name: 'VnFloatingLabel',
  props: {

    // eslint-disable-next-line vue/require-prop-types
    config: {
    
      required: true,
    },
  },
  data() {
    return {
      defaultSettings: {
        classes: {
          error: 'has-error',
        },
        hasError: false,
        height: 64,
        hasClearButton: true,
        line: true,
        scale: true,
        labelOffset: {
          top: 10,
          left: 8,
        },
        color: {
          focusColor: '#128CED',
          lineColor: '#128CED',
          errorColor: '#ff0000',
          blurredColor: 'rgba(3, 23, 40, 0.34)',
        },
      },
      value: '',
      hasFocus: false,
      hasContent: false,
    };
  },
  computed: {
    activeLabelClasses() {
      return {
        'label__active--canscale': this.settings.scale,
      };
    },
    hasClearButton() {
      // eslint-disable-next-line no-prototype-builtins
      if (this.config.hasOwnProperty('hasClearButton')) {
        return this.config.hasClearButton;
      }

      return false;
    },
    containerClasses() {
      const classes = {
        'has-line': this.settings.line,
        'vn-floating-label--focus': this.hasFocus,
        'vn-floating-label--content': this.hasContent,
      };

      if (this.settings.hasError) {
        classes[this.settings.classes.error] = true;
      }

      return classes;
    },
    hasError() {
      return this.value !== '';
    },
    labelName() {
      if (this.config.name !== undefined) {
        return this.config.name;
      }

      return this.config.label.toLowerCase();
    },
    accessibilityStyle() {
      let color = this.settings.color.lineColor;

      if (this.settings.hasError) {
        color = this.settings.color.errorColor;
      }

      return {
        'background-color': color,
      };
    },
    labelColor() {
      if (!this.settings.hasError) {
        return this.hasFocus ? this.settings.color.focusColor : this.settings.color.blurredColor;
      }
 
      return this.settings.color.errorColor;
    },
    activeLabelStyle() {
      return {
        top: `${this.settings.labelOffset.top}px`,
        left: `${this.settings.labelOffset.left}px`,
        color: this.labelColor,
      };
    },
    inputContainerStyle() {
      return {
        height: `${this.settings.height}px`,
      };
    },
    settings() {
      return Object.assign({}, this.defaultSettings, this.config);
    },
  },
  mounted() {
    this.formElement = this.$refs['input-container'].querySelector('input, select');

    if (this.formElement) {
      this.formElement.addEventListener('input', this.input);
      this.formElement.addEventListener('blur', this.blur);
      this.formElement.addEventListener('focus', this.focus);

      if (this.formElement.type === 'select-one') {
        this.hasContent = true;
        this.settings.scale = false;
        this.settings.hasClearButton = false;
      }
    }
  },
  methods: {
    clear() {
      this.formElement.value = '';
      this.hasContent = false;
      this.hasFocus = false;
      this.$emit('clear');
    },
    // eslint-disable-next-line no-unused-vars
    focus(event) {
      this.hasFocus = true;
      this.$emit('focus');
    },
    input(event) {
      this.hasFocus = true;
      this.hasContent = event.target.value !== '';
      this.$emit('input');
    },
    // eslint-disable-next-line no-unused-vars
    blur(event) {
      this.hasFocus = false;
      this.$emit('blur');
    },
  },
};
</script>
