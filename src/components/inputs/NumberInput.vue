<template>
  <div
    contenteditable
    @input="formatInput"
    :style="{ minWidth: '72px', width: `${inputLength}px` }"
    class="editable-input"
  ></div>
</template>

<script>
export default {
  data() {
    return {
      inputLength: 72,
    };
  },
  methods: {
    formatInput(event) {
      const initialPosition = window.getSelection().anchorOffset;
      let value = event.target.innerText.replace(/[^0-9]/g, '');
      const formattedLength = value.length;
      
      value = value.replace(/\B(?=(\d{3})+(?!\d))/g, " ");
      
      event.target.innerText = value;

      this.inputLength = event.target.scrollWidth;
      
      const range = document.createRange();
      const selection = window.getSelection();
      range.selectNodeContents(event.target);
      const newPosition = this.calculateNewCursorPosition(initialPosition, formattedLength, value);
      range.setStart(event.target.childNodes[0], newPosition);
      range.collapse(true);
      selection.removeAllRanges();
      selection.addRange(range);
    },
    calculateNewCursorPosition(initialPosition, formattedLength, formattedValue) {
      return formattedValue.length;
    }
  }
}
</script>

<style scoped>
.editable-input {
  display: inline-block;
  overflow: hidden;
  white-space: nowrap;
  padding: 0 8px;
  border-bottom: 1px solid #ccc;
}
</style>
