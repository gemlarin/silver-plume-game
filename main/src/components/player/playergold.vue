<template>
    <div class="wrap">
        <img class="food" :src="require('./../../assets/player-coin.svg')" />
        <label>{{ gold }}</label>
    </div>
</template>

<script>

export default {
  name: 'playergold',
  data () {
    return {
    
    }
  },
  computed:{
      gold(){
          return this.$store.state.gold;
      }
  },
  methods:{
      
  }
}
</script>
<style scoped>
    img{
        height:21px;
        width:auto;
    }
    label{
        font-size:14px;
        position:absolute;
        top:0;
        left:25px;
    }
    .wrap{
        position:absolute;
        left:75px;
    }
</style>

