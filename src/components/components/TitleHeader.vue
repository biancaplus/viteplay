<script setup>
import { ref } from 'vue';

const props = defineProps({
    title: String,
    type: Number
});
</script>

<template>
    <div class="title-header-wrap" :class="props.type == 2 ? 'title-header-wrap2' : ''">
        <h5 class="title">
            <span>{{ props.title }}</span>
        </h5>
        <div class="header-border">
            <div class="separator"></div>
        </div>
    </div>
</template>

<style lang="scss" scoped>
.title-header-wrap {
    width: 100%;
    margin-bottom: 20px;
    .title {
        color: var(--my-text-color-1);
        font-family: 'Open Sans', Hind;
        font-weight: 200;
        font-size: 20px;
        margin: 0;
        margin-bottom: 15px;
    }
    .header-border {
        width: 100%;
        height: 1px;
        background: var(--my-border-color-2);
        .separator {
            width: 48px;
            height: 1px;
            background: var(--my-theme);
        }
    }

    &.title-header-wrap2 {
        text-align: center;
        position: relative;
        .header-border {
            .separator {
                margin: 0 auto;
            }
        }
    }
}

@media screen and (max-width: 960px) {
    .title-header-wrap {
        margin-bottom: 15px;
    }
}
@media screen and (max-width: 500px) {
    .title-header-wrap {
        margin-bottom: 10px;
    }
}
</style>
