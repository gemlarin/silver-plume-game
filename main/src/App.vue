  <template>
  <div id="app">
      <div class="row main-wrapper-app no-gutters">
        <div class="col-md-4">
          <appPlayer v-show='isOpen'></appPlayer>
          <button class="btn btn-primary" @click="purchase()">Buy Stuff</button>
          <button class="btn btn-primary" @click="pay()">Pay yourself</button>
          <button class="btn btn-primary" @click='toggle()'>Open/Close</button>
        </div>
        <div class="col-md-8">
          <appDialog></appDialog>
        </div>
      </div>
  </div>
</template>

<script>
import { lifeBus } from './main.js';
import { manaBus } from './main.js';
import { healBus } from './main.js';
import { itemBus } from './main.js';
import { slayBus } from './main.js';
import Player      from './components/player/Player'
import DialogBox   from './components/dialog/Dialogbox'
import './assets/player-knight.svg'

export default {
  name: 'app',
 
  data () {
    return {
      isOpen:true,
      playerdamage:4,
      playermana:2,
      heal:5
    }
  },
  components: {
    appPlayer: Player,
    appDialog: DialogBox,
  },
  methods:{
    //temporary: for testing. remove after dev. using to hide component during build.
     toggle(){
        this.isOpen = !this.isOpen
      },
      //temporary: for testing. remove after the manaBus emitters are configured
     purchase() {
       this.$store.commit("updateGold", { value:3, type:"buy"});
     },
      //temporary: for testing. remove after the healBus emitters are configured
     pay() {
       this.$store.commit("updateGold", { value:5, type:"pay"});
     }
  },
  computed:{
  
  },
  created() {
    //itemBus receives its data from the individual dialog pages if the item defined there is found via search
    //If anyone ever reads this and asks WTF? I put it here because I needed a common parent of the dialog pages to pick up the data coming down the pipe and there were some weird conflicts when trying to access $store from inside the $ON of another vue. IDK. THis fixed it, so whatever.
    itemBus.$on('newItem', (data) =>{
      //add the items object you found to the $store - stores them to an array
      this.$store.commit('itemFound', {'item':data.item, 'itemID':data.itemID});
    });
  },
  metaInfo: {
      title: 'The Silver Plume', // set a main global title
      titleTemplate: '%s - Choose Your Path', // set your default global subtitle
      htmlAttrs: {
        lang: 'en',
        amp: undefined // "amp" has no value
      }
    }
}
</script>

<style lang="scss">

$text-color-light:lightgrey;


#app {
   @import url("https://fonts.googleapis.com/css?family=Kurale|Zilla+Slab");
  font-family: 'Kurale', serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: $text-color-light;
  overflow-x:hidden;
  //background-image: url("./assets/pattern-tan-bg.jpg");
  //background-repeat: repeat;
  width:100vw;
  height:100vh;
}
html{
  font-size: 62.5%;
}
.card{
  border-radius:0;
}
.main-wrapper-app{
  max-height:650px;
}
.wrap.dialog{
  padding: 30px 20px 20px 15px;
  margin: 10px 20px 0 0;
  position:relative;
  //background-color: #f3f2f0;
  //  border-radius: 15px;
}
p{
  font-size:1.8rem;
  display:block;
}
h1, h2 {
  font-weight: normal;
  margin-bottom:20px;
}
h1{
  font-size:3em;
}
h2{
  font-size:2.4em;
  color:#878881;
}
h3{
  color:#ab4646;
  font-size:1.7em;
  font-weight:700;
  margin-bottom:15px;
  margin-top:25px;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

a {
  color: #42b983;
}

.highlight{
  font-weight:bold;
  color:#ab4646 !important;
}

.disabled{
  display:none;
}

#options-nav{
  margin-top:40px;
}

#options-nav.disabled{
  opacity:.6;
  pointer-events: none;
}

.font--mod-bold{
  font-weight:bold;
}

.wrap.dialog{
  background-color:white;
}

.btn-primary:hover{
  color: #828282;
  border-color: #dedede;
}

button, html [type="button"], [type="reset"], [type="submit"]{
      background-color: white;
      border-color: #dedede;
      color: #828282;
      font-size: 16px;
      font-weight: 700;
      margin:0 0 10px 0;
      outline: none;
      margin-bottom:10px;
      //button border fix for safari
      border-top-style: normal;
      border-right-style: normal;
      border-bottom-style: normal;
      border-left-style: normal;
  }

  .btn{
    white-space: normal;
  }

  .btn-primary:focus, .btn-primary.focus {
      box-shadow:none;
  }

  .btn-primary:not(:disabled):not(.disabled):active:focus, .btn-primary:not(:disabled):not(.disabled).active:focus, .show > .btn-primary.dropdown-toggle:focus {
      box-shadow: none;
      border-color:#dedede
  }

  .btn-primary:not(:disabled):not(.disabled):active, .btn-primary:not(:disabled):not(.disabled).active, .show > .btn-primary.dropdown-toggle {
      outline: none;
      border-color: #dedede !important;
      /* Permalink - use to edit and share this gradient: http://colorzilla.com/gradient-editor/#ffffff+0,eaeaea+10,ffffff+91,e2e2e2+100 */
      box-shadow: none;
      color:#828282;
      border-color: #dedede !important;
      background: #ffffff; /* Old browsers */
      background: -moz-linear-gradient(top, #ffffff 0%, #eaeaea 10%, #ffffff 91%, #e2e2e2 100%); /* FF3.6-15 */
      background: -webkit-linear-gradient(top, #ffffff 0%,#eaeaea 10%,#ffffff 91%,#e2e2e2 100%); /* Chrome10-25,Safari5.1-6 */
      background: linear-gradient(to bottom, #ffffff 0%,#eaeaea 10%,#ffffff 91%,#e2e2e2 100%); /* W3C, IE10+, FF16+, Chrome26+, Opera12+, Safari7+ */
      filter: progid:DXImageTransform.Microsoft.gradient( startColorstr='#ffffff', endColorstr='#e2e2e2',GradientType=0 ); /* IE6-9 */
    }

  button{
    border-style: solid;
    border-width:1px;
  }

  button:hover, html [type="button"]:hover, [type="reset"]:hover, [type="submit"]:hover{
    /* Permalink - use to edit and share this gradient: http://colorzilla.com/gradient-editor/#ffffff+0,f7f7f7+10,ffffff+91,efefef+100 */
      background: #ffffff; /* Old browsers */
      background: -moz-linear-gradient(top, #ffffff 0%, #f7f7f7 10%, #ffffff 91%, #efefef 100%); /* FF3.6-15 */
      background: -webkit-linear-gradient(top, #ffffff 0%,#f7f7f7 10%,#ffffff 91%,#efefef 100%); /* Chrome10-25,Safari5.1-6 */
      background: linear-gradient(to bottom, #ffffff 0%,#f7f7f7 10%,#ffffff 91%,#efefef 100%); /* W3C, IE10+, FF16+, Chrome26+, Opera12+, Safari7+ */
      filter: progid:DXImageTransform.Microsoft.gradient( startColorstr='#ffffff', endColorstr='#efefef',GradientType=0 ); /* IE6-9 */
    }

    button.btn--alt-red{
      background: #a73434;
      border-color: #dedede;
      color:#fff;
      padding-top:7px;
      padding-bottom:7px;
  }


    button.btn--alt-red:hover{
      background: #b13c3c;
      border-color: #dedede;
      color:#fff;
      text-shadow: 1px 1px #4b4b4b;
      
    }

    button.btn--alt-red:not(:disabled):not(.disabled):active, button.btn--alt-red:not(:disabled):not(.disabled).active, .show > button.btn--alt-red.dropdown-toggle {
      color:#fff;
    }

     button.btn--alt-red:not(:disabled):not(.disabled):active, button.btn--alt-red:not(:disabled):not(.disabled).active, .show > button.btn--alt-red.dropdown-toggle {
      background: #ca4242;
      border-color: #dedede;
      text-shadow: 1px 1px #4b4b4b;
  }

  button.btn-close{
    width:auto;
    padding: 3px 20px;
    background-color:#333;
    background:#333;
    color:#fff;
    position:absolute;
    right:15px;
    top:70px
  }
  button.btn-close:hover{
    background:rgb(85, 85, 85);
  }
  button.btn-close:active{
    background:rgb(85, 85, 85);
  }

  //animation stuff

 


</style>
