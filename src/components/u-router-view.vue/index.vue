<template>
<div :class="$style.fake" v-if="$env.VUE_APP_DESIGNER" :designer="$env.VUE_APP_DESIGNER && designer">
    <router-view></router-view>
</div>
<router-view v-else></router-view>
</template>

<script>
export default {
    name: 'u-router-view',
    props: {
        designer: { type: Boolean, default: true },
    },
};
</script>

<style module>
.fake:empty {
    min-height: 32px;
    text-align: center;
}
.fake[designer]:empty {
    background: linear-gradient(-45deg, white 25%,
    #f3f5fa 25%, #f3f5fa 50%,
    white 50%, white 75%,
    #f3f5fa 75%);
    background-size: 45px 45px !important;
    background-repeat: repeat !important;
}
.fake:empty:after {
    content: '此容器为子页面呈现占位，可在子页面编辑内容';
    font-size: 12px;
    line-height: 32px;
    color: var(--font-second-color);
}
</style>
