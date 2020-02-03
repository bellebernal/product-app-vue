<template>
    <div class="details">
        <p>Product Details:</p>
        <p>
            <ul>
                <li 
                    v-for="detail in details" 
                    :key="detail.id">
                    {{ detail.value }}
                </li>
            </ul>
        </p>
    </div>
</template>

<script>
export default {
    name: 'Details',
    data() {
        return {
            details: [
                {
                    'id': 1, 
                    'value': '80% cotton'
                }, 
                {
                    'id': 2, 
                    'value': '20% polyester'
                }, 
                {
                    'id': 3, 
                    'value': 'one-size'
                }
            ]
        }
    }
}
</script>

<style>
    .details {
        display: flex;
    }
</style>
