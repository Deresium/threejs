<template>
    <div id="app">
        <div id="threejs" ref="threejs"/>

    </div>
</template>
<script lang="ts">
import {defineComponent, onMounted, ref} from "vue"
import * as THREE from "three"

export default defineComponent({
    setup(){
        const threejs = ref();

        const camera = new THREE.PerspectiveCamera(75, 1, 0.1, 1000);
        camera.position.z = 5;

        const renderer = new THREE.WebGLRenderer();
        renderer.setPixelRatio(window.devicePixelRatio);

        const updateSizeScene = () => {
            camera.aspect = threejs.value.clientWidth / threejs.value.clientHeight;
            camera.updateProjectionMatrix();
            renderer.setSize(threejs.value.clientWidth, threejs.value.clientHeight);
        }

        window.addEventListener('resize', updateSizeScene);

        onMounted(() => {
            updateSizeScene();

            const scene = new THREE.Scene();

            const light = new THREE.DirectionalLight('#FFFFFF', 1);
            light.position.set(-1, 2, 4);

            const geometry = new THREE.BoxGeometry();
            const material = new THREE.MeshPhongMaterial({color: '#FF0000'});
            const material2 = new THREE.MeshPhongMaterial({color: '#00FF00'});
            const cube = new THREE.Mesh(geometry, material);
            const cube2 = new THREE.Mesh(geometry, material2);
            cube2.position.x = 2;

            scene.add(cube);
            scene.add(cube2);
            scene.add(light);
            threejs.value.appendChild(renderer.domElement);

            function animate(){
                requestAnimationFrame(animate);
                cube.rotation.x += 0.01;
                cube.rotation.y += 0.01;

                cube2.rotation.x -= 0.01;
                cube2.rotation.y -= 0.01;
                renderer.render(scene, camera);
            }
            animate();
        })

        return{
            threejs
        }
    }
})
</script>

<style>
#app{
    display: flex;
    justify-content: center;
    width: 100%;
}

#threejs{
    width: 90%;
    height: 20vh;
}
</style>
