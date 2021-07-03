<template>
  <div @click="onFieldClick" class="edit-cell">
    <Tooltip v-if="!editMode && !showInput"
                :placement="toolTipPlacement"
                :content="toolTipContent">
      <div tabindex="0" @keyup.enter="onFieldClick">
        <slot name="content"></slot>
      </div>

    </Tooltip>
    <component :is="editableComponent"
               v-if="editMode || showInput"
               ref="input"
               @on-focus="onFieldClick"
               @on-blur="onInputExit"
               v-on="listeners"
               v-bind="$attrs"
               v-model="model">
      <slot name="edit-component-slot"></slot>
    </component>
  </div>
</template>
<script>
export default {
  name: "editable-cell",
  inheritAttrs: false,
  props: {
    value: {
      type: [String, Number],
      default: ""
    },
    meta: {
      type: Object,
      default: () => {}
    },
    toolTipContent: {
      type: String,
      default: "点击编辑"
    },
    toolTipPlacement: {
      type: String,
      default: "top-start"
    },
    showInput: {
      type: Boolean,
      default: false
    },
    editableComponent: {
      type: String,
      default: "Input"
    },
    closeEvent: {
      type: String,
      default: "blur"
    }
  },
  data() {
    return {
      editMode: false
    };
  },
  computed: {
    model: {
      get() {
        return typeof this.value === "number"
            ? String(this.value)
            : this.value;
      },
      set(val) {
        this.$emit("input", val);
      }
    },
    listeners() {
      return {
        [this.closeEvent]: this.onInputExit,
        ...this.$listeners
      };
    }
  },
  methods: {
    onFieldClick() {
      this.editMode = true;
      this.$nextTick(() => {
        let inputRef = this.$refs.input;
        if (inputRef) {
          inputRef.focus();
        }
      });
    },
    onInputExit() {
      this.editMode = false;
      this.$emit('on-save', this.value, this.meta)
    }
  }
};
</script>
<style scoped>
.edit-cell {
  cursor: pointer;
}
</style>

