<template>
    <div>
        <Header />
        <div class="content relative md:-top-9 -top-5">            
            <div class="query-container w-full flex bg-white rounded-full shadow-md py-2 px-3 space-x-2 md:py-4 md:px-6 md:space-x-4 mb-6">
                <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6 md:h-8 md:w-8 text-gray-400 mt-1.5" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M21 21l-6-6m2-5a7 7 0 11-14 0 7 7 0 0114 0z" />
                </svg>
                <input 
                    class="flex-grow border-b border-gray-300 focus focus:outline-none py-1 text-base md:text-xl"
                    v-model="query" 
                    type="text" 
                    placeholder="Ask a question"
                    v-on:keyup.enter="getAnswers"
                    />
                <button @click="getAnswers" class="bg-green-500 text-white flex justify-center w-8 h-8 md:w-10 md:h-10 py-1 rounded-full shadow md:text-xl hover:shadow-md">
                    <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 5l7 7-7 7" />
                    </svg>
                </button>
            </div>
            <Description v-if="!answers.length && !is_loading" />
            <ul class="md:mx-6" v-if="answers.length">
                <QueryResult 
                    v-for="answer in answers" 
                    :key="answer.text"
                    :text="answer.text"
                    :relevancy_score="answer.relevancy_score"
                    :page="answer.page"
                    :answer_start_ix="answer.answer_start_ix"
                    :answer_end_ix="answer.answer_end_ix"
                    @feedback="saveFeedback"
                />
            </ul>
            <div class="text-center">
                <button 
                    v-if="answers.length" 
                    @click="loadMoreAnswers" 
                    class="bg-gray-500 text-white py-1 px-3 rounded-full shadow hover:shadow-md"
                >Load more answers</button>
            </div>
            <Loader v-if="is_loading"/>
        </div>
    </div>
</template>

<script>
import QueryResult from '~/components/QueryResult.vue'
import Header from '~/components/Header.vue'
import Description from '~/components/Description.vue'
import Loader from '~/components/Loader.vue'

export default {
    components: {
        Header, QueryResult, Description, Loader
    },
    async asyncData() {
        var answers = {};
        var query = '';
        var query_ = '';
        var is_loading = false;
        return { answers, query, query_, is_loading };
    },
    methods: {
        async getAnswers() {
            if ( '' === this.query ) {
                return;
            }
            this.answers = {};
            this.is_loading = true;
            this.query_ = this.query;

            const body = {
                query: this.query,
                n: process.env.N_ANSWERS,
                offset: 0
            }
            this.answers = await this.$http.$post('/api/search/', body);

            // const url = `/api/search/?query=${encodeURI(this.query)}&n=${process.env.N_ANSWERS}&offset=0`;
            // this.answers = await this.$http.$get(url);
            
            // var example = {
            //     'text': 'Lorem ipsum dolor sit amet, consectetur adipiscing elit. Cras auctor sapien non neque semper dictum. Cras sed lectus tellus. Fusce maximus est mi, vel posuere lorem condimentum ut. Nam commodo massa id ante molestie, vitae iaculis enim accumsan. Maecenas ac odio pellentesque, mattis nunc nec, dictum enim. Nunc ligula lorem, dictum id elementum id, suscipit eget arcu. Nullam eget ante sit amet erat facilisis pretium eu in turpis. Cras rhoncus non ipsum ut lacinia. Proin facilisis odio ac mattis tincidunt. Phasellus risus metus, consectetur et lorem at, malesuada luctus velit. Maecenas quis lacus id mauris tincidunt vehicula. Cras bibendum eu nisl vitae euismod.',
            //     'page': '100',
            //     'relevancy_score': '0.75',
            //     'answer_start_ix': '100',
            //     'answer_end_ix': '200'
            // }
            // this.answers = []
            // for (var i = 0; i < 10; i++) {
            //     this.answers.push(example);
            // }

            this.is_loading = false;
        },
        async loadMoreAnswers() {
            if ( '' === this.query_ ) {
                return;
            }
            this.is_loading = true;
            // const offset = this.answers.length;
            // const url = `/api/search/?query=${encodeURI(this.query_)}&n=${process.env.N_ANSWERS}&offset=${offset}`;
            // this.answers = this.answers.concat(await this.$http.$get(url));
            
            const body = {
                query: this.query_,
                n: process.env.N_ANSWERS,
                offset: this.answers.length
            }
            this.answers = this.answers.concat(await this.$http.$post('/api/search/', body));

            this.is_loading = false;
        },
        async is_nlp_processor_running() {
            this.nlp_processor_running = await this.$http.$get('/api/nlp-processor-running');
        }
    },
    computed: {
        link_to_pdf() {
            return `/${process.env.PDF_NAME}`;
        },
    }
}
</script>

<style scoped>
</style>