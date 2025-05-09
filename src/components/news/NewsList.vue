<script setup>
import { ref, getCurrentInstance, watch } from 'vue';
const { proxy } = getCurrentInstance();
import { getNewsList } from './news-data.js';
import NewsImages from '@/assets/images/news/config-name.js';
import { getWebPImage, getOriginalImage } from '@/utils/imageUtils';
import miment from 'miment';
import { useI18n } from 'vue-i18n';
const { t, locale } = useI18n();

const list = ref(getNewsList(t));
const loading = ref(false);
const finished = ref(false);

// 监听语言切换，更新新闻列表
watch(locale, () => {
    list.value = getNewsList(t);
});

function onLoad() {
    // 异步更新数据
    // setTimeout 仅做示例，真实场景中一般为 ajax 请求
    setTimeout(() => {
        // for (let i = 0; i < 10; i++) {
        //   list.value.push(list.value.length + 1);
        // }
        list.value = [...list.value, ...getNewsList(t)];

        // 加载状态结束
        loading.value = false;

        // 数据全部加载完成
        if (list.value.length >= 10) {
            finished.value = true;
        }
    }, 1000);
}

function getYear(date) {
    return miment(date).format('YYYY');
}
function getMonth(date) {
    return miment(date).format('MM-DD');
}
function toDetail(id) {
    proxy.$router.push({
        name: 'news-detail',
        query: {
            id
        }
    });
}
</script>

<template>
    <div class="page">
        <van-list v-model:loading="loading" :finished="finished" :finished-text="t('no more')" @load="onLoad" class="news-list">
            <van-cell v-for="item in list" :key="item">
                <div class="cell-box" @click="toDetail(item.id)">
                    <div class="time-title-box">
                        <div class="time">
                            <p class="time1">{{ getMonth(item.date) }}</p>
                            <p class="time2">{{ getYear(item.date) }}</p>
                        </div>
                        <div class="title">
                            <p class="title1">{{ item.title }}</p>
                            <p class="title2">{{ item.subtitle }}</p>
                        </div>
                    </div>
                    <div class="img-box">
                        <picture>
                            <source :srcset="getWebPImage(NewsImages[item.imgUrl], 'news')" type="image/webp" />
                            <img :src="getOriginalImage(NewsImages[item.imgUrl], 'news')" alt="" class="img" />
                        </picture>
                    </div>
                </div>
            </van-cell>
        </van-list>
    </div>
</template>

<style lang="scss" scoped>
.page {
    padding: 20px;
    background: var(--van-cell-background);
}
</style>

<style lang="scss">
.van-list.news-list {
    .van-cell {
        padding: 20px 50px;
        cursor: pointer;
        &:hover {
            background: rgba(76, 152, 250, 0.1);
        }
        .cell-box {
            display: flex;
            justify-content: space-between;

            .time-title-box {
                flex: 1;
                display: flex;
                .time {
                    font-weight: bold;
                    width: 100px;
                    letter-spacing: 2px;
                    .time1 {
                        font-size: 20px;
                        color: var(--my-red);
                        margin-bottom: 10px;
                    }
                    .time2 {
                        font-size: 18px;
                        color: var(--my-blue);
                    }
                }
                .title {
                    flex: 1;
                    .title1 {
                        font-size: 17px;
                        font-weight: bold;
                        color: var(--van-text-color);
                        margin-bottom: 10px;
                    }
                    .title2 {
                        font-size: 15px;
                    }
                }
            }
            .img-box {
                width: 140px;
                height: 80px;
                margin-left: 15px;
                .img {
                    width: 90%;
                    height: 90%;
                }
            }

            &:hover {
                .time-title-box {
                    .time1 {
                        font-size: 21px;
                        transition: font-size 0.5s;
                    }
                    // .time2 {
                    //   font-size: 19px;
                    //   transition: font-size 0.5s;
                    // }
                    .title1 {
                        font-size: 18px;
                        transition: font-size 0.5s;
                    }
                }
                .img-box .img {
                    width: 100%;
                    height: 100%;
                    transition: width 0.8s, height 0.8s;
                }
            }
        }
    }
}
@media screen and (max-width: 960px) {
    .van-list.news-list {
        .van-cell {
            padding: 15px;
            .cell-box {
                .time-title-box {
                    .time {
                        width: 80px;
                    }
                }
            }
        }
    }
}
@media screen and (max-width: 740px) {
    .van-list.news-list {
        .van-cell {
            .cell-box {
                .time-title-box {
                    .time {
                        letter-spacing: 0px;
                        width: 70px;
                    }
                }
                .img-box {
                    display: none;
                }
            }
        }
    }
}
</style>
