<script>
import AuditionItem from '../../components/autidtion/AuditionItem.vue';
import ProductionItem from '../../components/production/ProductionItem.vue';
import ProductionItemOffer from '../../components/production/ProductionItemOffer.vue';
import ProductionItemOpen from '../../components/production/ProductionItemOpen.vue';

export default {
    name: "ProfileInterest",
    data() {
        return {
            openedProductionList: []
        }
    },
    computed: {
        // activeKey: {
        //     get() {
        //         return this.$route.query.type;
        //     }
        // }
        scrapList() {
            return this.$store.state.auditionList.filter(v => v.isScrap);
        },
        myProductionList() {
            return this.$store.state.productionList.filter(v => v.isLiked);
        },
        myOfferedProductionList() {
            let productionList = [{
                name: "한겨레영화아카데미",
                imgSrc: "/assets/production/han.png",
                isLiked: true,
                auditionList: [
                    {
                        title: "좋은 소식",
                        role: "2",
                        ageRange: [23, 29],
                        productionName: "한겨레영화아카데미",
                        prefer: "G1",
                        isScrap: true,
                        dueDate: "2023-02-17",
                        isOffered: true,
                        genre: "movie",
                        gender: "1",
                        applicantCnt: 0,
                        yesCnt: 1,
                        noCnt: 1,
                    },
                ],
            }];
            // 큐시트 참고
            // const productionList = this.$store.state.productionList.filter(v => v.auditionList.filter(e => e.isOffered));
            return productionList;
        }
    },
    watch: {
        "$route.query": {
            immediate: true,
            handler: function (route) {
                this.activeKey = this.$route.query.type;
            }
        }
    },
    methods: {
        handleTabClick(val) {
            this.$router.replace("/profile/interest?type=" + val);
        }
    },
    mounted() {
        console.log("mounted");
    },
    components: { AuditionItem, ProductionItem, ProductionItemOpen, ProductionItemOffer }
}
</script>
<template>
    <div class="profile-interest container">

        <a-tabs v-model:activeKey="activeKey" size="large" :tabBarStyle="{ 'font-weight': '700' }"
            @tabClick="handleTabClick">
            <a-tab-pane key="scrap" :tab="`스크랩`">
                <div class="container__title">
                    관심 제작사 리스트({{ 4 }})
                </div>
                <div class="tab-panel tab-panel--production">
                    <ul class="production-list">
                        <li class="production-list__item" v-for="(production, index) in myProductionList" :key="index">
                            <ProductionItem v-bind="production" :index="index"></ProductionItem>
                        </li>
                    </ul>
                </div>
                <div class="container__title">
                    스크랩한 공고({{ scrapList.length }})
                </div>
                <div class="tab-panel tab-panel--scrap">
                    <ul class="audition-list">
                        <li class="audition-list__item" v-for="(scrap, index) in scrapList" :key="index">
                            <AuditionItem v-bind="scrap" :index="index"></AuditionItem>
                        </li>
                    </ul>

                </div>

            </a-tab-pane>
            <a-tab-pane key="open" tab="열람" force-render>
                <div class="container__title">
                    열람한 제작사 리스트({{ scrapList.length }})
                </div>
                <div class="tab-panel tab-panel--open">
                    <ul class="open-list">
                        <li class="open-list__item" v-for="(production, index) in myProductionList" :key="index">
                            <ProductionItemOpen v-bind="production" :index="index"></ProductionItemOpen>
                        </li>
                    </ul>
                </div>
            </a-tab-pane>
            <a-tab-pane key="offer" tab="받은 제안">
                <div class="container__title">
                    받은 제안({{ 1 }})
                </div>
                <div class="tab-panel tab-panel--open">
                    <ul class="open-list">
                        <li class="open-list__item" v-for="(production, index) in myOfferedProductionList" :key="index">
                            <template v-for="(audition, i) in production.auditionList" :key="i">
                                <ProductionItemOffer v-bind="{ ...production, ...audition }" :index="index">
                                </ProductionItemOffer>
                            </template>
                        </li>
                    </ul>
                </div>
            </a-tab-pane>
        </a-tabs>
    </div>
</template>
<style lang="scss" scoped>
.profile-interest {
    min-height: calc(100vh - 300px);
}

.audition-list,
.production-list {
    display: flex;
    flex-wrap: wrap;
    border: 1px solid #dadada;
    margin-bottom: 20px;

    &__item {
        flex: 50%;
    }
}
</style>
