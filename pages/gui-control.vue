<template>
<div>
    <div ref="canvas">

    </div>
</div>
</template>

<script>
let THREE = null
if (process.client) {
    THREE = require('three');
}
let dat = null
if (process.client) {
    dat = require('dat.gui')

}
export default {
    data() {
        return {
            scene: new THREE.Scene(),
            camera: new THREE.PerspectiveCamera(70, window.innerWidth / window.innerHeight, 0.1, 1000),
            renderer: new THREE.WebGLRenderer(),
            boxGeometry: new THREE.BoxGeometry(4, 4, 4),
            planeGeometry: new THREE.PlaneGeometry(30, 30),
            sphereGeometry: new THREE.SphereGeometry(4, 50, 50),
            planeMaterial: new THREE.MeshBasicMaterial({
                color: 0xFFFFFF,
                side: THREE.DoubleSide //double side give color to both side of plane
            }),
            boxMaterial: new THREE.MeshBasicMaterial({
                color: 0x000229
            }),
            sphereMaterial: new THREE.MeshBasicMaterial({
                color: 0x000229,
                wireframe: false //wireframe true to view skeleton
            }),
            box: null,
            sphere:null,
            options:null,
            step:0,
            
            // speed:0.01 //for direct use...but shown here using gui see below

        }
    },
    mounted() {
        //rendering the view section
        this.renderer.setSize(window.innerWidth, window.innerHeight)
        this.$refs.canvas.appendChild(this.renderer.domElement)
        // displaying axis and camera position
        const axesHelper = new THREE.AxesHelper(5)
        this.scene.add(axesHelper)
        this.camera.position.set(-5, 2, 5);

        //to move object freely using mouse and to zoom in and zoom out 
        let OC = require("three/examples/jsm/controls/OrbitControls.js")
        const orbit = new OC.OrbitControls(this.camera, this.renderer.domElement)
        orbit.update()
        // creating object mesh
        this.box = new THREE.Mesh(this.boxGeometry, this.boxMaterial)
        const plane = new THREE.Mesh(this.planeGeometry, this.planeMaterial)
        this.sphere = new THREE.Mesh(this.sphereGeometry, this.sphereMaterial)
        this.sphere.position.set(-10, 5,0)
        //creating grid helper
        const grid = new THREE.GridHelper(30, 10); //first parameter for grid area ,second for sub-division
        //making plane horizontal
        plane.rotation.x = -0.5 * Math.PI
        // adding object to scene
        this.scene.add(this.box);
        this.scene.add(plane)
        this.scene.add(this.sphere)
        this.scene.add(grid)
        this.renderer.render(this.scene, this.camera);
        //setting animation loop
        this.renderer.setAnimationLoop(this.animate)

        this.getGUI()
    },
    methods: {
        getGUI(){
            const gui=new dat.GUI()
            // default property setting
            this.options={
                sphereColor:'#ffea00',
                wireframe:false,
                speed:0.01
            }
            //setting function to be called when a color is selected
            gui.addColor(this.options,'sphereColor').onChange((e)=>{
                this.sphere.material.color.set(e)
            })
            //setting function to be called when a wireframe mode is selected
            gui.add(this.options,'wireframe').onChange((e)=>{
                this.sphere.material.wireframe=e
            })

            //controlling sphere bounce
            gui.add(this.options,'speed',0,0.1)
        },
        animate() {
            //cube rotation
            this.box.rotation.x += 0.01
            this.box.rotation.y += 0.01
            //sphere bounce
            this.step+=this.options.speed
            this.sphere.position.y=10*Math.abs(Math.sin(this.step))

            this.renderer.render(this.scene, this.camera);
        }
    }

}
</script>
