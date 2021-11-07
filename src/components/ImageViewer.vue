<template>
    <div class="img-nav">
        <input type="number" v-model="currImageId" />
    </div>
    <div class="image">
        <img :src="images[currImageId].location" :alt="images[currImageId].name" />
    </div>
</template>

<style lang="postcss">
.img-nav {
    @apply h-12 flex items-center justify-center;

    input[type='number'] {
        @apply pl-2 border border-black;
    }
}

.image {
    @apply flex justify-center;
    img {
        @apply w-1/2 border border-black;
    }
}
</style>

<script lang="ts">
import { defineComponent, ref } from 'vue';

export default defineComponent({
    setup() {
        const images = [
            {
                id: 1,
                location: 'src/assets/images/1.png',
                name: 'Blue Image',
            },
            {
                id: 2,
                location: 'src/assets/images/2.png',
                name: 'Green Image',
            },
            {
                id: 3,
                location: 'src/assets/images/3.png',
                name: 'Purple Image',
            },
        ];

        const currImageId = ref(0);

        return {
            currImageId,
            images,
        };
    },
});
</script>
