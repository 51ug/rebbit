<template>
    <img class="ct" :src="image_options.preview" loading="lazy" :style="image_options.style"
        @click.prevent="emit('open_post')">
</template>

<script setup>
import { ref } from 'vue';
import { blurFunc } from '/js/functions.js'

const emit = defineEmits(['open_post']);
const props = defineProps({
    data: {
        type: Object,
        required: true
    }
})

const image_options = ref({});

async function get_sources() {
    if (!props.data.preview) {
        image_options.value = {
            src: props.data.url,
            preview: props.data.url,
            style: {
                'aspect-ratio': `${props.data.thumbnail.width} / ${props.data.thumbnail.height}}`,
                'filter': blurFunc.IfOver18(props.data.over_18)
            }
        }
        return;
    }

    image_options.value = {
        src: props.data.url,
        preview: props.data.preview.images[0].resolutions.pop().url.replaceAll("&amp;", "&"),
        style: {
            'aspect-ratio': `${props.data.preview.images[0].resolutions.slice(-1)[0].width} / ${props.data.preview.images[0].resolutions.slice(-1)[0].height}`,
            'filter': blurFunc.IfOver18(props.data.over_18)
        }
    }
}

// setup
get_sources();
</script>