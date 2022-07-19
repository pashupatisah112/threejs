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
import { CubeTextureLoader } from "three";
import space from "../assets/images/space.jpg"
export default {
    data() {
        return {
            scene: new THREE.Scene(),
            camera: new THREE.PerspectiveCamera(70, window.innerWidth / window.innerHeight, 0.1, 1000),
            renderer: new THREE.WebGLRenderer(),
            boxGeometry: new THREE.BoxGeometry(4, 4, 4),
            planeGeometry:new THREE.PlaneGeometry(30,30),
            sphereGeometry:new THREE.SphereGeometry(4,50,50),
            planeMaterial: new THREE.MeshBasicMaterial({
                color: 0xFFFFFF,side:THREE.DoubleSide  //double side give color to both side of plane
            }),
            boxMaterial: new THREE.MeshBasicMaterial({
                // color: 0x000229
            }),
            sphereMaterial:new THREE.MeshBasicMaterial({
                color:0x000229,wireframe:true  //wireframe true to view skeleton
            }),
            box:null,

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

        const textureLoader=new THREE.TextureLoader()

        // for different faces of box
        const boxFaces=[
            new THREE.MeshBasicMaterial({map:textureLoader.load(space)}),
            new THREE.MeshBasicMaterial({color:0x000229}),
            new THREE.MeshBasicMaterial({map:textureLoader.load(space)}),
            new THREE.MeshBasicMaterial({map:textureLoader.load(space)}),
            new THREE.MeshBasicMaterial({color:0xFFFF00}),
            new THREE.MeshBasicMaterial({map:textureLoader.load(space)}),

        ]
        // creating object mesh
        this.box = new THREE.Mesh(this.boxGeometry, boxFaces)
        const plane=new THREE.Mesh(this.planeGeometry,this.planeMaterial)
        const sphere=new THREE.Mesh(this.sphereGeometry,this.sphereMaterial)
        sphere.position.set(-10,0,0)
        //creating grid helper
        const grid=new THREE.GridHelper(30,10);//first parameter for grid area ,second for sub-division
        //making plane horizontal
        plane.rotation.x=-0.5*Math.PI
        // adding object to scene
        this.scene.add(this.box);
        this.scene.add(plane)
        this.scene.add(sphere)
        this.scene.add(grid)

        //setting color background
        // this.renderer.setClearColor(0xFF0000)

        //setting image as background
        // normal bg
        this.scene.background=textureLoader.load(space)
        // object paint with image
        this.box.material.map=textureLoader.load(space)

        //setting animation loop
        this.renderer.setAnimationLoop(this.animate)
    },
    methods: {
        animate() {
            this.box.rotation.x +=0.01
            this.box.rotation.y += 0.01
            this.renderer.render(this.scene, this.camera);
        }
    }

}
</script>
