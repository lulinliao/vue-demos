<template>
  <div id="transition-effect" >
    <h1 title="Click Me !" @click="showEffect" :class="effect ? 'styleEffect' : ''">Transition Effect</h1>
    <div id="transition-effect-box" v-if="effect">
      <div id="demo">
        <button v-on:click="show = !show">
          Toggle
        </button>
        <transition name="fade">
          <p v-if="show">hello</p>
        </transition>
      </div>

      <div id="example-1">
        <button v-on:click="show1 = !show1">
          Toggle render
        </button>
        <transition name="slide-fade">
          <p v-if="show1">hello</p>
        </transition>
      </div>

      <div id="example-2">
        <button @click="show2 = !show2">
          Toggle show
        </button>
        <transition name="bounce">
          <p v-if="show2">Look at me!</p>
        </transition>
      </div>

      <div id="example-3">
        <button @click="show3 = !show3">
          Toggle render
        </button>
        <transition
          name="custom-classes-transition"
          enter-active-class="animated tada"
          leave-active-class="animated bounceOutRight"
        >
          <p v-if="show3">Animate.css</p>
        </transition>
      </div>

      <div id="example-4">
        <button @click="show4 = !show4">
          Toggle
        </button>
        <transition
          v-on:before-enter="beforeEnter"
          v-on:enter="enter"
          v-on:leave="leave"
          v-bind:css="false"
        >
          <p v-if="show4">
            Demo
          </p>
        </transition>
      </div>

      <div id="example-5">
        <input type="radio" name="radio" @click="toggleOutIn"/>
        <input type="radio" name="radio" @click="toggleOutIn"/>
        <transition name="component-fade" mode="out-in">
          <component :is="viewOutIn"></component>
        </transition>
      </div>

      <div id="example-6">
        <input type="radio" name="radio" @click="toggle"/>
        <input type="radio" name="radio" @click="toggle"/>
        <transition name="component-fade">
          <component :is="view"></component>
        </transition>
      </div>

      <div id="example-7">
        <input type="radio" name="radio" @click="toggleInOut"/>
        <input type="radio" name="radio" @click="toggleInOut"/>
        <transition name="component-fade" mode="in-out">
          <component :is="viewInOut"></component>
        </transition>
      </div>
      
      <div id="example-8">
        <button @click="shuffle1">Shuffle</button>
        <button @click="add">Add</button>
        <button @click="remove">Remove</button>
        <transition-group name="list" tag="p">
          <span v-for="item in items1" v-bind:key="item" class="list-item">
            {{ item }}
          </span>
        </transition-group>
      </div>

      <div id="example-9">
        <button @click="shuffle2">shuffle</button>
        <transition-group name="flip-list" tag="ul" class="shuffle">
          <li v-for="item in items2" :key="item">
            {{ item }}
          </li>
        </transition-group>
      </div>

      <div id="example-10">
        <input type="text" name="example-10" v-model="query" />
        <transition-group name="staggered-fade" tag="ul" v-on:before-enter="beforeEnter10" v-on:enter="enter10" v-on:leave="leave10" v-bind:css="false" class="stagger">
          <li v-for=" (item,index) in computedList" v-bind:key="item.msg" v-bind:data-index="index" class="stagger-li">
            {{ item.msg }}
          </li>
        </transition-group>
      </div>

      <div id="example-11">
        Fade in: <input type="range" v-model="fadeInDuration" min="0" :max="maxFadeDuration" />
        Fade out: <input type="range" v-model="fadeOutDuration" min="0" :max="maxFadeDuration" />
        <transition :css="false" @before-enter="beforeEnter11" @enter="enter11" @leave="leave11">
          <p v-if="show11">hello</p>
        </transition>
        <button v-if="stop11" v-on:click="stop11 = false; show11 = false">Start animating</button>
        <button v-else v-on:click="stop11 = true">Stop it!</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'hello',
  data () {
    return {
      effect: false,
      show: true,
      show1: true,
      show2: true,
      show3: true,
      show4: false,
      show5: true,
      view: 'v-a',
      viewOutIn: 'v-a',
      viewInOut: 'v-a',
      items1: [1,2,3,4,5,6,7,8,9],
      items2: [1,2,3,4,5,6,7,8,9],
      nextNum: 10,
      query: '',
      list: [
        { msg: 'lulin' },
        { msg: 'liao' },
        { msg: 'wangxiaoxian' },
        { msg: 'wangna' },
        { msg: 'liulu' },
        { msg: 'dingchen' },
        { msg: 'yamei' },
        { msg: 'yangjing' },
        { msg: 'luansheng' },
        { msg: 'luwei' }
      ],
      show11: true,
      stop11: true,
      fadeInDuration: 1000,
      fadeOutDuration: 1000,
      maxFadeDuration: 1500
    }
  },
  computed: {
    computedList: function() {
      var vm = this;
      return this.list.filter(function(item) {
        return item.msg.toLowerCase().indexOf(vm.query.toLowerCase()) !== -1
      })
    }
  },
  methods: {
    showEffect: function() {
      this.effect = !this.effect;
    },
    beforeEnter: function(el) {
      el.style.opacity = 0;
      el.style.transformOrigin = 'left'
    },
    beforeEnter10: function(el) {
      el.style.opacity = 0;
      el.style.height = 0;
    },
    beforeEnter11: function(el) {
      el.style.opacity = 0;
    },
    enter: function(el,done) {
      Velocity(el, { opacity: 1, fontSize: '1.4em' }, { duration: 300})
      Velocity(el, { fontSize: '1em' }, { complete: done})
    },
    enter10: function(el,done) {
      var delay = el.dataset.index * 200 ;
      setTimeout(function() {
        Velocity(el, { opacity: 1, height: '1.6rem' }, { complete: done });
      }, delay)
    },
    enter11: function(el,done) {
      var vm = this;
      Velocity(el, { opacity: 1 }, { duration: this.fadeInDuration, complete: function() { 
        done() 
        if (!vm.stop11) { vm.show11 = false }
        }
      })
    },
    leave: function(el,done) {
      Velocity(el, { translateX: '15px', rotateZ: '50deg' }, { duration: 600 })
      Velocity(el, { rotateZ: '100deg' }, { loop: 2 })
      Velocity(el, {
        rotateZ: '15deg',
        translateY: '300px',
        translateX: '100px',
        opacity: 0
      }, { complete: done })
    },
    leave10: function(el,done) {
      var delay = el.dataset.index * 200;
      setTimeout(function() {
        Velocity(el, { opacity: 0, height: 0 }, { complete: done });
      }, delay)
    },
    leave11: function(el,done) {
      var vm = this;
      Velocity(el, { opacity: 0 }, { duration: this.fadeOutDuration, complete: function() {
        done()
        vm.show11 = true;
      }})
    },
    toggle: function() {
      if (this.view == 'v-a') {
        this.view = 'v-b'
      } else {
        this.view = 'v-a'
      }
    },
    toggleOutIn: function() {
      if (this.viewOutIn == 'v-a') {
        this.viewOutIn = 'v-b'
      } else {
        this.viewOutIn = 'v-a'
      }
    },
    toggleInOut: function() {
      if (this.viewInOut == 'v-a') {
        this.viewInOut = 'v-b'
      } else {
        this.viewInOut = 'v-a'
      }
    },
    add: function() {
      this.items1.splice(this.randomIndex(), 0, this.nextNum++);
    },
    remove: function() {
      this.items1.splice(this.randomIndex(), 1);
    },
    randomIndex: function() {
      return Math.floor(Math.random() * this.items1.length)
    },
    shuffle1: function() {
      this.items1 = _.shuffle(this.items1);
    },
    shuffle2: function() {
      this.items2 = _.shuffle(this.items2);
    }
  },
  components: {
    'v-a': {
      template: '<div>Component A</div>'
    },
    'v-b': {
      template: '<div>Component B</div>'
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

h1:hover {
  font-style: italic;
  font-size: 40px;
  color: grey;
}

.styleEffect {
  color: cadetblue;
  background: black;
  width: 50%;
  height: 100%;
  margin: 25px auto;
  padding: 10px;
  font-size: 40px;
  border-radius: 50%;
  box-shadow: 10px 10px 5px darkslategrey;
}

.fade-enter-active, .fade-leave-active {
  transition: opacity .9s;
}

.fade-enter, .fade-leave-to /* .fade-leave-active in below version 2.1.8 */ {
  opacity: 0;
  transform: translateX(20px);
}

.slide-fade-enter-active {
  transition: all .3s ease;
}

.slide-fade-leave-active {
  transition: all .8s cubic-bezier(1.0, 0.5, 0.8, 1.0);
}

.slide-fade-enter,.slide-fade-leave-to {
  opacity: 0;
  transform: translateX(10px);
}

.bounce-enter-active {
  animation: bounce-in .5s;
}

.bounce-leave-active {
  animation: bounce-in .5s reverse;
}

@keyframes bounce-in {
  0% {
    transform: scale(0);
  }
  50% {
    transform: scale(1.5);
  }
  100% {
    transform: scale(1);
  }
}

.component-fade-enter-active, .component-fade-leave-active {
  transition: opacity .3s ease;
}

.component-fade-enter, .component-fade-leave-to
/* .component-fade-leave-active for below version 2.1.8 */ {
  opacity: 0;
}

.list-item {
  display: inline-block;
  margin-right: 10px;
}

.list-enter-active,.list-leave-active {
  transition: all 1s;
}

.list-enter,.list-leave-to {
  opacity: 0;
  transform: translateY(50px);
}

.flip-list-move {
  transition: transform 1s;
}

.shuffle {
  width: 5%;
  margin: 20px auto;
}

.list-move {
  transition: 2s;
}

.stagger {
  width: 10%;
  margin: 20px auto;
}

.stagger-li {
  margin: 10px;
}

</style>
