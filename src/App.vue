<template>
    <ImageViewer />
</template>

<script lang="ts">
import { defineComponent } from 'vue';

import ImageViewer from './components/ImageViewer.vue';

export default defineComponent({
    name: 'HTML over Image',
    components: {
        ImageViewer,
    },
    setup() {},
});
</script>
