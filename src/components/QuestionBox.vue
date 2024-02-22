<template>
    <div class="question-box-container">
        <b-jumbotron>
            <template #lead>
                {{ currentQuestion.question }}
            </template>

            <b-list-group>
                <b-list-group-item 
                    v-for="(answer, index) in answers"
                    :key="index"
                    @click.prevent="selectAnswer(index)"
                    :class="answerClass(index)"
                    :disabled="answered"
                >
                    {{  answer }}
                </b-list-group-item>
            </b-list-group>

            <b-button 
                variant="primary"
                @click="submitAnswer"
                :disabled="selectedIndex === null || answered"
            >
                Submit
            </b-button>
            <b-button variant="success" @click="next" :disabled="selectedIndex === null || !answered">Next</b-button>
        </b-jumbotron>
    </div>
</template>

<script>
import _ from 'lodash'

export default {
    props: {
        currentQuestion: Object,
        next: Function,
        increment: Function,
        isQuizOver: Boolean
    },
    data() {
        return {
            selectedIndex: null,
            shuffledAnswers: [],
            answered: false,
            correctIndex: null,
        }
    },
    computed: {
        answers() {
            let answers = [...this.currentQuestion.incorrect_answers]
            answers.push(this.currentQuestion.correct_answer)
            return answers
        }
    },
    watch: { //runs when props changes
        currentQuestion: { //same name as variable that will change
            immediate: true, //runs this function when this variable is first passed as prop and every other time it changes
            handler() {
                this.selectedIndex = null
                this.answered = false
                this.shuffleAnswers()
            }
        }
    },
    methods: {
        selectAnswer(index) {
            this.selectedIndex = index
            console.log(index)
        },
        submitAnswer() {
            let isCorrect = false;

            if(this.selectedIndex === this.correctIndex) {
                isCorrect = true
            }

            this.answered = true
            this.increment(isCorrect)
        },
        shuffleAnswers() {
            let answers = [...this.currentQuestion.incorrect_answers, this.currentQuestion.correct_answer]
            this.shuffledAnswers = _.shuffle(answers)
            this.correctIndex = this.shuffledAnswers.indexOf(this.currentQuestion.correct_answer)
        },
        answerClass(index) {
            let answerClass = ''

            if (!this.answered && index === this.selectedIndex) {
                answerClass = 'selected'
            } else if (this.answered && this.correctIndex === index) {
                answerClass = 'correct'
            } else if (this.answered && this.correctIndex !== index && this.selectedIndex === index ) {
                answerClass = 'incorrect'
            }

            return answerClass
        }
    },
    // mounted() { //could do that to shuffle first question when mounted, but using "immediate" on "watch" above is better/cleaner
    //     this.shuffledAnswers(); 
    // }
}
</script>

<style scoped> 
/* "scoped" to only affect styles in this component */
    .question-box-container {
        margin-top: 15px;
    }
    
    .list-group {
        margin-bottom: 15px;
    }

    .list-group-item:hover{
       background-color: lightgray;
       cursor: pointer;
    }

    .btn {
        margin: 0 5px;
    }

    .selected {
        background-color: lightblue;
    }

    .correct {
        background-color: lightgreen;
    }

    .incorrect {
        background-color: lightcoral;
    }
</style>