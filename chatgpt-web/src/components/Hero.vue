<template>
    <el-row justify="center">
        <el-col :span="16">
            <el-row justify="center">
                <p class="hero-title">ChatGPT网页版</p>
            </el-row>
            <el-row justify="center">
                <el-link class="hero-subtitle">🏡https://github.com/SmileBuild/ChatGPT-Web/</el-link>
            </el-row>
            <el-row justify="center">
                <p class="desc">😊Editor:Smile</p>
            </el-row>
            <el-row justify="center">
                <p class="desc">🚀Easy, Fast, Everywhere</p>
            </el-row>
            <el-row justify="center">
                <p class="desc">🌊Let's surf together with chatGPT now.</p>
            </el-row>
            <el-row :gutter="20" justify="center">
                <el-image alt="Version"
                    src="https://img.shields.io/badge/bilibili-共享技术官-00A1D6?logo=bilibili&link=https://space.bilibili.com/34147682" />
                <el-image alt="wechat" src="https://img.shields.io/badge/wechat-共享技术官-07C160?logo=wechat" />


                <el-image alt="GitHub" src="https://img.shields.io/github/license/smilebuild/chatgpt-web" />
                <el-image alt="Version"
                    src="https://img.shields.io/github/package-json/v/SmileBuild/ChatGPT-Web?filename=chatgpt-web%2Fpackage.json" />
                <el-image alt="Last Commit" src="https://img.shields.io/github/last-commit/SmileBuild/Chatgpt-Web" />
                <el-image alt="build"
                    src="https://github.com/SmileBuild/ChatGPT-Web/actions/workflows/pages/pages-build-deployment/badge.svg?branch=gh-pages" />
                <el-image alt="build-status"
                    src="https://github.com/SmileBuild/ChatGPT-Web/actions/workflows/pages/pages-build-deployment" />
                <el-image alt="build-status" src="https://img.shields.io/github/stars/smilebuild/ChatGPT-Web" />
                <el-image alt="Vue" src="https://img.shields.io/badge/Vue.js-35495E?logo=vue.js&logoColor=4FC08D" />

            </el-row>
        </el-col>
    </el-row>
</template>

<script>

</script>

<style>
.hero-title {
    font-size: 4rem;
    margin: 1rem;
    font-weight: bolder;
}

.hero-subtitle {
    font-size: 2rem;
    margin: 1rem;
    font-weight: bold;
}

.el-image {
    margin: 1px;
}

.desc {
    font-size: 1.5rem;
    margin: 0rem 0;


}
</style>