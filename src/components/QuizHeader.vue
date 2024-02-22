<template>
    <div>
        <b-nav tabs>
            <b-nav-item disabled>Vue Quiz App</b-nav-item>
            <b-nav-item 
                v-if="!isQuizOver"
                disabled
            >
                Counter: {{numCorrect}} / {{ numTotal }}
            </b-nav-item>
        </b-nav>
    </div>
</template>

<script>
export default {
    props: [
        'numCorrect',
        'numTotal',
        'isQuizOver'
    ]
}
</script>