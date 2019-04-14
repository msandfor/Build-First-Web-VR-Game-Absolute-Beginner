# Build your First Web VR Game for the Absolute Beginner
This is source and tutorial for building your first website and first web vr game - all in one!

### Download VS Code, create index.html and index.js files
1. First things first. We need the right tools for the right job. (Download VsCode)[] if you dont already have it.
2. Open VS code and create a new file called index.html
3. Copy and paste this html and css into the file. 
```html <!DOCTYPE html>
<html>

    <head>
        <style>
            html,
            body {
                overflow: hidden;
                width: 100%;
                height: 100%;
                margin: 0;
                padding: 0;
                text-align: center;
            }

            #renderCanvas {
                width: 100%;
                height: 100%;
                touch-action: none;
            }
        </style>
        
        <!-- Babylon.js -->
        <script src="https://code.jquery.com/pep/0.4.2/pep.min.js"></script>
        <script src="https://cdnjs.cloudflare.com/ajax/libs/dat-gui/0.6.2/dat.gui.min.js"></script>
        <script src="https://preview.babylonjs.com/ammo.js"></script>
        <script src="https://preview.babylonjs.com/cannon.js"></script>
        <script src="https://preview.babylonjs.com/Oimo.js"></script>
        <script src="https://preview.babylonjs.com/gltf_validator.js"></script>
        <script src="https://preview.babylonjs.com/earcut.min.js"></script>
        <script src="https://preview.babylonjs.com/babylon.js"></script>
        <script src="https://preview.babylonjs.com/inspector/babylon.inspector.bundle.js"></script>
        <script src="https://preview.babylonjs.com/materialsLibrary/babylonjs.materials.min.js"></script>
        <script src="https://preview.babylonjs.com/proceduralTexturesLibrary/babylonjs.proceduralTextures.min.js"></script>
        <script src="https://preview.babylonjs.com/postProcessesLibrary/babylonjs.postProcess.min.js"></script>
        <script src="https://preview.babylonjs.com/loaders/babylonjs.loaders.js"></script>
        <script src="https://preview.babylonjs.com/serializers/babylonjs.serializers.min.js"></script>
        <script src="https://preview.babylonjs.com/gui/babylon.gui.min.js"></script>
    </head>

    <body>
        <canvas id="renderCanvas"></canvas>
        <script src="index.js"></script>
    </body>

</html>
```
4. Now create the index.js file and copy and paste this code into it.
```javascript
var canvas = document.getElementById("renderCanvas");
var engine = new BABYLON.Engine(canvas, true);

function createScene() {
    // Create scene
    var scene = new BABYLON.Scene(engine);

    // Add Camera
    var camera = new BABYLON.UniversalCamera("UniversalCamera", new BABYLON.Vector3(0, 0, 0), scene);
    // Targets the camera to a particular position. In this case the scene origin
    camera.setTarget(BABYLON.Vector3.Zero());
    // Attach the camera to the canvas
    camera.attachControl(canvas, true);

    // Add Light
    var light = new BABYLON.HemisphericLight("light", new BABYLON.Vector3(1, 1, 0), scene);

    // Create sphere
    var sphere = BABYLON.MeshBuilder.CreateSphere("sphere", { diameter: 5 }, scene);
    sphere.position.y = 10;
    sphere.position.x = -10;
    sphere.position.z = -10;
    sphere.material = new BABYLON.StandardMaterial("sphere material", scene);

    // Create Ground
    var ground = BABYLON.Mesh.CreateBox("Ground", 1, scene);
    ground.scaling = new BABYLON.Vector3(100, 1, 100);
    ground.position.y = -10.0;
    ground.checkCollisions = true;

    //Ground material
    var groundMat = new BABYLON.StandardMaterial("groundMat", scene);
    groundMat.diffuseColor = new BABYLON.Color3(0.5, 0.5, 0.5);
    groundMat.emissiveColor = new BABYLON.Color3(0.2, 0.2, 0.2);
    groundMat.backFaceCulling = false;
    ground.material = groundMat;
  
    // Enable VR
    var vrHelper = scene.createDefaultVRExperience();
    vrHelper.enableInteractions();

    return scene;

}

var scene = createScene();

engine.runRenderLoop(() => {
    scene.render();
});
```
5. Save file and navigate to the file explorer. Double click the index.html (browswer will open ooo ahhh I made a site)

## Now lets add gravlity to our site
1. To add gravity to our game we need to add the physics engine. The physics engine we will use is Cannonjs and we already imported the library with our script tags.
```
 <script src="https://preview.babylonjs.com/cannon.js"></script>
```
2. Add gravity to the camera by pasting this code below `camera.attachControl(canvas, true);` on `line 16`
```javascript
    // Apply Gravity to Camera
    camera.applyGravity = true;
```
3. Enable physics and set the gravitational force with a vector on `line 22`
```javascript
    // Enable Physics and set gravtiy force with a vector
    var gravityVector = new BABYLON.Vector3(0, -10, 0);
    scene.enablePhysics(gravityVector, new BABYLON.CannonJSPlugin());
```
3. Add a `physicsImposter` to the `sphere` on `line 32`
```javascript
sphere.physicsImpostor = new BABYLON.PhysicsImpostor(sphere, BABYLON.PhysicsImpostor.SphereImpostor, { mass: 1, restitution: 0.9 }, scene);
```
4. Add a `physicsImposter` to the `ground` on `line 46`
```javascript
ground.physicsImpostor = new BABYLON.PhysicsImpostor(ground, BABYLON.PhysicsImpostor.BoxImpostor, { mass: 0, friction: 0.5, restitution: 0.7 }, scene);
```
5. Save the changes and refresh the browser. You should now see the sphere fall from the sky and bounce on the ground.
