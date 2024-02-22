<template>
     <div class="container">        
        <b-jumbotron>
            <template #lead>
                Congrats, you completed the quiz!
            </template>

            <p>Score:  {{numCorrect}} / {{ numTotal }}</p>

            <b-button
                variant="primary"
                @click="reloadPage"
            >
                Restart
            </b-button>
        </b-jumbotron>
    </div>
</template>

<script>
export default {
    props: [
        'numCorrect',
        'numTotal'
    ],
    methods: {
        reloadPage() {
            window.location.reload();
        }
    }
}
</script>

<style scoped> 
/* "scoped" to only affect styles in this component */
    .container {
        margin-top: 25px;
    }
</style>