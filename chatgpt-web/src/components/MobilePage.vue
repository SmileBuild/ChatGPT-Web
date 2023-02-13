<template>
    <el-container style="height:90vh">
        <el-header>
            <el-row :gutter="8" justify="space-between">
                <el-col :span="4">
                    <el-button size="large" type="primary" icon="Monitor" @click="jumpWeb" circle>
                    </el-button>
                </el-col>
                <el-col :span="16"><el-form label-width="auto" size="large">
                        <el-input v-model="api_key" placeholder="add key here. 在这里输入秘钥" show-password
                            type="password"></el-input></el-form>
                </el-col>
            </el-row>
        </el-header>
        <el-main>


            <el-list>
                <el-list-item v-for="(chat, index) in chatList" :key="index">

                    <!--判断消息是从自己发出还是接收-->
                    <template v-if="chat.from === 'self'">
                        <el-row :gutter="8" justify="end" style="margin-top: 15px;">

                            <el-card>
                                <span style="white-space: pre-wrap; margin: 8px;">{{ chat.text }}</span>
                            </el-card>
                            <el-avatar style="margin-left:4px;" icon="UserFilled" />
                        </el-row>
                    </template>
                    <template v-else>
                        <el-row :gutter="8" justify="start" style="margin-top: 15px;">
                            <el-avatar size="mini" style="margin-right: 4px;">
                                <img :src="chat.face" />
                            </el-avatar>
                            <el-card>
                                <span style="white-space: pre-wrap; margin: 8px;">{{ chat.text }}</span>
                            </el-card>

                        </el-row>
                    </template>

                </el-list-item>
            </el-list>
        </el-main>
        <el-footer>
            <el-row justify="end">
                <el-col :span="22" style="margin-bottom:24px;">
                    <el-form label-width="auto" size="large">
                        <el-input type="textarea" v-model="prompt" placeholder="Input text here. 在这里输入内容"
                            rows="3"></el-input></el-form>
                </el-col>
                <el-col :span="2" justify="end">

                    <el-button size="large" type="success" icon="Search" @click="submitForm">
                    </el-button>
                </el-col>

            </el-row>
        </el-footer>
    </el-container>

</template>

<script>
import axios from 'axios'
export default {

    emits: ['turnMobile'],
    data() {
        return {
            api_key: "",
            prompt: '',
            chatMsg: '',
            prompt: '',
            max_tokens: 1000,
            model: 'text-davinci-003',
            mobile: true,
            response: '',
            chatList: [
                {
                    from: 'bot',
                    face:
                        'https://i.postimg.cc/jdS60KQW/openai.png',
                    text: 'Hi，你好'
                },
                {
                    from: 'self',
                    face:
                        'https://i.postimg.cc/jdS60KQW/openai.png',
                    text: '嗨，很高兴认识你'
                }
            ]
        };
    },
    mounted() {
        var temp_key = localStorage.getItem('api-key')
        if (temp_key != null)
            if (temp_key != '') {
                this.api_key = localStorage.getItem('api-key')
            }
    },
    methods: {

        jumpWeb() {
            this.$emit('turnMobile', false)
        },
        submitForm() {
            let data = {
                prompt: this.prompt,
                model: this.model,
                max_tokens: this.max_tokens,
            }
            // this.loading = true
            this.$emit('bigLoading', true)
            this.chatList.push({
                from: 'self',
                face:
                    'https://i.postimg.cc/jdS60KQW/openai.png',
                text: this.prompt
            });
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
                    this.chatList.push({
                        from: 'bot',
                        face:
                            'https://i.postimg.cc/jdS60KQW/openai.png',
                        text: this.response
                    });
                    console.log(this.response)

                    this.saveToHistory()
                    this.prompt = "";
                })
                .catch(error => {
                    // this.loading = false
                    this.$emit('bigLoading', false)
                    console.log(error);
                });
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

        },
    }
};
</script>