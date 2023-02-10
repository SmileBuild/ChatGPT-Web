<template>
  <el-row class="input-row" justify="center">
    <el-col :span="8">
      <el-form label-width="auto" size="large">
        <el-form-item label="ApiKey">
          <el-input v-model="api_key" placeholder="add key here. 在这里输入秘钥" show-password type="password"></el-input>
        </el-form-item>
        <el-form-item label="Prompt">
          <el-input type="textarea" v-model="prompt" placeholder="Input text here. 在这里输入内容" rows="3"></el-input>
        </el-form-item>
      </el-form>
    </el-col>
  </el-row>

  <el-row class="button-row" justify="center">
    <el-tooltip class="box-item" effect="dark" content="参数设置" placement="bottom">
      <el-button size="large" type="primary" icon="Operation" @click="toggleParamsBtn" plain>
        Params</el-button>
    </el-tooltip>
    <el-tooltip class="box-item" effect="dark" content="运行" placement="bottom">
      <el-button size="large" type="success" icon="Select" @click="submitForm">
        Run</el-button>
    </el-tooltip>

    <el-tooltip class="box-item" effect="dark" content="历史记录" placement="bottom">
      <el-button size="large" type="warning" @click="toggleHistory" icon="List" plain> History
      </el-button>
    </el-tooltip>
  </el-row>

  <el-row class="widget-row" justify="center">
    <el-col :span="4" class="params-widget">
      <Transition name="el-zoom-in-right">
        <el-card shadow="hover" v-show="display_params" style="background-color:var(--el-fill-color)"
          :body-style="{ height: '24rem' }">
          <template #header>
            <div class="card-header">
              <span class="card-header-text">Params 参数配置</span>
              <el-tooltip class="box-item" effect="dark" content="param info (参数说明)" placement="top">
                <el-button size="large" type="primary" @click="jumpDocs" icon="MagicStick" circle></el-button>
              </el-tooltip>
            </div>

          </template>

          <el-scrollbar>
            <el-form>
              <h4>Max_tokens</h4>

              <el-input-number type="number" v-model="max_tokens"></el-input-number>
              <h4>Temperature(0~1)</h4>
              <div class="slider-block">

                <el-slider v-model="temperature" :step="0.1" :min="0" :max="1" show-input />
              </div>
              <h4>Top_p(0~1)</h4>
              <div class="slider-block">

                <el-slider v-model="top_p" :step="0.1" :min="0" :max="1" show-input />
              </div>
              <h4>Frequency_penalty(-2~2)</h4>
              <div class="slider-block">

                <el-slider v-model="frequency_penalty" :step="0.1" :min="-2" :max="2" show-input />
              </div>
              <h4>Presence_penalty(-2~2)</h4>
              <div class="slider-block">

                <el-slider v-model="presence_penalty" :step="0.1" :min="-2" :max="2" show-input />
              </div>
              <h4>Model</h4>
              <el-select v-model="model" class="m-2" placeholder="Model" size="large">
                <el-option v-for="item in models" :key="item" :label="item" :value="item">{{
                  item
                }}</el-option>
              </el-select>
            </el-form>
          </el-scrollbar>
        </el-card>
      </Transition>
    </el-col>
    <el-col :span="8" class="result-widget">
      <el-card shadow="never" :body-style="{ height: '24rem' }">
        <template #header>
          <div class="card-header">
            <span class="card-header-text">Result 结果</span>
            <el-tooltip class="box-item" effect="dark" content="Download-下载" placement="top">
              <el-button size="large" type="danger" @click="downloadTxt" icon="Download" :disabled="download_disable"
                circle></el-button>

            </el-tooltip>

          </div>
        </template>
        <el-scrollbar>
          <p id="result" style="font-weight: bolder;white-space: pre-wrap;">{{ response }}</p>
        </el-scrollbar>
      </el-card>
    </el-col>

    <el-col :span="4" class="history-widget">
      <Transition name="el-zoom-in-top">
        <el-card v-show="display_history" style="background-color:var(--el-fill-color)" shadow="hover"
          :body-style="{ height: '24rem' }">
          <template #header>
            <div class="card-header">
              <span class="card-header-text">History 历史记录</span>

              <el-tooltip class="box-item" effect="dark" content="delete(刪除所有）" placement="top">
                <el-popconfirm width="220" confirm-button-text="OK" cancel-button-text="No" :icon="WarnTriangleFilled"
                  icon-color="#626AEF" title="Are you sure to delete all?删除所有记录？" @confirm="deleteAll">
                  <template #reference>
                    <el-button size="large" type="warning" class="button" icon="Delete" circle></el-button>
                  </template>
                </el-popconfirm>

              </el-tooltip>
            </div>
          </template>
          <el-scrollbar>
            <ul>
              <li v-for="(item, index) in history_list" :key="index">
                <el-link type="warning" @click="showHistoryItem = true, currentItem = item">{{ item.prompt }}</el-link>
              </li>

            </ul>
          </el-scrollbar>
        </el-card>
      </Transition>
    </el-col>

  </el-row>
  <el-row justify="center">
    <p class="footer-top">{{ pkgjson.name }} {{ pkgjson.version }}</p>
  </el-row>
  <el-row justify="center">
    <el-link class="footer-text" icon="Link" href="https://space.bilibili.com/34147682" target="_blank">created by Smile</el-link>
  </el-row>
  <el-row justify="center">
    <p class="footer-text">免责声明：本页面开发目的仅用于学习和探索</p>
  </el-row>
  <el-row justify="center">
    <p class="footer-text">
      通过使用\修改本页面内容随之而来的一切风险与本作者无关,请合理合法合规使用</p>
  </el-row>
  <Teleport to="body">

    <HistoryItemDialog :show="showHistoryItem" @close="showHistoryItem = false" :nowitem="currentItem"
      @delete="deleteItem(currentItem)">

    </HistoryItemDialog>
  </Teleport>
  <!-- 
  <el-dialog v-model="centerDialogVisible" title="{{currentItem.prompt}}" width="30%" align-center>
    <el-scrollbar>
      <span>{{ currentItem.response }}</span>
    </el-scrollbar>
    <template #footer>
      <span class="dialog-footer">
        <el-button @click="centerDialogVisible = false">Cancel</el-button>
        <el-button type="primary" @click="centerDialogVisible = false">
          Confirm
        </el-button>
      </span>
    </template>
  </el-dialog> -->


</template>

<!-- https://beta.openai.com/docs/api-reference/completions/create -->
<script>
import packageJSON from "../../package.json";
import axios from 'axios'
import HistoryItemDialog from './HistoryItemDialog.vue'
export default {
  name: 'Editor',
  components: {
    HistoryItemDialog
  },
  emits: ['bigLoading'],
  data() {
    return {
      pkgjson: packageJSON,    
      currentItem: {},
      showHistoryItem: false,
      api_key: '',
      prompt: '',
      temperature: 1,
      top_p: 1,
      max_tokens: 100,
      frequency_penalty: 0,
      presence_penalty: 0,
      stop: ["Human:", "AI:"],
      model: 'text-davinci-003',
      models: ['text-davinci-003', 'text-davinci-002', 'text-curie-001'],
      response: '',

      download_disable: true,
      display_params: false,
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
    jumpDocs() {
      window.location.href = "https://platform.openai.com/docs/api-reference/completions/create#completions/create-model";

    },
    toggleParamsBtn() {
      this.display_params = !this.display_params;
    },
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
      console.log(historyArr)
      localStorage.setItem('history-list', JSON.stringify(historyArr));
      this.getHistory();
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
      // this.loading = true
      this.$emit('bigLoading', true)

      axios.post('https://api.openai.com/v1/completions', data, {
        headers: {
          'Content-Type': 'application/json',
          'Authorization': `Bearer ` + this.api_key,
        }
      })
        .then(response => {
          // this.loading = false
          this.$emit('bigLoading', false)
          this.response = response.data.choices[0].text;
          console.log(this.response)
          this.saveToHistory()
        })
        .catch(error => {
          // this.loading = false
          this.$emit('bigLoading', false)
          console.log(error);
        });
    },
    deleteItem(data) {
      this.showHistoryItem = false
      this.history_list = this.history_list.filter((item) => {
        return item != data
      })
      localStorage.setItem('history-list', JSON.stringify(this.history_list));
      this.getHistory();
    },
    deleteAll() {
      this.history_list = {}
      localStorage.setItem('history-list', JSON.stringify(this.history_list));
      this.getHistory();
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

.button-row {
  margin-top: 1rem;
}

.input-row {
  margin-top: 1rem;
}

.widget-row {
  margin-top: 1rem;
}

.card-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.card-header-text {
  font-size: 1rem;
  font-weight: bolder;
}

.dialog-footer button:first-child {
  margin-right: 10px;
}

.footer-text {
  margin: 0;
  padding: 0;
  white-space: pre-wrap;
}
.footer-top{
  margin-top: 1rem;
  margin-bottom: 0;
  margin-left: 0;
  margin-right: 0;
  padding: 0;
  white-space: pre-wrap;
}
</style>