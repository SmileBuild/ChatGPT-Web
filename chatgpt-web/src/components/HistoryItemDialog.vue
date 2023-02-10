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
  },
  methods: {
    clickOnce(val) {      
      this.isfirst = val;
    }
  }
}
</script>
<template>

  <el-dialog v-model="isShow" width="50%" :before-close="handleClose" align-center>
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

        <el-button type="danger" @click="$emit('delete')">
          Delete!
        </el-button>
        <!-- <el-button type="danger" plain @click="clickOnce(false)">
          Delete? {{ isfirst }}
        </el-button> -->
        <el-button @click="$emit('close')">Cancel</el-button>
      </span>
    </template>
  </el-dialog>
</template>

<style scoped>
.dialog-footer button:first-child {
  margin-right: 10px;
}
</style>