<template>
    <div class="page-success">
        <div class="columns is-multiline mt-6">
            <div class="column is-12">
                <h1 class="title">Thank you</h1>

                <p>Your order have been successfully placed and Order will be processed within 48 hours</p>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: 'Success',
    mounted() {
        document.title = 'Success | DjangoREST vs VUE'
    },
}
</script>