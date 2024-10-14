# Using Three Js React

[#webdev](https://dev.to/t/webdev)[#javascript](https://dev.to/t/javascript)[#threejs](https://dev.to/t/threejs)[#react](https://dev.to/t/react)

In today’s world, creating engaging web experiences is becoming more important than ever, and **3D graphics** play a huge part in that. Whether you're building interactive product showcases, immersive UI components, or even games, integrating **Three.js** with **React** gives developers a smooth way to add 3D elements to websites with ease.

### [](https://dev.to/codeparrot/using-three-js-react-3me4#what-is-threejs)**What is Three.js?**

**Three.js** is a JavaScript library that makes it easier to render 3D graphics using **WebGL**. WebGL is incredibly powerful, but it’s complex to use directly. Three.js simplifies that process, providing tools to build 3D objects, lights, cameras, and animations with less effort.

### [](https://dev.to/codeparrot/using-three-js-react-3me4#why-use-threejs-with-react)**Why Use Three.js with React?**

React makes it easy to **break down a user interface into reusable components**. If you combine it with **Three.js**, you can build your 3D scenes in a similar way — component by component. This helps keep your code more **modular** and **easier to maintain**.

Luckily, there’s a library called **react-three-fiber**, which provides a bridge between Three.js and React, making it possible to render Three.js elements directly in React components.

### [](https://dev.to/codeparrot/using-three-js-react-3me4#setting-up-your-3d-react-project)**Setting Up Your 3D React Project**

To get started with Three.js and React, let’s install the necessary dependencies. We’ll use:

- **three**: The core Three.js library.
- **@react-three/fiber**: A React renderer for Three.js.
- **@react-three/drei**: A helper library with useful tools for common Three.js tasks.

#### [](https://dev.to/codeparrot/using-three-js-react-3me4#step-1-install-the-dependencies)**Step 1: Install the Dependencies**

Open your terminal and create a new React app. Then install the required libraries:

```bash
npx create-react-app threejs-react-demo
cd threejs-react-demo
npm install three @react-three/fiber @react-three/drei
```

#### [](https://dev.to/codeparrot/using-three-js-react-3me4#step-2-create-your-first-3d-scene-with-a-rotating-cube)**Step 2: Create Your First 3D Scene with a Rotating Cube**

We’ll build a simple rotating cube that will react to user interactions like clicks and mouse hovers. Let’s start by creating a `Scene` component.

### [](https://dev.to/codeparrot/using-three-js-react-3me4#building-the-3d-cube-component)**Building the 3D Cube Component**

Create a file called **`Scene.js`** inside your `src` folder and paste the following code:

```js
// Scene.js
import React, { useRef, useState } from 'react';
import { Canvas, useFrame } from '@react-three/fiber';

const RotatingCube = () => {
  const meshRef = useRef();
  const [hovered, setHovered] = useState(false);
  const [clicked, setClicked] = useState(false);

  // Rotate the cube on every frame update
  useFrame(() => {
    meshRef.current.rotation.x += 0.01;
    meshRef.current.rotation.y += 0.01;
  });

  return (
    <mesh
      ref={meshRef}
      scale={clicked ? 1.5 : 1}
      onClick={() => setClicked(!clicked)}
      onPointerOver={() => setHovered(true)}
      onPointerOut={() => setHovered(false)}
    >
      <boxGeometry args={[1, 1, 1]} />
      <meshStandardMaterial color={hovered ? 'hotpink' : 'orange'} />
    </mesh>
  );
};

const Scene = () => {
  return (
    <Canvas style={{ height: '100vh' }}>
      <ambientLight intensity={0.5} />
      <pointLight position={[10, 10, 10]} />
      <RotatingCube />
    </Canvas>
  );
};

export default Scene;
```

### [](https://dev.to/codeparrot/using-three-js-react-3me4#how-the-code-works)**How the Code Works**

1.  **Canvas**: The `Canvas` component acts like a container for your 3D scene, just like a `<canvas>` element in HTML.
2.  **mesh**: A 3D object that holds the geometry (shape) and material (appearance). Here, it’s used to create a cube.
3.  **boxGeometry**: A built-in geometry to create a cube shape.
4.  **meshStandardMaterial**: A material that supports basic lighting.
5.  **useRef**: A hook to reference the mesh so we can rotate it using JavaScript.
6.  **useFrame**: A hook from `@react-three/fiber` that runs on every animation frame, allowing us to rotate the cube continuously.
7.  **Interactivity**: The cube changes its scale and color when clicked or hovered, adding a simple form of user interaction.

### [](https://dev.to/codeparrot/using-three-js-react-3me4#step-3-use-the-scene-component-in-your-app)**Step 3: Use the Scene Component in Your App**

Now, let’s render our 3D scene inside the main `App.js` file.

```js
// App.js
import React from 'react';
import Scene from './Scene';

function App() {
  return (
    <div style={{ height: '100vh', width: '100vw', margin: 0, padding: 0 }}>
      <Scene />
    </div>
  );
}

export default App;
```

[threejs demo](https://media.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fwano2j6u1xt35flyga07.gif)

### [](https://dev.to/codeparrot/using-three-js-react-3me4#making-your-3d-scene-more-interactive)**Making Your 3D Scene More Interactive**

We’ve built a simple rotating cube, but let’s explore how we can improve it. Here are a few ways to make your scene more interactive:

#### [](https://dev.to/codeparrot/using-three-js-react-3me4#1-adding-shadows)**1. Adding Shadows**

Shadows add realism to your 3D scenes. You can enable shadows by adding these lines:

```js
<Canvas shadows>
  <ambientLight intensity={0.5} />
  <spotLight position={[10, 10, 10]} angle={0.3} castShadow />
  <mesh receiveShadow>
    <planeGeometry args={[10, 10]} />
    <meshStandardMaterial color="gray" />
  </mesh>
</Canvas>
```

#### [](https://dev.to/codeparrot/using-three-js-react-3me4#2-importing-3d-models)**2. Importing 3D Models**

You can import more complex 3D models (like GLTF/GLB files) using the `useGLTF` hook from `@react-three/drei`. Here’s a quick example:

```js
import { useGLTF } from '@react-three/drei';

const Model = () => {
  const { scene } = useGLTF('/path-to-your-model/model.gltf');
  return <primitive object={scene} />;
};
```

### [](https://dev.to/codeparrot/using-three-js-react-3me4#performance-tips-for-large-3d-scenes)**Performance Tips for Large 3D Scenes**

1.  **Optimize your 3D models** by reducing polygon counts.
2.  **Use fewer lights** since lighting calculations can be costly.
3.  **Enable frustum culling** to avoid rendering off-screen objects.
4.  **Lazy-load heavy components** with React’s `Suspense` component.

### [](https://dev.to/codeparrot/using-three-js-react-3me4#conclusion)**Conclusion**

You’ve just created a basic 3D cube using **Three.js** in React! 🎉 With this setup, you can start experimenting with more complex scenes, animations, and models. Integrating Three.js with React gives you the best of both worlds — the **power of 3D graphics** and the **modularity of React components**.

For more information visit the [documentation](https://threejs.org/).
