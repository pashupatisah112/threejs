<template>
<div>
    <div ref="canvas">Description</div>

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
            geometry: new THREE.ExtrudeGeometry('Hello three.js!', {
                size: 80,
                height: 5,
                curveSegments: 12,
                bevelEnabled: true,
                bevelThickness: 10,
                bevelSize: 8,
                bevelOffset: 0,
                bevelSegments: 5
            })
        }
    },
    mounted() {
        const loader = new FontLoader();

        this.renderer.setSize(window.innerWidth, window.innerHeight)
        // document.body.appendChild(this.renderer.domElement)
        this.$refs.canvas.appendChild(this.renderer.domElement)

        this.scene.add(this.cube);
        this.camera.position.z = 5;
    }
}
</script>

<style>
#info {
    position: absolute;
    top: 10px;
    width: 100%;
    text-align: center;
    z-index: 100;
    display: block;
}
</style>
