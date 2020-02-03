<template>
    <div>
        <span class="tab"
            :class="{ activeTab: selectedTab === tab  }"
            v-for="(tab, index) in tabs"
            :key="index"
            @click="selectedTab = tab">
            {{ tab }}
        </span>

        <div v-show="selectedTab === 'Reviews'">
            <div class="no-reviews" v-if="!reviews.length">
                <p>There are no reviews for this product yet.</p>
            </div>
            <ul v-else>
                <li v-for="(review, index) in reviews" :key="index">
                    <p>{{ review.name }}</p>
                    <p>Raiting: {{ review.rating }}</p>
                    <p>{{ review.review }}</p>
                </li>
            </ul>
        </div>  

        <Review 
            class="review"
            v-show="selectedTab === 'Create Review'"/>

    </div>
</template>

<script>
import Review from './product-review.vue'

export default {
    name: 'Tabs',
    components: { Review },
    props: ['reviews'],
    data() {
        return {
            tabs: ['Reviews', 'Create Review'],
            selectedTab: 'Reviews'
        }
    }
}
</script>

<style>
    .tab {
        padding: 5px;
        margin: 2px;
        background-color: lightgray;
        border-radius: 5px 5px;
    }
    .activeTab {
        border: 1px solid #1dbf81;
    }

    .no-reviews {
        margin-top: 30px;
        width: 100%;
        border: 1px solid rgb(51, 52, 97);
        text-align: center;
    }

    .review {
        margin-top: 30px;
    }
</style>


