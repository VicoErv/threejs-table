<template>
    <div id="app">
        <div class="wawa"></div>
        <canvas id="canvas" ref="canvas"></canvas>
        <div class="wawa">

        </div>
    </div>
</template>

<script>
import * as THREE from 'three'
import gsap from 'gsap'
import ScrollTrigger from 'gsap/ScrollTrigger'

gsap.registerPlugin(ScrollTrigger)

export default {
    name: 'App',
    mounted() {
        const renderer = new THREE.WebGLRenderer({canvas: this.$refs.canvas})
        renderer.shadowMap.enabled = true
        renderer.shadowMap.type = THREE.PCFSoftShadowMap
        renderer.setSize(window.innerWidth, window.innerHeight)

        const camera = new THREE.PerspectiveCamera(45, window.innerWidth / window.innerHeight, 1, 500)
        camera.position.set(-70, 50, 70)
        camera.lookAt(0, 0, 0)

        console.log(camera.position)
        gsap.to(camera.position, {
            delay: .2,
            duration: 1,
            x: 100,
            onUpdate: () => {
                camera.lookAt(scene.position)
            }
        })

        const scene = new THREE.Scene()

        const geometry = new THREE.BoxGeometry(50, 2, 30)
        const material = new THREE.MeshStandardMaterial({color: 0x00ff00})
        const cube = new THREE.Mesh(geometry, material)
        cube.castShadow = true
        cube.position.set(0, 15, 0)

        scene.add(cube)

        const feetGeo = new THREE.BoxGeometry(2, 15, 2)
        const feetMaterial = new THREE.MeshStandardMaterial({color: 0xff0000})
        const feet = new THREE.Mesh(feetGeo, feetMaterial)
        const feet2 = new THREE.Mesh(feetGeo, feetMaterial)
        const feet3 = new THREE.Mesh(feetGeo, feetMaterial)
        const feet4 = new THREE.Mesh(feetGeo, feetMaterial)

        feet.castShadow = true
        feet2.castShadow = true
        feet3.castShadow = true
        feet4.castShadow = true

        feet.position.set(24, 8, 14)
        feet2.position.set(24, 8, -14)
        feet3.position.set(-24, 8, -14)
        feet4.position.set(-24, 8, 14)

        scene.add(feet)
        scene.add(feet2)
        scene.add(feet3)
        scene.add(feet4)

        const planeGeo = new THREE.BoxGeometry(100, 1, 100)
        const plane = new THREE.Mesh(planeGeo, material)

        scene.add(plane)
        plane.receiveShadow = true

        const light = new THREE.PointLight(0xffffff, 1, 100)
        light.position.set(5, 70, 30)
        light.castShadow = true
        scene.add(light)

        gsap.from(light.position, {
            delay: .2,
            duration: 1,
            y: 200,
        })

        gsap.to(light.position, {
            scrollTrigger: {
                trigger: this.$refs.canvas,
                scrub: true,
                end: 'bottom 50%',
                markers: true
            },
            duration: .5,
            y: 50
        })

        function animate() {
            requestAnimationFrame(animate)
            renderer.render(scene, camera)
        }

        animate()
    }
}
</script>

<style>
body {
    margin: 0;
}

.wawa {
    height: 100vh;
}
</style>
