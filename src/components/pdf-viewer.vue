<template>
    <div    class="pdf_page"
           
            style="width: 100vw; height: 100vh;">

            <pdf :src="pdf" 
                v-for="index in numpages" :key="index" :page="index" :id="index"
                :style="page_style"
                @load="load_change"
                :scale="page_scale"  
                ></pdf>
    </div>
</template>

<script>
import pdf from 'pdfvuer'

export default {
    components: {
        pdf
    },
    computed: {
        page_style() {
           return window.innerHeight < window.innerWidth ? {"width" : "100%"} : {"height" : "100%"}
        },
        page_scale() {
            return window.innerHeight > window.innerWidth ? "page-width" : "page-height";
            //return Math.max( window.innerWidth / window.innerHeight, window.innerHeight / window.innerWidth );
        }
    },
    methods: {
        clamp( num, min, max) {
            return Math.min(Math.max(num, min), max)
        },
        load_pdf( event) {
            this.pdf = undefined;
            this.pdf = pdf.createLoadingTask(event.detail.path, {onProgress : (data) => console.log(data)});
            this.pdf.then( pdf => { this.numpages = pdf.numPages;} );
        },
        load_change( data ) {
            console.log("Load change", data);
        },
        turn_page( event ) {
            this.page_index = this.clamp( this.page_index + event.detail.offset, 1, this.numpages);
            this.scroll();
        },
        go_to_page( event ) {
            this.page_index = this.clamp( event.detail.page, 1, this.numpages );
            this.scroll();
        },
        scroll() {
            document.getElementById(String(this.page_index)).scrollIntoView({behavior: "smooth"});
        }
    },
    data: () => ( {
        path     :   "",
        page_index : 1,
        pdf : undefined,
        numpages : 0

    }),
    created() {
        document.addEventListener( "loadpdf", this.load_pdf );
        document.addEventListener( "turnpage", this.turn_page );
        document.addEventListener( "gotopage", this.go_to_page );
    }
}
</script>

<style>
#viewerContainer {
    display: flex; 
    justify-content: center; 
    align-items: center;
  
    height: 100%;
    width: 100%;
}
</style>