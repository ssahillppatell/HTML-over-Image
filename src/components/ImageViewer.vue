<template>
    <div class="img-nav">
        <input type="number" min="0" max="2" v-model="currImageId" />
    </div>

    <div class="image">
        <img :src="images[currImageId].location" :alt="images[currImageId].name" />
        <span class="drag-resize">
            <p></p>
        </span>
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
    width: 1000px;
    @apply relative m-auto;

    img {
        width: 1000px;
        @apply border border-black;
    }

    .drag-resize {
        @apply absolute top-0 left-0;
        background-color: #29e;
        @apply text-white;

        width: 150px;
        height: 50px;

        touch-action: none;
    }
}
</style>

<script lang="ts">
import { defineComponent, ref } from 'vue';
import interact from 'interactjs';

function dragMoveListener(event: any) {
    var target = event.target,
        x = (parseFloat(target.getAttribute('data-x')) || 0) + event.dx,
        y = (parseFloat(target.getAttribute('data-y')) || 0) + event.dy;

    target.style.webkitTransform = target.style.transform = 'translate(' + x + 'px, ' + y + 'px)';

    target.setAttribute('data-x', x);
    target.setAttribute('data-y', y);
}

export default defineComponent({
    setup() {
        const images = [
            {
                id: 1,
                location: new URL('../assets/images/1.png', import.meta.url).toString(),
                name: 'Blue Image',
            },
            {
                id: 2,
                location: new URL('../assets/images/2.png', import.meta.url).toString(),
                name: 'Green Image',
            },
            {
                id: 3,
                location: new URL('../assets/images/3.png', import.meta.url).toString(),
                name: 'Purple Image',
            },
        ];

        const currImageId = ref(0);

        interact('.drag-resize')
            .resizable({
                edges: { left: true, right: true, bottom: true, top: true },

                listeners: {
                    move(event) {
                        var target = event.target;
                        var x = parseFloat(target.getAttribute('data-x')) || 0;
                        var y = parseFloat(target.getAttribute('data-y')) || 0;

                        target.style.width = event.rect.width + 'px';
                        target.style.height = event.rect.height + 'px';

                        x += event.deltaRect.left;
                        y += event.deltaRect.top;

                        target.style.transform = 'translate(' + x + 'px,' + y + 'px)';

                        target.setAttribute('data-x', x);
                        target.setAttribute('data-y', y);
                        target.textContent = `
                            Width: ${Math.round(event.rect.width)} | Height: ${Math.round(event.rect.height)} | 
                            X: ${document.querySelector('.drag-resize')!.getBoundingClientRect().x - document.querySelector('img')!.getBoundingClientRect().x} | Y: ${document.querySelector('.drag-resize')!.getBoundingClientRect().y - document.querySelector('img')!.getBoundingClientRect().y}
                        `
                    },

                    end(event) {
                        console.log(`Width: ${Math.round(event.rect.width)} | Height: ${Math.round(event.rect.height)}`)
                    }
                },
                modifiers: [
                    interact.modifiers.restrictEdges({
                        outer: 'parent',
                    }),

                    interact.modifiers.restrictSize({
                        min: { width: 100, height: 50 },
                    }),
                ],

                inertia: true,
            })
            .draggable({
                listeners: {
                    move: dragMoveListener,

                    end(event) {
                        var textEl = event.target;

                        textEl.textContent = `
                            Width: ${Math.round(event.rect.width)} | Height: ${Math.round(event.rect.height)} | 
                            X: ${document.querySelector('.drag-resize')!.getBoundingClientRect().x - document.querySelector('img')!.getBoundingClientRect().x} | Y: ${document.querySelector('.drag-resize')!.getBoundingClientRect().y - document.querySelector('img')!.getBoundingClientRect().y}
                        `
                    },
                },
                inertia: false,
                modifiers: [
                    interact.modifiers.restrictRect({
                        restriction: 'parent',
                        endOnly: true,
                    }),
                ],
            });

        return {
            currImageId,
            images,
        };
    },
});
</script>
