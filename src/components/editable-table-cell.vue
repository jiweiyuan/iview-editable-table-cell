<template>
  <div @dblclick="onFieldClick" class="edit-cell">
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
               v-on="listeners"
               v-bind="$attrs"
               v-model="model">
      <slot name="editable-component-slot"></slot>
    </component>
  </div>
</template>
<script>
export default {
  name: "editable-cell",
  inheritAttrs: false,
  props: {
    value: {
      type: [String, Number, Boolean],
      default: ""
    },
    meta: {
      type: Object,
      default: () => {}
    },
    toolTipContent: {
      type: String,
      default: "双击编辑"
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
      default: "on-blur"
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
        console.log('refs', this.$refs)
        let inputRef = this.$refs.input;
        if (inputRef && typeof inputRef.focus === 'function') {
          inputRef.focus();
        }
      });
    },
    onInputExit(val) {
      if (val instanceof FocusEvent)  {
        val = this.model
      }
      this.editMode = false;
      this.$emit('on-save', val, this.meta)
    }
  }
};
</script>
<style scoped>
.edit-cell {
  cursor: pointer;
}
</style>

