---
title: "O3D Extension for WebSharper available"
categories: "o3d,webgl,f#,websharper"
abstract: "In our continuing effort to make the most powerful JavaScript tools available to WebSharper developers, we are releasing the extension for Google O3D. O3D is a 3D programming library built on top of WebGL, enabling high-level resource and rendering management for 3D scenes."
identity: "2060,74892"
---
In our continuing effort to make the most powerful JavaScript tools available to WebSharper developers, we are releasing the extension for Google O3D. O3D is a 3D programming library built on top of WebGL. Its functionalities include:

 * A render graph allowing hierarchical management of objects, views and rendering passes;
 * Resource management: loading of individual resources (textures, models) or JSON object descriptions, convertible from COLLADA;
 * Algebraic operations for matrices, vectors and quaternions, integrated in the render graph;
 * Skinning primitives for bone-based animation and deformation.
The following sample, displaying a rotating cube, shows how O3D abstracts away most of WebGL.

The following sample, displaying a rotating cube, shows how O3D abstracts away most of WebGL.

```fsharp
[<JavaScript>]
let vertexShaderString = "
    attribute vec4 position;
    uniform mat4 world;
    uniform mat4 view;
    uniform mat4 projection;

    void main() {
        gl_Position = projection * view * world * position;
    }"

[<JavaScript>]
let pixelShaderString = "
    void main() {
        gl_FragColor = vec4(1, 0, 0, 1);  // Red.
    }"

[<JavaScript>]
let createCube (pack : O3D.Pack) (material : O3D.Material) =
    let cubeShape = pack.CreateShape()
    let streamBank = pack.CreateStreamBank()
    let positionsArray = [|
        -0.5; -0.5;  0.5;    0.5; -0.5;  0.5;  // vertex 0, 1
        -0.5;  0.5;  0.5;    0.5;  0.5;  0.5;  // vertex 2, 3
        -0.5;  0.5; -0.5;    0.5;  0.5; -0.5;  // vertex 4, 5
        -0.5; -0.5; -0.5;    0.5; -0.5; -0.5;  // vertex 6, 7
    |]
    let indicesArray = [|
        0; 1; 2;   2; 1; 3;  // face 1
        2; 3; 4;   4; 3; 5;  // face 2
        4; 5; 6;   6; 5; 7;  // face 3
        6; 7; 0;   0; 7; 1;  // face 4
        1; 7; 3;   3; 7; 5;  // face 5
        6; 0; 4;   4; 0; 2;  // face 6
    |]
    let positionsBuffer = pack.CreateVertexBuffer()
    let positionsField = positionsBuffer.CreateFloatField(3)
    positionsBuffer.Set positionsArray |> ignore
    let indexBuffer = pack.CreateIndexBuffer()
    indexBuffer.Set indicesArray |> ignore
    streamBank.SetVertexStream(O3D.Stream.POSITION, 0, positionsField, 0) |> ignore
    let cubePrimitive = pack.CreatePrimitive(Material = material,
                                             Owner = cubeShape,
                                             StreamBank = streamBank,
                                             primitiveType = O3D.Primitive.TRIANGLELIST,
                                             NumberPrimitives = 12,
                                             NumberVertices = 8,
                                             IndexBuffer = indexBuffer)
    cubePrimitive.CreateDrawElement(pack, null)
    cubeShape

[<JavaScript>]
let g_clock = ref 0.

[<JavaScript>]
let renderCallback (cubeTransform : O3D.Transform) (event : O3D.RenderEvent) =
    g_clock := !g_clock + event.ElapsedTime
    cubeTransform.Identity()
    cubeTransform.RotateY(2. * float !g_clock)

[<JavaScript>]
let Samples() =
    Div [Attr.Id "o3d"; Attr.Style "width: 600px; height: 600px;"]
    |>! OnAfterRender (fun d ->
        O3DJS.Webgl.MakeClients (fun clients ->
            let client = As<O3D.Client> (JavaScript.Get "client" clients.[0])
            let pack = client.CreatePack()
            let viewInfo = O3DJS.Rendergraph.CreateBasicView(pack, client.Root,
                                                             client.RenderGraphRoot)
            viewInfo.DrawContext.Projection <-
                O3DJS.Math.Matrix4.Perspective(O3DJS.Math.DegToRad 30.,
                                               float client.Width/float client.Height,
                                               1., 5000.)
            viewInfo.DrawContext.View <- O3DJS.Math.Matrix4.LookAt((0., 1., 5.),
                                                                   (0., 0., 0.),
                                                                   (0., 1., 0.))
            let redEffect = pack.CreateEffect()
            redEffect.LoadVertexShaderFromString vertexShaderString |> ignore
            redEffect.LoadPixelShaderFromString pixelShaderString |> ignore
            let redMaterial = pack.CreateMaterial(DrawList = viewInfo.PerformanceDrawList,
                                                  Effect = redEffect)
            let cubeShape = createCube pack redMaterial
            let cubeTransform = pack.CreateTransform(Parent = client.Root)
            cubeTransform.AddShape cubeShape
            client.SetRenderCallback (renderCallback cubeTransform))
    )
```

<img src="http://www.intellifactory.com/ShowDigitalAsset.aspx?DigitalAsset=194">

You can download the WebSharper Extension for O3D at [this address](http://www.websharper.com/extension/358-o3d/Some/2.3.16).
