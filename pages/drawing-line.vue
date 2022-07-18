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
            camera: new THREE.PerspectiveCamera(70, window.innerWidth / window.innerHeight, 0.1, 500),
            //(field of view,aspect ratio,near clipping plane,far clipping plane)
            renderer: new THREE.WebGLRenderer(),
            //create a blue LineBasicMaterial
            material: new THREE.LineBasicMaterial({
                color: 0x0000ff
            })

        }
    },
    mounted() {
        this.camera.position.set(0, 0, 100)
        this.camera.lookAt(0, 0, 0)
        this.renderer.setSize(window.innerWidth, window.innerHeight)
        // document.body.appendChild(this.renderer.domElement)
        this.$refs.canvas.appendChild(this.renderer.domElement)

        const points = [];
        points.push(new THREE.Vector3(-10, 0, 0));
        points.push(new THREE.Vector3(0, 10, 0));
        points.push(new THREE.Vector3(10, 0, 0));

        const geometry = new THREE.BufferGeometry().setFromPoints(points);
        const line = new THREE.Line(geometry, this.material);
        this.scene.background=new THREE.Color(0xffffffff)
        this.scene.add(line);
        this.renderer.render(this.scene, this.camera);

    },

}
</script>
