<template>
    <div :id="id" class="modal" v-show="show">
        <div class="modal-content">
            <div class="modal-header">
                <span class="closeModal" @click="closeModal()">&times;</span>
                <h2 class="modal-title" >{{day}} ({{date}})</h2>
            </div>
            <div class="modal-body" :class="{dark: isDark}">
                <slot></slot>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: "Modal",
    
    data() {
        return { 
            show: false
        }
    },

    props: {
        id: String,
        date: String,
        day: String,
        isDark: Boolean
    },
    methods: {
        closeModal() {
            this.show = false
            document.removeEventListener('click', this.clickOut);
        },
        openModal() {
            this.show = true
            document.addEventListener('click', this.clickOut);
            

        },
        clickOut(e) {
            if(e.target == this.$el) {
                this.closeModal()
            }
        }
    },
};
</script>

<style scoped>
    hr {
        width: 80%;
        margin-top: 30px
    }

    /* The Modal (background) */
    .modal {
        /* display: none; */
        position: fixed; /* Stay in place */
        z-index: 1; /* Sit on top */
        padding-top: 100px; /* Location of the box */
        left: 0;
        top: 0;
        width: 100%; /* Full width */
        height: 100%; /* Full height */
        overflow: auto; /* Enable scroll if needed */
        background-color: rgb(0,0,0); /* Fallback color */
        background-color: rgba(0,0,0,0.4); /* Black w/ opacity */
    }
    
    /* Modal Content */
    .modal-content {
        overflow-y: scroll;
        text-align: center;
        position: relative;
        background-color: #fefefe;
        margin: auto;
        padding: 0;
        border: 5px solid white;
        width: 70vw;
        max-width: 800px;
        height: 60vh;
        box-shadow: 0 4px 8px 0 rgba(0,0,0,0.2),0 6px 20px 0 rgba(0,0,0,0.19);
        -webkit-animation-name: animatetop;
        -webkit-animation-duration: 0.4s;
        animation-name: animatetop;
        animation-duration: 0.4s
    }

     @media screen and (max-width: 640px) {
        .modal-content {
            width: 90vw;
            height: 70vh;
        }
    }
    
    /* Add Animation */
    @-webkit-keyframes animatetop {
        from {top:-300px; opacity:0} 
        to {top:0; opacity:1}
    }
    
    @keyframes animatetop {
        from {top:-300px; opacity:0}
        to {top:0; opacity:1}
    }
    
    /* The Close Button */
    .closeModal {
        color: white;
        float: right;
        font-size: 28px;
        font-weight: bold;
    }
    
    .closeModal:hover,
    .closeModal:focus {
        color: #000;
        text-decoration: none;
        cursor: pointer;
    }
    
    .modal-header {
        padding: 10px 16px;
        background-color: #FBBD08;
        color: white;
    }
    
    .modal-body {padding: 40px 20px;}
    

</style>
