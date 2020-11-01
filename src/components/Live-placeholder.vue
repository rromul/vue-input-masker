<template>
  <div ref="ph" class="live-placeholder">
    <slot ref="inp"></slot>
  </div>
</template>

<script>
export default {
  mounted() {
    const ph = this.$refs.ph;
    const inp = ph.firstElementChild;
    const fs = inp.computedStyleMap().getAll("font-size")[0];
    const ff = inp.computedStyleMap().getAll("font-family")[0];
    const fontSize = Math.round(fs.value) + fs.unit;
    const fontFamily = ff.toString();
    ph.style.setProperty("--live-ph-font-size", fontSize);
    ph.style.setProperty("--live-ph-font-family", fontFamily);
    this.font = `${fontSize} ${fontFamily}`;
    this.ctx2d.font = this.font;
    inp.addEventListener("keyup", this.alignPlaceholderText);
    this.alignPlaceholderText();
  },
  beforeDestroy() {
    const ph = this.$refs.ph;
    const inp = ph.firstElementChild;
    inp.removeEventListener("keyup", this.alignPlaceholderText);
  },
  data() {
    return {
      ctx2d: document.createElement("canvas").getContext("2d"),
      font: "",
    };
  },
  methods: {
    alignPlaceholderText() {
      const ph = this.$refs.ph;
      const inp = ph.firstElementChild;
      const charCount = inp.value.length;
      ph.dataset.placeholderContent = inp.placeholder.substr(charCount);
      ph.style.setProperty(
        "--input-mask-offset",
        (this.getTextWidth(inp.value) + 0.5 * charCount + 2) + "px"
      );
    },
    getTextWidth(txt) {
      return this.ctx2d.measureText(txt).width;
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
    line-height: $height - 2 * $border-width;
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
