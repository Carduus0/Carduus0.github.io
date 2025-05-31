<template>
  <canvas
    ref="fireworksCanvas"
    :style="fireworksStyle"
    width="window.innerWidth"
    height="window.innerHeight"
  ></canvas>
</template>

<script setup>
import { ref, onMounted, onUnmounted, watch } from "vue";

const fireworksCanvas = ref(null);
const gl = ref(null);
const props = defineProps({
  fireworksStyle: {
    type: Object,
    default: () => ({}),
  },
  isVisible: {
    // Добавляем prop для видимости
    type: Boolean,
    default: false,
  },
});

onMounted(() => {
  const c = fireworksCanvas.value;
  const localGl = c.getContext("webgl", { preserveDrawingBuffer: true });
  if (!localGl) {
    console.error("WebGL не поддерживается");
    return;
  }
  gl.value = localGl;

  const w = (c.width = window.innerWidth);
  const h = (c.height = window.innerHeight);

  const webgl = {};
  const opts = {
    projectileAlpha: 0.8,
    projectileLineWidth: 1.3,
    fireworkAngleSpan: 0.5,
    baseFireworkVel: 3,
    addedFireworkVel: 3,
    gravity: 0.03,
    lowVelBoundary: -0.2,
    xFriction: 0.995,
    baseShardVel: 1,
    addedShardVel: 0.2,
    fireworks: 1000,
    baseShardsParFirework: 10,
    addedShardsParFirework: 10,
    shardFireworkVelMultiplier: 0.3,
    initHueMultiplier: 1 / 360,
    runHueAdder: 0.1 / 360,
  };

  // webgl.vertexShaderSource = `
  //   uniform int u_mode;
  //   uniform vec2 u_res;
  //   attribute vec4 a_data;
  //   varying vec4 v_color;

  //   vec3 h2rgb( float h ){
  //     vec3 color;
  //     float r = abs(mod(h * 6.0 + 0.0, 6.0) - 3.0) - 1.0;
  //     float g = abs(mod(h * 6.0 + 4.0, 6.0) - 3.0) - 1.0;
  //     float b = abs(mod(h * 6.0 + 2.0, 6.0) - 3.0) - 1.0;
  //     color.r = clamp(r, 0.0, 1.0);
  //     color.g = clamp(g, 0.0, 1.0);
  //     color.b = clamp(b, 0.0, 1.0);
  //     return color;
  //   }
  //   void clear(){
  //     gl_Position = vec4( a_data.xy, 0.0, 1.0 );
  //     v_color = vec4( 0.0, 0.0, 0.0, a_data.w );
  //   }
  //   void draw(){
  //     gl_Position = vec4( vec2( 1.0, -1.0 ) * ( ( a_data.xy / u_res ) * 2.0 - 1.0 ), 0.0, 1.0 );
  //     v_color = vec4( h2rgb( a_data.z ), a_data.w );
  //   }
  //   void main(){
  //     if( u_mode == 0 )
  //       draw();
  //     else
  //       clear();
  //   }
  // `;

  webgl.vertexShaderSource = `
  attribute vec4 a_data;
  void main() {
    gl_Position = vec4(a_data.xy, 0.0, 1.0);
  }
`;
  // webgl.fragmentShaderSource = `
  //     precision mediump float;
  //     varying vec4 v_color;

  //     void main(){
  //       gl_FragColor = v_color;
  //     }
  //   `;
  webgl.fragmentShaderSource = `
  void main() {
    gl_FragColor = vec4(1.0, 0.0, 0.0, 1.0); // Red
  }
`;

  webgl.vertexShader = gl.value.createShader(gl.value.VERTEX_SHADER);
  gl.value.shaderSource(webgl.vertexShader, webgl.vertexShaderSource);
  gl.value.compileShader(webgl.vertexShader);
  if (
    !gl.value.getShaderParameter(webgl.vertexShader, gl.value.COMPILE_STATUS)
  ) {
    console.error(
      "Ошибка компиляции вершинного шейдера:",
      gl.value.getShaderInfoLog(webgl.vertexShader)
    );
    gl.value.deleteShader(webgl.vertexShader);
    return;
  }

  webgl.fragmentShader = gl.value.createShader(gl.value.FRAGMENT_SHADER);
  gl.value.shaderSource(webgl.fragmentShader, webgl.fragmentShaderSource);
  gl.value.compileShader(webgl.fragmentShader);
  if (
    !gl.value.getShaderParameter(webgl.fragmentShader, gl.value.COMPILE_STATUS)
  ) {
    console.error(
      "Ошибка компиляции фрагментного шейдера:",
      gl.value.getShaderInfoLog(webgl.fragmentShader)
    );
    gl.value.deleteShader(webgl.fragmentShader);
    gl.value.deleteShader(webgl.vertexShader);
    return;
  }

  webgl.shaderProgram = gl.value.createProgram();
  gl.value.attachShader(webgl.shaderProgram, webgl.vertexShader);
  gl.value.attachShader(webgl.shaderProgram, webgl.fragmentShader);
  gl.value.linkProgram(webgl.shaderProgram);
  if (
    !gl.value.getProgramParameter(webgl.shaderProgram, gl.value.LINK_STATUS)
  ) {
    console.error(
      "Ошибка связывания шейдерной программы:",
      gl.value.getProgramInfoLog(webgl.shaderProgram)
    );
    gl.value.deleteProgram(webgl.shaderProgram);
    gl.value.deleteShader(webgl.vertexShader);
    gl.value.deleteShader(webgl.fragmentShader);
    return;
  }
  gl.value.useProgram(webgl.shaderProgram);

  webgl.dataAttribLoc = gl.value.getAttribLocation(
    webgl.shaderProgram,
    "a_data"
  );
  webgl.dataBuffer = gl.value.createBuffer();

  gl.value.enableVertexAttribArray(webgl.dataAttribLoc);
  gl.value.bindBuffer(gl.value.ARRAY_BUFFER, webgl.dataBuffer);
  gl.value.vertexAttribPointer(
    webgl.dataAttribLoc,
    4,
    gl.value.FLOAT,
    false,
    0,
    0
  );

  webgl.resUniformLoc = gl.value.getUniformLocation(
    webgl.shaderProgram,
    "u_res"
  );
  webgl.modeUniformLoc = gl.value.getUniformLocation(
    webgl.shaderProgram,
    "u_mode"
  );

  gl.value.viewport(0, 0, w, h);
  gl.value.uniform2f(webgl.resUniformLoc, w, h);

  gl.value.blendFunc(gl.value.SRC_ALPHA, gl.value.ONE_MINUS_SRC_ALPHA);
  gl.value.enable(gl.value.BLEND);

  gl.value.lineWidth(opts.projectileLineWidth);

  webgl.data = [];

  var fireworks = [];
  var tick = 0;
  var sins = [];
  var coss = [];
  var maxShardsParFirework =
    opts.baseShardsParFirework + opts.addedShardsParFirework;
  var tau = 6.283185307179586476925286766559;

  for (var i = 0; i < maxShardsParFirework; ++i) {
    sins[i] = Math.sin((tau * i) / maxShardsParFirework);
    coss[i] = Math.cos((tau * i) / maxShardsParFirework);
  }

  //const showFireworks = ref(false);

  function Firework() {
    this.reset();
    this.shards = [];
    for (var i = 0; i < maxShardsParFirework; ++i)
      this.shards.push(new Shard(this));
    console.log("Firework created");
  }
  Firework.prototype.reset = function () {
    var angle = -Math.PI / 2 + (Math.random() - 0.5) * opts.fireworkAngleSpan;
    var vel = opts.baseFireworkVel + opts.addedFireworkVel * Math.random();

    this.mode = 0;
    this.vx = vel * Math.cos(angle);
    this.vy = vel * Math.sin(angle);

    this.x = Math.random() * w;
    this.y = h;

    this.hue = tick * opts.initHueMultiplier;
    console.log("Firework reset", this);
  };
  Firework.prototype.step = function () {
    if (this.mode === 0) {
      var ph = this.hue;
      var px = this.x;
      var py = this.y;

      this.hue += opts.runHueAdder;

      this.x += this.vx *= opts.xFriction;
      this.y += this.vy += opts.gravity;

      webgl.data.push(
        px,
        py,
        ph,
        opts.projectileAlpha * 0.2,
        this.x,
        this.y,
        this.hue,
        opts.projectileAlpha * 0.2
      );

      if (this.vy >= opts.lowVelBoundary) {
        this.mode = 1;

        this.shardAmount =
          (opts.baseShardsParFirework +
            opts.addedShardsParFirework * Math.random()) |
          0;

        var baseAngle = Math.random() * tau;
        var x = Math.cos(baseAngle);
        var y = Math.sin(baseAngle);
        var sin = sins[this.shardAmount];
        var cos = coss[this.shardAmount];

        for (var i = 0; i < this.shardAmount; ++i) {
          var vel = opts.baseShardVel + opts.addedShardVel * Math.random();
          this.shards[i].reset(x * vel, y * vel);
          var X = x;
          x = x * cos - y * sin;
          y = y * cos + X * sin;
        }
      }
    } else if (this.mode === 1) {
      this.ph = this.hue;
      this.hue += opts.runHueAdder;

      var allDead = true;
      for (var i = 0; i < this.shardAmount; ++i) {
        var shard = this.shards[i];
        if (!shard.dead) {
          shard.step();
          allDead = false;
        }
      }

      if (allDead) this.reset();
    }
  };
  function Shard(parent) {
    this.parent = parent;
  }
  Shard.prototype.reset = function (vx, vy) {
    this.x = this.parent.x;
    this.y = this.parent.y;
    this.vx = this.parent.vx * opts.shardFireworkVelMultiplier + vx;
    this.vy = this.parent.vy * opts.shardFireworkVelMultiplier + vy;
    this.starty = this.y;
    this.dead = false;
    this.tick = 1;
    console.log("Shard reset", this);
  };
  Shard.prototype.step = function () {
    this.tick += 0.05;

    var px = this.x;
    var py = this.y;

    this.x += this.vx *= opts.xFriction;
    this.y += this.vy += opts.gravity;

    var proportion = 1 - (this.y - this.starty) / (h - this.starty);

    webgl.data.push(
      px,
      py,
      this.parent.ph,
      opts.projectileAlpha / this.tick,
      this.x,
      this.y,
      this.parent.hue,
      opts.projectileAlpha / this.tick
    );

    if (this.y > h) this.dead = true;
    console.log("Shard step", this);
  };

  function anim() {
    console.log("anim start");

    window.requestAnimationFrame(anim);
    webgl.clear();
    ++tick;
    if (fireworks.length < opts.fireworks) fireworks.push(new Firework());
    fireworks.map(function (firework) {
      firework.step();
    });

    console.log("webgl.data:", webgl.data);
    // webgl.draw(gl.value.LINES);
    gl.value.drawArrays(gl.value.LINES, 0, webgl.data.length / 4);
    console.log("anim end", {
      tick,
      fireworks,
      webglDataLength: webgl.data.length,
    });
  }

  watch(
    () => props.isVisible, // Следим за isVisible
    (newIsVisible) => {
      console.log("isVisible changed:", newIsVisible);
      if (newIsVisible) {
        fireworks.length = 0;
        console.log("showFireworks");
        anim();
        setTimeout(() => {
          // Возможно, стоит emit-ить событие обратно в Mouse.vue,
          // чтобы сообщить о завершении анимации и скрыть все
          // props.isVisible.value = false;
        }, 3500);
      } else {
        if (gl.value) {
          webgl.clear();
        }
      }
    }
  );

  window.addEventListener("resize", function () {
    c.width = window.innerWidth;
    c.height = window.innerHeight;
    gl.value.viewport(0, 0, c.width, c.height);
    gl.value.uniform2f(webgl.resUniformLoc, c.width, c.height);
  });
});

onUnmounted(() => {
  if (gl.value) {
    gl.value.getExtension("WEBGL_lose_context")?.loseContext();
    gl.value = null;
    console.log("WebGL context destroyed");
  }
});
</script>

<!-- <style scoped>
canvas {
  position: fixed;
  top: 0;
  left: 0;
  z-index: -2;
  pointer-events: none;
}
</style> -->
