<template>



  <el-row :gutter="40">
    <el-col :span="8" class="col-params" v-loading="loading" element-loading-text="Loading..."
      :element-loading-spinner="svg" element-loading-svg-view-box="-10, -10, 50, 50"
      element-loading-background="rgba(122, 122, 122, 0.8)">
      
      <Edit style="width: 1em; height: 1em; margin-right: 8px" />
      <el-button type="primary" :icon="Search" />
      <el-button size="large" type="success" icon="Search" @click="submitForm">
        
        确定
      </el-button>
      <h2>Params 参数配置</h2>
      <el-divider />

      <el-form>
        <h4>API Key</h4>
        <el-input v-model="api_key" placeholder="API秘钥" show-password type="password"></el-input>
        <h4>Prompt</h4>
        <el-input type="textarea" v-model="prompt" placeholder="输入内容" rows="10"></el-input>

        <h4>Max_tokens</h4>

        <el-input-number type="number" v-model="max_tokens"></el-input-number>
        <h4>Temperature</h4>
        <div class="slider-block">
          <span class="slider-laber">Temperature</span>
          <el-slider v-model="temperature" :step="0.1" :min="0" :max="1" show-input />
        </div>
        <h4>Top_p</h4>
        <div class="slider-block">
          <span class="slider-laber">Top_p</span>
          <el-slider v-model="top_p" :step="0.1" :min="0" :max="1" show-input />
        </div>
        <h4>Frequency_penalty</h4>
        <div class="slider-block">
          <span class="slider-laber">Frequency_penalty</span>
          <el-slider v-model="frequency_penalty" :step="0.1" :min="-2" :max="2" show-input />
        </div>
        <h4>Presence_penalty</h4>
        <div class="slider-block">
          <span class="slider-laber">Presence_penalty</span>
          <el-slider v-model="presence_penalty" :step="0.1" :min="-2" :max="2" show-input />
        </div>
        <h4>Model</h4>
        <el-select v-model="model" class="m-2" placeholder="Model" size="large">
          <el-option v-for="item in models" :key="item.value" :label="item.label" :value="item.value">{{
            item
          }}</el-option>
        </el-select>
      </el-form>
      <el-divider />
      <el-link href="https://beta.openai.com/docs/api-reference/completions/create#completions/create-model"
        target="_blank" type="danger">param info (参数说明)</el-link>



    </el-col>
    <el-col :span="16">
      <ul v-if="display_history">
        <li v-for="(item, index) in history_list" :key="index">{{ item.prompt }}</li>
      </ul>
      <el-button size="large" type="warning" @click="downloadTxt" :disabled="download_disable">save 保存</el-button>
      <el-button size="large" type="warning" @click="toggleHistory"> {{ display_history? "Hide": "Show" }}
      </el-button>
      <h2>Repsonse 结果</h2>
      <el-divider />


      <p id="result" style="color:red;white-space: pre-wrap;">{{ response }}</p>
    </el-col>

  </el-row>



</template>

<!-- https://beta.openai.com/docs/api-reference/completions/create -->
<script>
import axios from 'axios'
import { Search } from '@element-plus/icons-vue'
export default {
  name: 'Editor',
  data() {
    return {
      loading: false,
      api_key: '',
      prompt: '',
      temperature: 1,
      top_p: 1,
      max_tokens: 2048,
      frequency_penalty: 0,
      presence_penalty: 0,
      stop: ["Human:", "AI:"],
      model: 'text-davinci-003',
      models: ['text-davinci-003', 'text-davinci-002', 'text-curie-001'],
      response: '',
      download_disable: true,
      display_history: false,
      history_list: []
    }
  },
  mounted() {
    var temp_key = localStorage.getItem('api-key')
    if (temp_key != null)
      if (temp_key != '') {
        this.api_key = localStorage.getItem('api-key')
      }
    this.getHistory()
  },
  watch: {
    api_key: {
      handler(val) {
        localStorage.setItem('api-key', val)
      },
      deep: true
    },

    response(resp) {
      if (resp == '') {
        this.download_disable = true;
      } else {
        this.download_disable = false;

      }
    }
  },
  methods: {
    toggleHistory() {
      this.display_history = !this.display_history;
    },
    getHistory() {

      if (localStorage.getItem('history-list')) {
        this.history_list = JSON.parse(localStorage.getItem('history-list'));
      }
    },
    saveToHistory() {
      let historyArr = [];
      if (localStorage.getItem('history-list')) {
        historyArr = JSON.parse(localStorage.getItem('history-list'));
      }
      historyArr.unshift({
        prompt: this.prompt,
        response: this.response,
        time: new Date().getTime()
      });
      localStorage.setItem('history-list', JSON.stringify(historyArr));
    },
    downloadTxt() {
      let text = 'Prompt: ' + this.prompt + '\n' + document.getElementById('result').innerText;
      let element = document.createElement("a");
      let file = new Blob([text], { type: 'text/plain' });
      element.href = URL.createObjectURL(file);
      element.download = "download.txt";
      element.click();
    },
    submitForm() {
      let data = {
        prompt: this.prompt,
        temperature: this.temperature,
        top_p: this.top_p,
        model: this.model,
        max_tokens: this.max_tokens,
        frequency_penalty: this.frequency_penalty,
        presence_penalty: this.presence_penalty,
        stop: this.stop
      }
      this.loading = true
      axios.post('https://api.openai.com/v1/completions', data, {
        headers: {
          'Content-Type': 'application/json',
          'Authorization': `Bearer ` + this.api_key,
        }
      })
        .then(response => {
          this.loading = false
          this.response = response.data.choices[0].text;
          console.log(this.response)
          this.saveToHistory()
        })
        .catch(error => {
          this.loading = false
          console.log(error);
        });
    }
  }
}
</script>

<style scoped>
.el-header {
  background: var(--el-color-black);
  color: aliceblue;
}

.slider-block {
  display: flex;
  align-items: center;
}

.slider-block .el-slider {
  margin-top: 0;
  margin-left: 12px;
}

.slider-block .slider-laber {
  font-size: 14px;
  color: var(--el-text-color-secondary);
  line-height: 44px;
  flex: 1;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
  margin-bottom: 0;
}

.slider-block .slider-laber+.el-slider {
  flex: 0 0 70%;
}
</style>