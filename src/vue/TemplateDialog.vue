<template>
    <v-dialog
            v-model="modal"
            scrollable
            persistent
            width="920px"
            height="920px">
        <v-card>

            <v-spacer></v-spacer>

            <v-card-text style="height: 640px" >
                <div class="row" v-if="template_selected">
                    <div class="col-12">
                        <h3 class="text-center mt-5 mb-5">{{template_selected.name}}</h3>
                        <vueper-slides
                                class="no-shadow"
                                :visible-slides="1"
                                slide-multiple
                                :gap="3"
                                fixed-height="400px"
                                :dragging-distance="200"
                                :breakpoints="{ 800: { visibleSlides: 1, slideMultiple: 1 } }">
                            <vueper-slide v-for="(photo, index) in template_selected.images" :key="index" :image="photo.image.url" />
                        </vueper-slides>
                    </div>
                </div>
                <br>
               <div class="row mt-5">
                   <div class="col-12 d-flex align-items-center justify-content-center text-center">
                       <button class="btn-normal mr-3" @click="proceed()">Use this template</button>
                       
                       <button class="btn-normal" @click="closeDialog()">Keep Browsing</button>

                   </div>
               </div>
            </v-card-text>

        </v-card>
    </v-dialog>
</template>

<script>
    const axios = require('axios');

    export default {
        props:['template','template_dialog'],
        computed:{
            modal(){
                return this.template_dialog;
            },
            template_selected(){
                return this.template;
            }
        },
        data () {
            return {
                //  dialog: false,
            }
        },
        methods:{
            closeDialog(){
                this.$emit('closeTemplateDialog');
            },
            proceed(){
                this.$emit('proceed');
            }
        }

    }
</script>


<style >
    .vueperslides__arrow.vueperslides__arrow--prev, .vueperslides__arrow.vueperslides__arrow--next{
        background: black !important;
    }

    .vueperslides__arrow.vueperslides__arrow--prev:hover, .vueperslides__arrow.vueperslides__arrow--next:hover{
        background: #f58320 !important;
    }
</style>
