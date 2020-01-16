# federatedmodels-v7
`load a federated set of models, or multi-model load in forge viewer v7`

See Blog post: [**forge.autodesk.com/blog**]

![multimodel](https://user-images.githubusercontent.com/440241/72572879-c163de00-3878-11ea-8abb-af8d20384df7.jpg)


### Introduction

With version 7 of the viewer, there is a new way to load multiple models with `loadDocumentNode`.

In version 6, it was just a matter of calling `loadModel` without calling `teardown()`, but little did I know, that no longer works!

### Details
Now, to load multiple models, use `loadDocumentNode` as normal, but when loading the second or third models, make sure you add the viewer option `keepCurrentModels: true`, like this:

To load an array of URNs (models), I can put it in a loop:

```
loadModels([
        { urn: "dXJuOmFkc2sub2JqZWN0czpvcy5vYmplY3Q6dnJwYXJ0eTIvcnN0X2Jhc2ljX3NhbXBsZV9wcm9qZWN0LnJ2dA", xform: {x:-60,y:0,z:0} },
        { urn: "dXJuOmFkc2sub2JqZWN0czpvcy5vYmplY3Q6dnJwYXJ0eTIvNDMyJTIwTmFwYS5ydnQ", xform: {x:60,y:0,z:0} },
        { urn: "dXJuOmFkc2sub2JqZWN0czpvcy5vYmplY3Q6dnJwYXJ0eTEvcmFjLnJ2dA", xform: {x:50,y:0,z:-50} },
        { urn: "dXJuOmFkc2sub2JqZWN0czpvcy5vYmplY3Q6dnJwYXJ0eTEvcmFjLnJ2dA", xform: {x:-50,y:0,z:-50} },
 ])


function loadModels(urns) {

    const viewerOptions = {
        env: 'AutodeskProduction',
        accessToken: _adsk.token.access_token,
        extensions:[ ]
    };

    Autodesk.Viewing.Initializer(viewerOptions, () => {

        const div = document.getElementById('forgeViewer');
        viewer = new Autodesk.Viewing.Private.GuiViewer3D(div);
        viewer.start();
        urns.map((m)=>{
            Autodesk.Viewing.Document.load(`urn:${m.urn}`, (doc) => {
                var viewables = doc.getRoot().getDefaultGeometry();
                viewer.loadDocumentNode(doc, viewables,{
                    placementTransform: (new THREE.Matrix4()).setPosition(m.xform),
                    keepCurrentModels: true,
                })
                .then( onLoadFinished );
            });

        })
    });

    function onLoadFinished(doc) {
        console.log('loaded');
    }
```

See a working example here: [LIVE LINK]



 

