<template>
    <div class="md:text-xl space-y-3 px-2 md:px-4">
        <p><a href="https://www.ipcc.ch/">IPCC (Intergovernmental Panel on Climate Change)</a> is a United Nations institution in charge of studying climate change.</p>
        <p>IPCC releases once every few years a comprehensive report summarizing latest scientific studies in a wide variety of fields related to climate change.
            Latest report has been published in 2014 and the next one is expected for 2022.</p>
        <p class="text-sm md:text-base">
            Browse the full 2014 report 
            <a :href="link_to_pdf" target="_blank">
                here
                <svg xmlns="http://www.w3.org/2000/svg" class="h-3.5 inline align-sub" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M10 6H6a2 2 0 00-2 2v10a2 2 0 002 2h10a2 2 0 002-2v-4M14 4h6m0 0v6m0-6L10 14" />
                </svg>
            </a>.
        </p>
    </div>
</template>

<script>
export default {
    computed: {
        link_to_pdf() {
            return `${process.env.PDF_URL}`;
        },
    }
}
</script>
