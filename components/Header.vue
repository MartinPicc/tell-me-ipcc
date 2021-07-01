<template>
    <div class="bg-background-img bg-center bg-cover font-mono">
        <div class="content pt-8 pb-10 md:pb-16 space-y-2">
            <h1 class="md:text-8xl text-5xl">Tell me IPCC,</h1>
            <div id="typewriter-effect-container" class="flex justify-end leading-relaxed md:leading-relaxed text-3xl md:text-6xl overflow-hidden">
                <p class="cursor whitespace-nowrap flex-grow">{{ text }}</p>
            </div>
            <p class="md:text-2xl text-xl font-bold">A semantic search engine to browse IPCC's report on climate change.</p>
        </div>
    </div>
</template>

<script>
export default {
    name: "Header",
    data: function() {
        return {
            char_ix: 0,
            speedForward: 100, //Typing Speed
            speedWait: 1000, // Wait between typing and backspacing
            speedBackspace: 25, //Backspace Speed
            isBackspacing: false,
            text: '',
            textArray: [
                'What is climate change?',
                'How can we mitigate climate change?',
                'What is the impact of climate change on sea level?',
                'What are the causes of climate change?',
                'What is the impact of human activities on CO2 concentration?'
            ],
            text_ix: 0
        }
    },
    methods: {
        typeWriter: function (id, array) {
            // cf https://codepen.io/daviddcarr/pen/XVyQMM?editors=0010
            var aString = this.textArray[this.text_ix];

            if (!this.isBackspacing) {
                if (this.char_ix < aString.length) {
                    this.text += aString.charAt(this.char_ix);
                    this.char_ix ++;
                    setTimeout( () => this.typeWriter(), this.speedForward);
                } else {
                    this.isBackspacing = true;
                    setTimeout( () => this.typeWriter(), this.speedWait);
                }
            } else {
                if ( this.text.length > 0 ) {
                    this.text = this.text.substring(0, this.text.length - 1);
                    setTimeout( () => this.typeWriter(), this.speedBackspace);
                } else {
                    this.isBackspacing = false;
                    this.char_ix = 0;
                    this.text_ix = (this.text_ix + 1) % this.textArray.length;
                    setTimeout( () => this.typeWriter(), this.speedWait);
                }
            }
        }
    },
    mounted: function () {
        this.typeWriter();
    }
}
</script>