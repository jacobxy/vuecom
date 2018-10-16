<template>
    <div class="base">
        <div @click="click(-4)" class="elem" >{{this.left}}</div>
        <div @click="click(-3)" class="elem">{{this.leftone}}</div>
        <div @click="click(1)" class="elem" :class="{isselect:cur==1}">1</div>
        <div  class="ignore" v-if="showIgnore && cur > 3">...</div>
        <div @click="click(v)" class="elem" v-for="(v,index) in showElems" :key="index" :class="{isselect:cur==v}">{{v}}</div>
        <div  class="ignore" v-if="showIgnore && cur < length-2">...</div>
        <div @click="click(length)" class="elem" :class="{isselect:cur==this.length}">{{this.length}}</div>
        <div @click="click(-2)" class="elem">{{this.rightone}}</div>
        <div @click="click(-1)" class="elem">{{this.right}}</div>
    </div>
</template>

<script>
export default {
    name:'jump-page',
    props:{
        length:{
            type:Number,
            default:0,
        },
    },
    data:function(){
        return {
            left:"<<",
            right:">>",
            leftone:"<",
            rightone:">",
            cur:1,
        }
    },
    computed:{
        showIgnore:function(){
            return this.length > 10;
        },
        showElems:function(){
            var res = []
            for(var i = 2; i <= this.length-1;i++){
                res.push(i)
            }
            if(!this.showIgnore){
                return res
            }
            if(this.cur < 4){
                return [2,3,4,5]
            }
            if(this.cur > this.length - 3){
                return [this.length-4,this.length-3,this.length-2,this.length-1]
            }
            return [this.cur -1, this.cur, this.cur + 1]
        }
    },
    methods:{
        show:function(v){

        },
        click:function(val){
            var old = this.cur
            switch(val) {
                case -4:
                    this.cur = 1;
                    break;
                case -3:
                    if(this.cur > 1){
                        this.cur --;
                    }
                    break
                case -2:
                    if(this.cur < this.length){
                        this.cur ++ 
                    }
                    break
                case -1:
                    this.cur = this.length
                    break
                default:{
                    if(val > 0 && val <= this.length){
                        this.cur = val
                    }
                }
            }
            if(old != this.cur){
               this.$emit('show',this.cur-1) 
            }

        }
    }
}
</script>

<style scoped>
    .base {
        margin-left: auto;
        margin-right: auto;
        width: 100%;
        display: flex;
        justify-content: center;
    }
    .elem{
        border:2px solid gray;
        padding: 1px;
        margin: 0 5px;
        width: 3%;
        text-align: center;
        color: gray;
        font-size: 1.5em;
        font-weight: bold;
    }

    .ignore {
        vertical-align: bottom;
        font-size: 2em;
        color: gray;
        padding: 1px;
        margin: 0 5px;
        width: 3%;
        text-align: center;
        font-size: 1.5em;
        font-weight: bold;
    }

    .isselect {
        background: white;
    }
</style>
