---
title: Three.js 入门
published: 2023-12-12
description: 'Read more about 3D graphics and Three.js in guang_blog'
image: ''
tags: [Three.js, 3D图形, Web开发]
category: '前端开发'
draft: false
---

### Three.js 入门

------

### 什么是 Three.js？

Three.js 是一个基于 WebGL 的 JavaScript 库，WebGL 是浏览器中用来渲染 3D 图形的技术。通过 Three.js，你可以不用关心 WebGL 的底层细节，直接使用高级 API 来实现 3D 渲染。

简单来说，Three.js 就是 WebGL 的“包装纸”，让你可以更轻松地在网页上展示 3D 内容。

------

### 安装 Three.js

要开始使用 Three.js，首先你需要安装它。在现代前端项目中，通常会使用 npm 来管理包。打开你的终端，输入以下命令：

```
bash


复制代码
npm install three
```

如果你只是想快速试试，也可以直接在 HTML 文件中通过 CDN 引入：

```
html


复制代码
<script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>
```

------

### 一个最简单的 Three.js 示例

首先，让我们来写一个最简单的 Three.js 程序，创建一个旋转的立方体。你可以通过这个示例了解 Three.js 的基本构成。

#### HTML 文件

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Three.js 入门</title>
    <style>
        body { margin: 0; }
        canvas { display: block; }
    </style>
</head>
<body>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>
    <script>
        // 1. 场景 (Scene)
        const scene = new THREE.Scene();
        
        // 2. 相机 (Camera)
        const camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000);
        
        // 3. 渲染器 (Renderer)
        const renderer = new THREE.WebGLRenderer();
        renderer.setSize(window.innerWidth, window.innerHeight);
        document.body.appendChild(renderer.domElement);

        // 4. 立方体 (Cube)
        const geometry = new THREE.BoxGeometry();
        const material = new THREE.MeshBasicMaterial({ color: 0x00ff00 });
        const cube = new THREE.Mesh(geometry, material);
        scene.add(cube);

        // 5. 设置相机位置
        camera.position.z = 5;

        // 6. 渲染循环 (Animation)
        function animate() {
            requestAnimationFrame(animate);
            cube.rotation.x += 0.01; // X 轴旋转
            cube.rotation.y += 0.01; // Y 轴旋转
            renderer.render(scene, camera);
        }

        animate();
    </script>
</body>
</html>
```

#### 代码解析：

1. **场景（Scene）**：所有的 3D 对象都需要添加到场景中。这里我们创建了一个空的场景，后续将添加 3D 物体到其中。
2. **相机（Camera）**：我们使用了透视相机（`PerspectiveCamera`），它模拟了人眼的视觉效果。你可以控制相机的位置、视角等。
3. **渲染器（Renderer）**：`WebGLRenderer` 用来在浏览器中渲染 3D 图形，并将结果显示在页面上。`setSize` 用来设置画布的大小，这里我们将其设为窗口的大小。
4. **立方体（Cube）**：我们创建了一个简单的立方体，通过 `BoxGeometry` 来定义它的形状，通过 `MeshBasicMaterial` 来给它上色。然后将立方体添加到场景中。
5. **相机位置**：为了让立方体能够在屏幕上显示，我们将相机放置在 z 轴上，距离场景一定的距离。
6. **动画（Animation）**：`animate` 函数使用了 `requestAnimationFrame`，这是一个浏览器提供的 API，用来创建流畅的动画效果。每一帧我们都会让立方体旋转，之后重新渲染场景。

------

### 重要概念讲解

#### 1. **场景 (Scene)**

场景是所有 3D 对象的容器。在 Three.js 中，你会创建一个 `THREE.Scene()` 实例，所有你要渲染的物体（如立方体、球体等）都需要添加到这个场景中。

#### 2. **相机 (Camera)**

相机决定了你从哪个角度观看场景。常用的相机类型有：

- **PerspectiveCamera（透视相机）**：模拟真实世界的视觉效果，常用于大多数 3D 场景。
- **OrthographicCamera（正交相机）**：没有透视效果，适用于地图或工程图等场景。

#### 3. **渲染器 (Renderer)**

渲染器负责将场景和相机的画面“翻译”成图像并显示在屏幕上。Three.js 的默认渲染器是 WebGLRenderer，但你也可以使用其他渲染器（如 CanvasRenderer）。

#### 4. **物体（Mesh）**

物体是 3D 场景中的可视元素，它们由几何体（`Geometry`）和材质（`Material`）构成。几何体定义了物体的形状，材质定义了物体的外观。

常见的几何体类型包括：

- **BoxGeometry**：立方体
- **SphereGeometry**：球体
- **PlaneGeometry**：平面

材质有很多种，比如 `MeshBasicMaterial`（不受光照影响的基础材质），`MeshLambertMaterial`（受光照影响的材质）等。

------

### 增加光源和相机控制

### 1. **添加光源和相机控制**

我们可以通过添加光源和使用 `OrbitControls.js` 来增强场景的真实感和互动性。下面是如何增加光源和控制相机的代码：

#### HTML 文件 (增加光源和相机控制)

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Three.js 入门 - 光源与相机控制</title>
    <style>
        body { margin: 0; }
        canvas { display: block; }
    </style>
</head>
<body>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/controls/OrbitControls.js"></script>
    <script>
        // 1. 创建场景
        const scene = new THREE.Scene();

        // 2. 创建相机
        const camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000);
        camera.position.z = 5;

        // 3. 创建渲染器
        const renderer = new THREE.WebGLRenderer();
        renderer.setSize(window.innerWidth, window.innerHeight);
        document.body.appendChild(renderer.domElement);

        // 4. 创建立方体
        const geometry = new THREE.BoxGeometry();
        const material = new THREE.MeshStandardMaterial({ color: 0x00ff00 });
        const cube = new THREE.Mesh(geometry, material);
        scene.add(cube);

        // 5. 添加光源
        const ambientLight = new THREE.AmbientLight(0x404040, 2); // 环境光
        scene.add(ambientLight);

        const pointLight = new THREE.PointLight(0xff0000, 1, 100); // 点光源
        pointLight.position.set(10, 10, 10);
        scene.add(pointLight);

        // 6. 使用 OrbitControls 让相机可以旋转、缩放、平移
        const controls = new THREE.OrbitControls(camera, renderer.domElement);

        // 7. 渲染循环
        function animate() {
            requestAnimationFrame(animate);
            cube.rotation.x += 0.01; // X 轴旋转
            cube.rotation.y += 0.01; // Y 轴旋转
            controls.update(); // 更新相机控制器
            renderer.render(scene, camera);
        }

        animate();
    </script>
</body>
</html>
```

#### 代码说明：

- **光源**：我们添加了两种光源：`AmbientLight` (环境光) 和 `PointLight` (点光源)。环境光是全局的，不会产生阴影，点光源则模拟一个从一点发散的光源。
- **OrbitControls**：通过引入 `OrbitControls.js`，我们可以通过鼠标拖拽来旋转、缩放和移动相机视角。

### 2. **加载外部 3D 模型**

让我们加载一个外部的 3D 模型（如 `.glb` 文件），并在场景中展示。假设你已经有了一个 `.glb` 文件，可以使用 `GLTFLoader` 来加载它。

#### HTML 文件 (加载外部 3D 模型)

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Three.js 入门 - 加载 3D 模型</title>
    <style>
        body { margin: 0; }
        canvas { display: block; }
    </style>
</head>
<body>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/loaders/GLTFLoader.js"></script>
    <script>
        // 1. 创建场景
        const scene = new THREE.Scene();

        // 2. 创建相机
        const camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000);
        camera.position.z = 5;

        // 3. 创建渲染器
        const renderer = new THREE.WebGLRenderer();
        renderer.setSize(window.innerWidth, window.innerHeight);
        document.body.appendChild(renderer.domElement);

        // 4. 创建光源
        const ambientLight = new THREE.AmbientLight(0x404040, 2);
        scene.add(ambientLight);

        const pointLight = new THREE.PointLight(0xff0000, 1, 100);
        pointLight.position.set(10, 10, 10);
        scene.add(pointLight);

        // 5. 加载 3D 模型
        const loader = new THREE.GLTFLoader();
        loader.load('path_to_your_model.glb', function(gltf) {
            scene.add(gltf.scene); // 将模型添加到场景
        });

        // 6. 渲染循环
        function animate() {
            requestAnimationFrame(animate);
            renderer.render(scene, camera);
        }

        animate();
    </script>
</body>
</html>
```

#### 代码说明：

- **GLTFLoader**：这是 Three.js 提供的一个加载器，用于加载 `.glb` 格式的 3D 模型文件。你需要将模型文件路径替换为实际路径。
- **添加光源**：同样添加了环境光和点光源，确保模型能够正确显示。

### 3. **制作一个简单的 3D 场景交互**

我们可以添加一些交互功能，比如通过鼠标点击改变物体的颜色，或者添加一个按钮控制物体的动画。

#### HTML 文件 (3D 场景交互)

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Three.js 入门 - 交互</title>
    <style>
        body { margin: 0; }
        canvas { display: block; }
        #button { position: absolute; top: 20px; left: 20px; padding: 10px; background-color: #007BFF; color: white; border: none; cursor: pointer; }
    </style>
</head>
<body>
    <button id="button">点击改变颜色</button>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>
    <script>
        // 1. 创建场景
        const scene = new THREE.Scene();

        // 2. 创建相机
        const camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000);
        camera.position.z = 5;

        // 3. 创建渲染器
        const renderer = new THREE.WebGLRenderer();
        renderer.setSize(window.innerWidth, window.innerHeight);
        document.body.appendChild(renderer.domElement);

        // 4. 创建立方体
        const geometry = new THREE.BoxGeometry();
        const material = new THREE.MeshBasicMaterial({ color: 0x00ff00 });
        const cube = new THREE.Mesh(geometry, material);
        scene.add(cube);

        // 5. 渲染循环
        function animate() {
            requestAnimationFrame(animate);
            cube.rotation.x += 0.01;
            cube.rotation.y += 0.01;
            renderer.render(scene, camera);
        }

        animate();

        // 6. 交互：点击按钮改变颜色
        document.getElementById('button').addEventListener('click', function() {
            cube.material.color.set(0xff0000); // 改变颜色为红色
        });
    </script>
</body>
</html>
```

#### 代码说明：

- **按钮交互**：我们添加了一个简单的 HTML 按钮，当用户点击按钮时，会触发颜色变化事件，改变立方体的颜色。
- **渲染循环**：通过 `requestAnimationFrame` 创建的动画使得立方体不断旋转。

------

### 常见的三维场景应用

1. **3D 可视化**：比如数据可视化、建筑设计等。
2. **游戏开发**：使用 Three.js 可以开发简单的 3D 游戏，当然它不是专业的游戏引擎，但对于一些轻量级的项目来说完全足够。
3. **动画效果**：Three.js 可以与其他库（如 GSAP）配合，制作流畅的 3D 动画。

------

### 总结

通过这篇入门博客，你已经了解了如何使用 Three.js 来创建一个简单的 3D 场景，并且掌握了构建 3D 图形的基本概念。你可以用 Three.js 制作酷炫的网页动画，或者构建一个完整的 3D 游戏。

下一步，你可以尝试：

- 增加更多的物体和光源，让场景更加丰富。
- 用动画和交互控制提升用户体验。
- 尝试加载外部 3D 模型（如 `.obj` 或 `.glb` 格式）。





