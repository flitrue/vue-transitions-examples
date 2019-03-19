<template>
  <div>
    <svg width="200" height="200">
      <polygon :points="points"></polygon>
      <rect width="200" height="200" stroke="#000" fill="transparent"></rect>
    </svg>
  </div>
</template>

<script>
export default {
  name: "TransLate",
  data() {
    return {
      points: "100,43 34,87 54,90",
      x: [100, 34, 54],
      y: [43, 87, 90],
      total: 3,
    }
  },
  watch: {
    x: function(){
      TweenLite.to(this.$data, 1, {points: this.genPoints()})
    }
  },
  methods: {
    genPoints() {
      let points = []
      for(let i=0;i<this.total;i++){
        points.push(this.x[i]+","+this.y[i])
      }
      return points.join(' ');
    },
    genX() {
      let _this = this
      this.x = this.x.map(function () {
      	return _this.random(1, 200)
      })
    },
    genY() {
      let _this = this
      this.y = this.y.map(function () {
      	return _this.random(1, 200)
      })
    },
    run() {
      setInterval(()=>{
        this.genX();
        this.genY();
      }, 1000)
    },
    random(lower, upper) {
	    return Math.ceil(Math.random() * (upper - lower)) + lower;
    }
  },
  mounted() {
    this.run();
  }
}
</script>

<style scoped>
circle{
  fill: transparent;
  stroke: #000;
}
polygon{
  fill: aqua;
}
</style>
