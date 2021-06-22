<template lang="pug">
#three
</template>


<script>
const THREE = require('three')
/* eslint-disable no-unused-vars */
  // canvasHeight;
  // canvasWidth;
  //var newscene;
  //var renderer;
  //var camera;
  //var box;  // 定義各種共用的數據

 export default 
  {
  //    el: '#Three',
      beforeCreate: function() { console.log("beforeCreate") },
      created: function() { console.log("created") },
      beforeMount: function() { console.log("beforeMount")},
      mounted: function() { console.log("mounted"); this.init(); this.render() },
      /* 初始化不會觸發 */
      beforeUpdate: function() { console.log("beforeUpdate") },
      updated: function() { console.log("updated") },
      methods: {
        init() {
          console.log("init")
          this.canvasHeight = document.getElementById("three").offsetHeight;  // 將 canvas 高度設定成 #three 的高度
          this.canvasWidth = document.getElementById("three").offsetWidth;   // 將 canvas 高度設定成 #three 的高度
          console.log("this.canvasHeight=" + this.canvasHeight )
          console.log("this.canvasWidth=" + this.canvasWidth )
          this.createScene()
          // 監聽螢幕寬高變化來做簡單 RWD 設定
          let cam_for_innerhandler = this.camera
          let renderer_for_innerhandler = this.renderer
          window.addEventListener('resize', function() {
            cam_for_innerhandler.aspect = window.innerWidth / window.innerHeight
            cam_for_innerhandler.updateProjectionMatrix()
            renderer_for_innerhandler.setSize(window.innerWidth, window.innerHeight)
            console.log(window.innerWidth, window.innerHeight)
          })
        },
        createScene() {
          this.scene = new THREE.Scene()         // 建立新場景
          this.renderer = new THREE.WebGLRenderer()
          this.renderer.setSize(window.innerWidth, window.innerHeight) // 場景大小
          this.renderer.setClearColor(0xeeeeee, 1.0) // 預設背景顏色
          this.renderer.shadowMap.enable = true // 陰影效果

          // 將渲染器的 DOM 綁到網頁上
          document.body.appendChild(this.renderer.domElement)

          this.camera = new THREE.PerspectiveCamera(
            45,
            window.innerWidth / window.innerHeight,
            0.1,
            100
          )
          this.camera.position.set(10, 10, 10) // 相機位置
          this.camera.lookAt(this.scene.position) // 相機焦點

          let pointLight = new THREE.PointLight(0xffffff)
          pointLight.position.set(10, 10, -10)
          this.scene.add(pointLight)

          const geometry = new THREE.BoxGeometry(3, 3, 3) // 幾何體
          const material = new THREE.MeshPhongMaterial({ 
              color: 0x0000ff 
          }) // 材質
          this.cube = new THREE.Mesh(geometry, material) // 建立網格物件
          this.cube.position.set(0, 0, 0)
          this.scene.add(this.cube)

        },
        animate() {
          this.cube.rotation.x += 0.01
          this.cube.rotation.y += 0.01
        },        
        // 渲染場景
        render() {
          this.animate()
          requestAnimationFrame(this.render)
          this.renderer.render(this.scene, this.camera)
        }
      }

  

  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="stylus" scoped>
  #three
    width 100%
    height 100%
    background #ffffff
</style>