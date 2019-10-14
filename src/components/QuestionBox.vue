<template>
    <div class="container question-box">
        <b-jumbotron header="Openmobo" lead="Giving you back your data">
            <template slot="lead">
                {{ currentQuestion.question }}
            </template>
                <hr class="my-4">
                <b-list-group>
                    <b-list-group-item v-for="(answer, index) in answers" 
                    :key="index" 
                    @click="selectAnswer(index)"
                    :class="[selectedIndex === index ? 'selected' : '']"
                    >
                        {{ answer }}    
                    </b-list-group-item>
                </b-list-group>
            <b-button
                variant="success" 
                href="#"
                @click="submitAnswer"
                :disabled="selectedIndex === null"
            >
                Submit Answer
            </b-button>
            <b-button
                variant="primary" 
                href="#" 
                @click="next"
            >
                Next
            </b-button>
        </b-jumbotron>
    </div>
</template>


<script>
import _ from 'lodash'

    export default {
        props: {
            currentQuestion: Object,
            next: Function,
            increment: Function
        },
        data() {
            return {
                selectedIndex: null,
                shuffledAnswers: []
            }
        },
        computed: {
            answers() {
                let answers = [...this.currentQuestion.incorrect_answers]
                answers.push(this.currentQuestion.correct_answer)
                return answers
            }
        },
        methods: {
            selectAnswer(index){
                this.selectedIndex = index
            },
            submitAnswer(){
                let isCorrect = false
                if (this.selectedIndex === this.correctIndex){
                    isCorrect = true
                }
                this.increment(isCorrect)
            },
            shuffleAnswers(){
                let answers = [...this.currentQuestion.incorrect_answers, this.currentQuestion.correct_answer]
                this.shuffledAnswers = _.shuffle(answers)
            }
        },
        watch: {
            currentQuestion: {
                immediate: true,
                handler() {
                    this.selectedIndex = null
                    this.shuffleAnswers()
                }
            }
        }
    }

</script>


<style scoped>
    .list-group {
        margin-bottom: 15px;
    }
    .list-group-item:hover{
        background-color: #eee;
        cursor:pointer;
    }
    .btn {
        margin: 0 5px;
    }
    .selected {
        background-color: lightblue;
    }
    .correct { 
        background-color: green;
        color: white;
    }
    .incorrect {
        background-color: red;
        color: white;
    }
</style>