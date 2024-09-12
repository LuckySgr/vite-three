<template>
    <div id="threeContainer"></div>
</template>

<script setup>
import { onMounted } from 'vue'
import * as THREE from 'three'


onMounted(() => {
    // 初始化容器
    let container = document.getElementById('threeContainer');
    container.innerWidth = window.innerWidth;
    container.innerHeight = window.innerHeight;
    // 场景
    const scene = new THREE.Scene();
    // 相机
    const camera = new THREE.PerspectiveCamera(75, container.innerWidth / container.innerHeight, 0.1, 1000);
    // 渲染器
    const renderer = new THREE.WebGLRenderer();
    renderer.setSize(container.innerWidth, container.innerHeight);
    container.appendChild(renderer.domElement);

    // 几何体
    const geometry = new THREE.BoxGeometry(10, 10, 10);
    const material = new THREE.MeshBasicMaterial({ color: 0x00ff00 });
    const cube = new THREE.Mesh(geometry, material);
    scene.add(cube);
    camera.position.z = 20;
    const animate = () => {
        requestAnimationFrame(animate);
        cube.rotation.x += 0.01;
        cube.rotation.y += 0.01;
        renderer.render(scene, camera);
    }
    animate();

    // onresize 事件会在窗口被调整大小时发生
    window.onresize = () => {
        // 重置渲染器输出画布canvas尺寸
        renderer.setSize(window.innerWidth, window.innerHeight);
        // 全屏情况下：设置观察范围长宽比aspect为窗口宽高比
        camera.aspect = window.innerWidth / window.innerHeight;
        // 如果相机视锥体相关参数发生了变化，需要执行updateProjectionMatrix()方法更新相机的投影矩阵
        camera.updateProjectionMatrix();
    };
})

</script>

<style scoped>
#threeContainer {
    width: 100%;
    height: 100%;
}
</style>
