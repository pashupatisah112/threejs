<template>
<div>
    <v-container fluid>
        <v-row justify="center" ref="canvas">

        </v-row>
        <v-row>
            <v-col>

            </v-col>
        </v-row>
    </v-container>
</div>
</template>

<script>
import * as THREE from 'three';
export default {
    data() {
        return {
            //screne,camera and renderer are first three things that we create in three js
            scene: new THREE.Scene(),
            camera: new THREE.PerspectiveCamera(70, window.innerWidth / window.innerHeight, 0.1, 1000),
            //(field of view,aspect ratio,near clipping plane,far clipping plane)
            renderer: new THREE.WebGLRenderer(),
            //object to render
            geometry: new THREE.BoxGeometry(1, 1, 1),
            // material for geometry
            material: new THREE.MeshBasicMaterial({
                color: 0x00ff00
            }),
            
        }
    },
    mounted() {
        // adding material to geometry
            this.cube= new THREE.Mesh(this.geometry, this.material)
        this.renderer.setSize(window.innerWidth, window.innerHeight)
        // document.body.appendChild(this.renderer.domElement)
                this.$refs.canvas.appendChild(this.renderer.domElement)

        this.scene.add(this.cube);
        this.camera.position.z = 5;
        this.animate()
    },
    methods: {
        animate() {
            requestAnimationFrame(this.animate);

            this.cube.rotation.x += 0.01;
            this.cube.rotation.y += 0.01;

            this.renderer.render(this.scene, this.camera);
        }
    }

}
</script>
