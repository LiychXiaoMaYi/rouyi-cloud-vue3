<script setup>
import {ref} from 'vue'
import { PUBLIC_PATH } from '../../../config/commonConfig';
import IframeComp from '../../../components/IframeComp/index.vue';

const url = ref(PUBLIC_PATH + "/swagger-ui/index.html")
</script>
<template>
   <iframe-comp :src="url"></iframe-comp>
</template>

<style lang='scss' scoped>

</style>