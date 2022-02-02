<template>
    <div>
        <h1> I am from Home component </h1>
        <tasklist/>
    </div>
</template>


<script>
import Tasklist from './tasklist.vue'
    export default {
        name:"Home",
        components:{
            Tasklist
                
        }
    }
</script>