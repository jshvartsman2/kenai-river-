# kenai-river-
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Kenai River 3D Fishing</title>
    <style>
        body, html { margin: 0; height: 100%; background: #87CEEB; overflow: hidden; }
        #canvas { display: block; width: 100%; height: 100%; }
    </style>
</head>
<body>
    <canvas id="canvas"></canvas>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/cannon-es/0.20.0/cannon-es.js"></script>

    <script>
        // Set up the scene, camera, and renderer
        const scene = new THREE.Scene();
        const camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000);
        const renderer = new THREE.WebGLRenderer({ canvas: document.getElementById('canvas') });
        renderer.setSize(window.innerWidth, window.innerHeight);
        document.body.appendChild(renderer.domElement);

        // Set up lighting
        const ambientLight = new THREE.AmbientLight(0x404040, 1); // Ambient light
        const directionalLight = new THREE.DirectionalLight(0xffffff, 1); // Directional light
        directionalLight.position.set(5, 5, 5).normalize();
        scene.add(ambientLight);
        scene.add(directionalLight);

        // Set up water simulation
        let waterGeometry = new THREE.PlaneGeometry(100, 100);
        let waterMaterial = new THREE.MeshPhongMaterial({ color: 0x1e90ff, side: THREE.DoubleSide });
        let waterMesh = new THREE.Mesh(waterGeometry, waterMaterial);
        waterMesh.rotation.x = Math.PI / -2;
        waterMesh.position.y = -5;
        scene.add(waterMesh);

        // Set up fish model (example: a simple sphere as placeholder)
        const fishGeometry = new THREE.SphereGeometry(0.5, 16, 16);
        const fishMaterial = new THREE.MeshBasicMaterial({ color: 0xFFD700 });
        let fish = new THREE.Mesh(fishGeometry, fishMaterial);
        fish.position.set(0, 1, 0);
        scene.add(fish);

        // Camera position
        camera.position.z = 10;

        // Physics setup (using Cannon.js for realism)
        const world = new CANNON.World();
        world.gravity.set(0, -9.82, 0);
        const fishBody = new CANNON.Body({
            mass: 1, position: new CANNON.Vec3(0, 1, 0)
        });
        fishBody.addShape(new CANNON.Sphere(0.5));
        world.addBody(fishBody);

        // Fish movement (AI for randomness)
        function moveFish() {
            fish.position.x += Math.sin(Date.now() * 0.001) * 0.1;
            fish.position.z += Math.cos(Date.now() * 0.001) * 0.1;
        }

        // Line physics (simplified version)
        function simulateLine() {
            // Simulate fishing line dynamics with some tension effects
            // This will require more advanced physics but can be approximated with simple effects
        }

        // Game loop
        function animate() {
            requestAnimationFrame(animate);

            moveFish();  // Update fish AI behavior
            simulateLine();  // Simulate fishing line tension

            // Update physics world
            world.step(1 / 60);
            fish.position.copy(fishBody.position);

            // Render the scene
            renderer.render(scene, camera);
        }

        // Initialize game
        animate();
    </script>
</body>
</html>
