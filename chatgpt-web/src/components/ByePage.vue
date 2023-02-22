<template>
    <el-row justify="center">
        <el-col :span="16">
            <el-row justify="center">
                <p class="hero-title">让我们，下个路口见！</p>
            </el-row>
            <el-row justify="center" align="middle">

                <el-link icon="Link" class="hero-subtitle" href="https://space.bilibili.com/34147682" target="_blank">
                    BiliBili
                </el-link>

            </el-row>


            <el-row justify="center">
                <p class="desc">💔因众所周知的原因，即日起我们将停止这个页面的使用，开启时间另行通知</p>
            </el-row>
            <el-row justify="center">
                <p class="desc">🚀我们将在AIGC（人工智能生产内容）的方面继续分享，帮助更多的人学好用好这些工具</p>
            </el-row>

            <el-row justify="center">
                <p class="desc">😊请关注我们的公众号“共享技术官” </p>
            </el-row>

            <el-row justify="center">
                <el-image style="width: 100px; height: 100px"
                    src="https://i.postimg.cc/2jTQgP9g/qrcode-for-gh-0e5c8d2cb1b0-258.jpg" />
            </el-row>
            <el-row justify="center">

                <el-image style="width: 1600px; height: 900px;"
                    src="https://s1.ax1x.com/2023/02/22/pSjz74J.png" />
            </el-row>
        </el-col>
    </el-row>
</template>

<script>

export default {
    mounted() {
        document.documentElement.className = 'dark';
    }
}
</script>

<style>
.hero-title {
    font-size: 2rem;
    margin: 1rem;
    font-weight: bolder;
}

.hero-subtitle {
    font-size: 1.5rem;
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

.desc-small {
    font-size: 1rem;
    margin: 0rem 0;
    font-weight: bolder;

}
</style>