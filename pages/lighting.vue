<template>
<div>
    <p>Ambient lights: indirect light from source like in our room</p>
        <p>Spot light: light focusing at certai point like torch light</p>
        <p>Directional light: Sunlight</p>

    <div ref="canvas">

    </div>
</div>
</template>

<script>
import { Plane } from 'three';

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
            planeGeometry:new THREE.PlaneGeometry(30,30),
            sphereGeometry:new THREE.SphereGeometry(4,50,50),
            //MeshBasicMaterial is not effected by light so changing to MeshStandardMaterial
            planeMaterial: new THREE.MeshStandardMaterial({
                color: 0xFFFFFF,side:THREE.DoubleSide  //double side give color to both side of plane
            }),
            boxMaterial: new THREE.MeshStandardMaterial({
                color: 0x000229
            }),
            sphereMaterial:new THREE.MeshStandardMaterial({
                color:0x000229,wireframe:false  //wireframe true to view skeleton
            }),
            box:null,
            spotlight:null,
            options:null,
            sLightHelper:null,
            sphere:null

        }
    },
    mounted() {
        //rendering the view section
        this.renderer.setSize(window.innerWidth, window.innerHeight)
        this.$refs.canvas.appendChild(this.renderer.domElement)

        this.camera.position.set(-5, 30, 20);

        //to move object freely using mouse and to zoom in and zoom out 
        let OC = require("three/examples/jsm/controls/OrbitControls.js")
        const orbit = new OC.OrbitControls(this.camera, this.renderer.domElement)
        orbit.update()

        // creating object mesh
        const plane=new THREE.Mesh(this.planeGeometry,this.planeMaterial)
        this.sphere=new THREE.Mesh(this.sphereGeometry,this.sphereMaterial)
        this.sphere.position.set(-10,7,0)
        //creating grid helper
        const grid=new THREE.GridHelper(30,10);//first parameter for grid area ,second for sub-division
        //making plane horizontal
        plane.rotation.x=-0.5*Math.PI

        // //adding light to scene
        // const ambientLight=new THREE.AmbientLight(0x333333)
        // this.scene.add(ambientLight)
        // const directionalLight=new THREE.DirectionalLight(0xFFFFFF,0.8)
        // this.scene.add(directionalLight)
        // directionalLight.position.set(-30,30,0)
        // const dLightHelper=new THREE.DirectionalLightHelper(directionalLight,5)//5 is size of light
        // this.scene.add(dLightHelper)
        
        // // enabling shadow
        this.renderer.shadowMap.enabled=true
        // sphere.castShadow=true
        // Plane.receiveShadow=true

        this.spotLight=new THREE.SpotLight(0xFFFFFF)
        
        this.spotLight.position.set(-100,100,0)
        this.sLightHelper=new THREE.SpotLightHelper(this.spotLight)
        this.spotLight.castShadow=true
        this.spotLight.angle=0.1
        // adding object to scene
                this.scene.add(this.sLightHelper)

        this.scene.add(this.spotLight)
        this.scene.add(plane)
        this.scene.add(this.sphere)
        this.scene.add(grid)
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
                speed:0.01,
                angle:0.1,
                penumbra:0,
                intensity:1
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
            gui.add(this.options,'angle',0,1)

            gui.add(this.options,'penumbra',0,1)

            gui.add(this.options,'intensity',0,1)

        },
        animate() {
            // this.box.rotation.x +=0.01
            // this.box.rotation.y += 0.01
            this.step+=this.options.speed
            this.sphere.position.y=10*Math.abs(Math.sin(this.step))
            this.spotLight.angle=this.options.angle
            this.spotLight.penumbra=this.options.penumbra
            this.spotLight.intensity=this.options.intensity
            this.sLightHelper.update()

            this.renderer.render(this.scene, this.camera);
        }
    }

}
</script>
 