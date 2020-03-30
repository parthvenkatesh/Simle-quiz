<template>
    <div class="question-box-container">
    <b-jumbotron >
        
        <template v-slot:lead>
        {{ques.question}}
        </template>

        <hr class="my-4">

        <b-list-group>
            <b-list-group-item 
            v-for="(ans,ind) in answers" 
            :key="ind"
            @click="opted(ind)"
            :class="retClass(ind)"
            >
            {{ans}}
        </b-list-group-item>
        
        </b-list-group>
        

        <b-button 
        @click="validate"
        :disabled="selected===null || validated===1"
        variant="primary"
        >Submit</b-button>

        <b-button @click="next" variant="success" href="#" :disabled="validated===0">Next</b-button>
    </b-jumbotron>
    </div>
</template>

<script>
import _ from 'lodash'
export default {
    props :{
        ques: Object,
        next:Function,
        increment : Function
    },
    data(){
        return{
            selected:null,
            shuffledans:[],
            correctIndex:null,
            validated:0
        }
    },
    watch:{
        ques:{
            immediate:true,
            handler(){
                this.selected=null
                this.shuffle()
                this.validated=0
            }
        }
    }
    ,
    methods:{
        opted(ind){
            this.selected=ind
        },
        shuffle(){
            let ans = [...this.ques.incorrect_answers, this.ques.correct_answer]
            this.shuffledans = _.shuffle(ans)
            this.correctIndex = this.shuffledans.indexOf(this.ques.correct_answer)
        },
        validate(){
            this.validated=1
            let isCorrect = false
            if(this.selected === this.correctIndex)
                isCorrect = true
            this.increment(isCorrect)

        },
        retClass(index){
            let ans = ''
            if(!this.validated && this.selected === index)
                ans='selected'
            else if(this.validated && this.correctIndex ===index)
                ans='correct'
            else if(this.validated && index===this.selected && this.correctIndex!==this.selected)
                ans='incorrect'
            return ans
        }

    },
    computed :{
        answers(){
            let ans = [...this.ques.incorrect_answers]
            ans.push(this.ques.correct_answer)
            return ans
        }
    }
}
</script>

<style scoped>
.list-group{
    margin-bottom: 15px;
}

.list-group-item:hover{
    background: #EEE;
    cursor: pointer;
}

.btn{
    margin: 0 5x;
}
.selected{
    background-color: lightblue;
}
.correct{
    background-color: lightgreen;
}
.incorrect{
    background-color: red;
}
</style>