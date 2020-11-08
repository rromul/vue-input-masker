<template>
  <div ref="ph" class="live-placeholder">
    <slot></slot>
  </div>
</template>

<script>
import VMasker from "vanilla-masker";

export default {
  props: {
    pattern: { type: String, default: '' },
    placeholder: { type: String, default: '' },
    showPlaceholder: { type: Boolean, default: false },
    fixDefault: { type: Boolean, default: true },
    maskDigit: { type: String, default: '#' },
    maskAlpha: { type: String, default: '_' },
  },
  data() {
    return {
      defaultValue: '',
      ctx2d: null,
      font: '',
    };
  },
  computed: {
    placeholderEl() {
      return this.$refs.ph;
    },
    inputEl() {
      return this.placeholderEl.firstElementChild;
    },
  },
  mounted() {
    this.ctx2d = document.createElement('canvas').getContext('2d')
    this.defaultValue = this.inputEl.value
    const {fontSize, fontFamily} = this.getFontParams()
    this.placeholderEl.style.setProperty('--live-ph-font-size', fontSize)
    this.placeholderEl.style.setProperty('--live-ph-font-family', fontFamily)
    this.font = `${fontSize} ${fontFamily}`;
    this.ctx2d.font = this.font;
    this.inputEl.addEventListener("keyup", this.alignPlaceholderText);
    this.inputEl.addEventListener("paste", this.onPaste);

    if (this.showPlaceholder) {
      this.inputEl.placeholder = this.getPlaceholder()
    }
    this.inputEl.maxLength = this.pattern.length
    this.alignPlaceholderText();
  },
  beforeDestroy() {
    this.inputEl.removeEventListener("keyup", this.alignPlaceholderText);
    this.inputEl.removeEventListener("paste", this.onPaste);
  },
  methods: {
    /*eslint no-debugger: "warn"*/
    alignPlaceholderText(evt) {
      this.changeValue(evt);
      const charCount = this.inputEl.value.length;
      this.placeholderEl.dataset.placeholderContent = this.inputEl.placeholder.substr(charCount);
      this.placeholderEl.style.setProperty(
        "--input-mask-offset",
        this.getTextWidth(this.inputEl.value) + "px"
      );
    },
    onPaste(evt) {
      this.inputEl.value = VMasker.toPattern(evt.clipboardData.getData('text/plain'));
    },
    getTextWidth(txt) {
      return this.ctx2d.measureText(txt).width  + 0.5 * txt.length + 2;
    },
    changeValue(evt) {
      this.inputEl.value = VMasker.toPattern(this.inputEl.value, this.pattern);
      const nextChar = this.pattern.substr(this.inputEl.value.length, 1)
      if (nextChar !== '9' && nextChar !== 'A' && !this.isBackspace(evt)) {
        this.inputEl.value += nextChar;
      }
      if (this.fixDefault && !this.inputEl.value.startsWith(this.defaultValue)) {
        this.inputEl.value = this.defaultValue;
      }
    },
    getPlaceholder() {
      if (this.inputEl.placeholder) {
        return this.inputEl.placeholder
      }
      return this.pattern
        .replace(/[a-zа-я]/gi, this.maskAlpha)
        .replace(/\d/g, this.maskDigit)
    },
    getFontParams () {
      if ('computedStyleMap' in this.inputEl) {
        const styleMap = this.inputEl.computedStyleMap()
        const fs = styleMap?.getAll('font-size')[0] ?? 14
        const ff = styleMap?.getAll('font-family')[0] ?? 'Sans serif'
        return {
          fontSize: Math.round(fs.value) + fs.unit,
          fontFamily: ff.toString(),
        }
      }
      // Safari, IE, FireFox fix
      const style = window.getComputedStyle(this.inputEl)
      return {
        fontSize: style.fontSize,
        fontFamily: style.fontFamily,
      }
    },
    isBackspace(evt) {
      return !!evt && ((evt.which || evt.keyCode) === 8 || (evt.code ||evt.key) === 'Backspace');
    },
  },
};
</script>

<style scoped lang="scss">
$height: 40px;
$width: 200px;
$border-width: 2px;

.live-placeholder {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
  position: relative;
  width: $width - $border-width;
  border: none;
  &::after {
    content: attr(data-placeholder-content);
    position: absolute;
    display: inline-block;
    top: $border-width;
    left: $border-width;
    pointer-events: none;
    vertical-align: middle;
    line-height: $height - 2*$border-width;
    opacity: 0.4;
    transform: translateX(var(--input-mask-offset, 0));
    font-size: var(--live-ph-font-size);
    font-family: var(--live-ph-font-family);
  }
  input[type="text"] {
    box-sizing: border-box;
    border-width: $border-width;
    height: $height;
    width: 100%;
    &::placeholder {
      color: transparent;
    }
  }
}
</style>
