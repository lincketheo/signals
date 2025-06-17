<template>
  <div class="w-screen h-screen">
    <canvas ref="canvas" class="w-full h-full"></canvas>
  </div>
</template>

<script setup>
const canvas = ref(null);

onMounted(() => {
  const dpr = window.devicePixelRatio || 1;
  const canvasEl = canvas.value;

  canvasEl.width = canvasEl.clientWidth * dpr;
  canvasEl.height = canvasEl.clientHeight * dpr;

  const gl = canvas.value.getContext('webgl');

  if (!gl) {
    console.error('WebGL not supported');
    return;
  }

  gl.viewport(0, 0, canvasEl.width, canvasEl.height);

  const vertexShaderSource = `
    attribute vec2 a_position;
    uniform float u_angle;
    uniform float u_aspect;
    void main() {
      vec2 scaled = vec2(a_position.x, a_position.y * u_aspect);
      gl_Position = vec4(a_position, 0, 1);
    }
  `;

  const fragmentShaderSource = `
    precision mediump float;
    void main() {
      gl_FragColor = vec4(0.2, 0.7, 1.0, 1.0);
    }
  `;


  function createShader(gl, type, src) {
    const shader = gl.createShader(type);
    gl.shaderSource(shader, src);
    gl.compileShader(shader);
    if (!gl.getShaderParameter(shader, gl.COMPILE_STATUS)) {
      console.error(gl.getShaderInfoLog(shader));
      gl.deleteShader(shader);
    }
    return shader;
  }

  // Create the two shaders
  const vertexShader = createShader(gl, gl.VERTEX_SHADER, vertexShaderSource);
  const fragmentShader = createShader(gl, gl.FRAGMENT_SHADER, fragmentShaderSource);

  function createProgram(gl, vShader, fShader) {
    const program = gl.createProgram();
    gl.attachShader(program, vShader);
    gl.attachShader(program, fShader);
    gl.linkProgram(program);
    if (!gl.getProgramParameter(program, gl.LINK_STATUS)) {
      console.error(gl.getProgramInfoLog(program));
      gl.deleteProgram(program);
    }
    return program;
  }

  const program = createProgram(gl, vertexShader, fragmentShader);
  gl.useProgram(program);

  const points = [1, 4, 5, 1, 10, 0.1, 1.3, 5.1, 2, 20, 1, 2.3, -1, 1, 2, 3, 4, 5, 6, 7, 8, 1]

  const max = Math.max(...points);

  const vertexArray = [];

  for (let i = 0; i < points.length - 1; i++) {
    const x1 = (i / (points.length - 1)) * 2 - 1;
    const y1 = (points[i] / max) * 2 - 1;

    const x2 = ((i + 1) / (points.length - 1)) * 2 - 1;
    const y2 = (points[i + 1] / max) * 2 - 1;

    vertexArray.push(x1, y1);
    vertexArray.push(x2, y2);
  }

  const vertices = new Float32Array(vertexArray);
  console.log(vertices)

  const buffer = gl.createBuffer();
  gl.bindBuffer(gl.ARRAY_BUFFER, buffer);
  gl.bufferData(gl.ARRAY_BUFFER, vertices, gl.STATIC_DRAW);

  const posAttribLoc = gl.getAttribLocation(program, 'a_position');
  gl.enableVertexAttribArray(posAttribLoc);
  gl.vertexAttribPointer(posAttribLoc, 2, gl.FLOAT, false, 0, 0);

  const angleUniformLoc = gl.getUniformLocation(program, 'u_angle');
  const aspectUniformLoc = gl.getUniformLocation(program, 'u_aspect');

  function render(time) {
    const aspect = gl.canvas.height / gl.canvas.width;
    const angle = time * 0.001; // radians

    gl.clearColor(0.05, 0.05, 0.05, vertices.length / 2);
    gl.clear(gl.COLOR_BUFFER_BIT);

    gl.uniform1f(angleUniformLoc, angle);
    gl.uniform1f(aspectUniformLoc, aspect);
    gl.drawArrays(gl.LINES, 0, vertices.length / 2);

    requestAnimationFrame(render);
  }

  requestAnimationFrame(render);
});
</script>

<style scoped>
canvas {
  display: block;
  width: 100%;
  height: 100%;
}
</style>
