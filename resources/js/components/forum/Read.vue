<template>
    <div v-if="question">
        <edit-question
         v-if="editing"
         :data = question
         ></edit-question>

        <ShowQuestion
            v-else
            :data = question
            >
        </ShowQuestion>
        <v-container >
            <Replies :question="question"/>
            <new-reply v-if="loggedIn" :questionSlug="question.slug"></new-reply>
            <div v-else class="mt-2">
                <router-link to="/login">
                    Login in to Reply
                </router-link>
            </div>
        </v-container>

    </div>
</template>

<script>
import ShowQuestion from './ShowQuestion'
import EditQuestion from './EditQuestion'
import Replies from '../reply/Replies'
import NewReply from '../reply/NewReply'
export default {
    components:{
        ShowQuestion,
        EditQuestion,
        Replies,
        NewReply
    },
    data(){
        return {
            question:null,
            editing:false
        }
    },
    created(){

        this.listen()
        this.getQuestion()

    },
    computed: {
        loggedIn() {
            return User.loggedIn()
        }
    },
    methods:{
        listen(){
            EventBus.$on('startEditing',()=>{
                this.editing = true
            })

            EventBus.$on('cancelEditing',()=>{
                this.editing = false
            })
        },
        getQuestion(){
            axios.get(`/api/question/${this.$route.params.slug}`)
            .then(res => this.question = res.data.data)
        }
    }
}
</script>

<style>
</style>
