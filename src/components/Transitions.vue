<template>
  <div>
    <div id="animated-number-demo">
      <input v-model.number="number" type="number" step="20">
      <p>{{ animatedNumber }}</p>
    </div>

    <div>
      <div>
        <svg width="200" height="200">
          <polygon :points="points"></polygon>
          <circle cx="100" cy="100" r="90"></circle>
        </svg>
      </div>
      <label>Sides: {{ sides }}</label>
      <input type="range" min="3" max="500" v-model.number="sides">
      <label>Minimum Radius: {{ minRadius }}%</label>
      <input type="range" min="0" max="90" v-model.number="minRadius">
      <label>Update Interval: {{ updateInterval }} milliseconds</label>
      <input type="range" min="10" max="2000" v-model.number="updateInterval">
    </div>
  </div>
</template>

<script>
export default {
  name: "Transitions",
  data() {
    var defaultSides = 10;
    var stats = Array.apply(null, { length: defaultSides }).map(function() {
      return 100;
    });
    return {
      number: 0,
      num: {
        tweenedNumber: 0
      },
      stats: stats,
      points: generatePoints(stats),
      sides: defaultSides,
      minRadius: 50,
      interval: null,
      updateInterval: 500
    };
  },
  computed: {
    animatedNumber: function() {
      return this.num.tweenedNumber.toFixed(0);
    }
  },
  watch: {
    number: function(newValue) {
      TweenLite.to(this.num, 0.5, { tweenedNumber: newValue });
    },
    sides: function(newSides, oldSides) {
      var sidesDifference = newSides - oldSides;
      if (sidesDifference > 0) {
        for (var i = 1; i <= sidesDifference; i++) {
          this.stats.push(this.newRandomValue());
        }
      } else {
        var absoluteSidesDifference = Math.abs(sidesDifference);
        for (var i = 1; i <= absoluteSidesDifference; i++) {
          this.stats.shift();
        }
      }
    },
    stats: function(newStats) {
      TweenLite.to(this.$data, this.updateInterval / 1000, {
        points: generatePoints(newStats)
      });
    },
    updateInterval: function() {
      this.resetInterval();
    }
  },
  methods: {
    randomizeStats: function() {
      var vm = this;
      this.stats = this.stats.map(function() {
        return vm.newRandomValue();
      });
    },
    newRandomValue: function() {
      return Math.ceil(this.minRadius + Math.random() * (100 - this.minRadius));
    },
    resetInterval: function() {
      var vm = this;
      clearInterval(this.interval);
      this.randomizeStats();
      this.interval = setInterval(function() {
        vm.randomizeStats();
      }, this.updateInterval);
    }
  },
  mounted: function() {
    this.resetInterval();
  }
};

function valueToPoint(value, index, total) {
  var x = 0;
  var y = -value * 0.9;
  var angle = ((Math.PI * 2) / total) * index;
  var cos = Math.cos(angle);
  var sin = Math.sin(angle);
  var tx = x * cos - y * sin + 100;
  var ty = x * sin + y * cos + 100;
  return { x: tx, y: ty };
}

function generatePoints(stats) {
  var total = stats.length;
  return stats
    .map(function(stat, index) {
      var point = valueToPoint(stat, index, total);
      return point.x + "," + point.y;
    })
    .join(" ");
}
</script>

<style scoped>
polygon {
  fill: #41b883;
}
circle {
  fill: transparent;
  stroke: #35495e;
}
input[type="range"] {
  display: block;
  width: 100%;
  margin-bottom: 15px;
}
</style>
