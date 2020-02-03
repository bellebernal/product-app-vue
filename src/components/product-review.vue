<template>
    <form class="review-form" @submit.prevent="onSubmit">
        <p class="errors">
            <b v-if="errors.length">Please correct the following error(s), required:</b>
            <ul>
                <li v-for="error in errors" :key="error.id">{{ error }}</li>
            </ul>
        </p>

        <p>
            <label for="name">Username</label>
            <input id="name" type="text" v-model="name">
        </p>

        <p>
            <label for="review">Review</label>
            <textarea id="review" type="text" v-model="review"></textarea>
        </p>

        <p>
            <label for="rating">Rating: </label>
            <select id="rating" v-model.number="rating">
            <!-- v-model.number - the .number is modifier that typecasts the input -->
                <option>5</option>
                <option>4</option>
                <option>3</option>
                <option>2</option>
                <option>1</option>
            </select>
        </p>

        <p>
            <input id="submit" type="submit" value="Submit">
        </p>
    </form>
</template>

<script>
import { eventBus } from '@/main.js'

export default {
    name: 'Review',
    data() {
        return {
            name: null,
            review: null,
            rating: null,
            errors: []
        }
    },
    methods: {
        onSubmit() {
            this.errors = [];
            if(this.name && this.review && this.rating){
                let productReview = {
                    name: this.name,
                    review: this.review,
                    rating: this.rating
                }

                eventBus.$emit('review-submitted', productReview);

                // reset the form back to blank
                this.name = null;
                this.review = null;
                this.rating = null;
            } else {
                if(!this.name) this.errors.push('Name required.');
                if(!this.review) this.errors.push('Review required.');
                if(!this.rating) this.errors.push('Rating required.');
            }
        }
    }
}
</script>

<style>
.review-form {
    width: 70%;
    height: auto;
    padding: 20px;
    border: 1px solid rgb(51, 52, 97);
}

#submit {
    background-color: rgb(51, 52, 97);
    color: #1dbf81;
    border-radius: 5px 15px;
    height: 50px;
    width: 100px;
    font-size: 14px;
}


</style>


