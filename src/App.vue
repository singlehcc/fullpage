<template>
  <div id="app">
    <Fullpage v-for="(item,index) in 3" :currentPage='currentPage'>
      <div>{{'hh'+index}}</div>
    </Fullpage>
  </div>
</template>

<script>
import HelloWorld from './components/HelloWorld'
import Fullpage from './components/Fullpage'

function GetSlideAngle(dx, dy) {
      return Math.atan2(dy, dx) * 180 / Math.PI
    }
  
function GetSlideDirection(startX, startY, endX, endY) {
  var dy = startY - endY;
  var dx = endX - startX;
  var result = 0;
  if(Math.abs(dx) < 50 && Math.abs(dy) < 50) {
    return result
  }
  var angle = GetSlideAngle(dx, dy);
   if(angle >= -45 && angle < 45) {  
        result = 4;  
    }else if (angle >= 45 && angle < 135) {  
        result = 1;  //向上
    }else if (angle >= -135 && angle < -45) {  
        result = 2;  // 向下
    }  
    else if ((angle >= 135 && angle <= 180) || (angle >= -180 && angle < -135)) {  
        result = 3;  
    } 
  return result
}
export default {
  name: 'App',
  data () {
    return {
      currentPage: 1,
      options: [{
        // the color of background
        background: 'rgba(229, 199, 46, 1)',
        // the color of text
        color: '#fff',
        // is content center
        isCenter: true,
        // the function before page show
      }, {
        background: 'rgba(79, 204, 76, 1)',
        color: '#fff',
        isCenter: true,
      }, {
        background: 'rgba(233, 84, 84, 1)',
        color: '#fff',
        isCenter: true,
      }, {
        background: 'rgba(46, 153, 229, 1)',
        color: '#fff',
        isCenter: true,
      }],
      startX: '',
      startY: '',
      endX: '',
      endY: ''
    }
  },
  components: {
    HelloWorld,
    Fullpage
  },
  created () {
    document.addEventListener('touchstart', (ev) => { 
      this.startX = ev.touches[0].pageX;
      this.startY = ev.touches[0].pageY
    })
    document.addEventListener('touchend', (ev) => {
      this.endX = ev.changedTouches[0].pageX;
      this.endY = ev.changedTouches[0].pageY;
      const  direction = GetSlideDirection(this.startX, this.startY, this.endX, this.endY);
      if (direction === 1) {
        console.log(this.currentPage)
        if (this.currentPage < this.options.length-1) {
          this.currentPage ++
        }
        //向上
      } else if ( direction ===2 ){
        
        //向下
        if (this.currentPage >= 2) {
          this.currentPage --
        }
        
      }
    })
    
  },
  mounted() {
    this.$children.forEach((child, index) => {
      // 动态设置各个page内的options
      if (child.option === null) {
        let childOption = this.options[index];
        this.$set(childOption, 'index', index + 1);
        console.log(childOption)
        child.option = childOption;
      }
    });
  }
}
</script>

<style>
html, body {
  width: 100%;
  height: 100%;
}
#app {
  width: 100%;
  height: 100%;
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
</style>
