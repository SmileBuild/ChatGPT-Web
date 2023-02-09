<script>

export default {
  props: {
    show: Boolean,
    nowitem: Object
  },
  data() {
    return {
      isShow: false,
      prompt: '',
      response: '',

    }
  },
  watch: {
    show(val) {
      this.isShow = val
    },
    nowitem(val) {
      this.prompt = val.prompt
      this.response = val.response
    }
  },
  mounted() {
    console.log(`the component Dialog is now mounted.`);
  }
}
</script>
<template>

  <el-dialog v-model="isShow" width="50%" align-center>
    <template #header>
      <div class="my-header">
        <h4>Prompt:{{ prompt }}</h4>
      </div>
    </template>
    <el-scrollbar>
      <h4>Response</h4>

      <p style="white-space: pre-wrap;">{{ response }}</p>
    </el-scrollbar>
    <template #footer>
      <span class="dialog-footer">
        <el-button @click="$emit('delete')">
          Del
        </el-button>
        <el-button @click="$emit('close')">Cancel</el-button>
      </span>
    </template>
  </el-dialog>
</template>

<!-- <template>
  <Transition name="modal">
    <div v-if="show" class="modal-mask">
      <div class="modal-container">
        <div class="modal-header">
          <slot name="header">{{nowitem.prompt}}</slot>
        </div>

        <div class="modal-body">
          <slot name="body">{{nowitem.request}}</slot>
        </div>

        <div class="modal-footer">
          <slot name="footer">
            default footer
            <button
              class="modal-default-button"
              @click="$emit('close')"
            >OK</button>
          </slot>
        </div>
      </div>
    </div>
  </Transition>
</template> -->

<style scoped>
.dialog-footer button:first-child {
  margin-right: 10px;
}
</style>