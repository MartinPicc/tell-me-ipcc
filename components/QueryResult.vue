<template>
    <li class="text-sm my-4 border rounded-md overflow-hidden bg-gray-50">
        <div class="shadow bg-white">
            <div class="bg-gray-200">
                <div :style="score_bar_style" class="h-1"></div>
            </div>
            <div class="flex justify-between py-1 px-2">
                <p>Relevancy {{ rounded_score }} / 100</p>
                <p>
                    <a :href="link_to_pdf" target="_blank">
                        Page {{ page_ }}
                        <svg xmlns="http://www.w3.org/2000/svg" class="h-4 inline align-sub" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M10 6H6a2 2 0 00-2 2v10a2 2 0 002 2h10a2 2 0 002-2v-4M14 4h6m0 0v6m0-6L10 14" />
                        </svg>
                    </a>
                </p>
            </div>
        </div>
        <p v-html="formatted_text" class="px-2 my-2"></p>
    </li>
</template>

<script>
export default {
    name: "QueryResult",
    props: ["relevancy_score", "text", "page", "answer_start_ix", "answer_end_ix"],
    computed: {
        rounded_score() {
            return Math.round(this.relevancy_score * 100);
        },
        score_bar_style () {
            return {
                width: `${this.rounded_score}%`,
                backgroundColor: `hsl(${30 + (90 - 30) * this.rounded_score / 100}, 100%, 42%)`
            }
        },
        formatted_text() {
            return (this.text.slice(0, this.answer_start_ix) 
                    + '<b>' + this.text.slice(this.answer_start_ix, this.answer_end_ix) + '</b>'
                    + this.text.slice(this.answer_end_ix));
        },
        link_to_pdf() {
            return `/${process.env.PDF_NAME}#page=${this.page_}`;
        },
        page_() {
            return parseInt(this.page) + 1
        }
    }
}
</script>
