---


---

<h1 id="how-polygonal-graphics-are-made">How Polygonal Graphics Are Made</h1>
<p>Like atoms, most computer-generated 3D objects are built with polygons, collections of triangles, or squares connected together in 3D space to approximate shapes. In this article, we will explore how computers see polygons out of information and turn that into graphics like you would see in a movie or video game. We will be using <a href="https://www.babylonjs.com/">Babylon.js</a>, an in-browser rendering engine that uses WebGL, to visualize some of the concepts discussed.</p>
<h3 id="d-from-2d">3D from 2D</h3>
<p>Computers handle 2D images really well. To take advantage of this property, 3D models are most commonly built from triangles or less commonly, from quadrilaterals arranged as faces of a solid. Shapes like spheres and cylinders can not be broken down into discrete triangles, so we approximate them as polygons with many faces.</p>
<p>These faces are constructed by vertices made up of points. We can visualize these vertices with wireframes, where vertices are highlighted and faces are transparent. For example, here is a <a href="https://playground.babylonjs.com/#T4TNWL">wireframe of a sphere</a>.</p>
<p><img src="https://i.imgur.com/gUnhTu4.png" alt="Sphere Wireframe"><br>
<em>Wireframe of a sphere</em></p>
<p>To explore how changing the number of vertices changes the approximation of the solid, try experimenting with the number of <a href="https://playground.babylonjs.com/#VR8AHB">tessellations on this cylinder</a>. Change the value of <code>poly</code> up or down to see how creating more subdivisions in a shape increases quality. The higher the vertext count, the more memory the shape takes up, and resources it needs to be operated on. This is why early movies and video games have models with much less detail than they do today.<br>
<img src="https://i.imgur.com/ScRkwYv.png" alt="7 divs"><br>
<em>A cylinder with 7 subdivisions on the top face</em><br>
<img src="https://i.imgur.com/jd9XR20.png" alt="20_poly"><br>
<em>A cylinder with 20 subdivisions on the top face</em><br>
<img src="https://i.imgur.com/3KZLqAe.png" alt="100 divs"><br>
<em>A cylinder with 100 subdivisions on the top face</em></p>
<h3 id="textures">Textures</h3>
<p>Once we define the triangles that make up the shape, known as a mesh, we can draw the faces over the triangles. This is known as texturing. To elaborate on this, let’s look at an <a href="https://playground.babylonjs.com/#T40FK#128">example of texturing faces of a cube</a> created by a <a href="https://www.html5gamedevs.com/topic/12392-having-different-textures-for-each-face-on-a-cube/">user</a> on the HTML5GameDevs Forums. The example shows applying different colors to each face of the cube with this code:</p>
<pre class=" language-javascript"><code class="prism  language-javascript"><span class="token operator">...</span>
<span class="token comment">// Stores colors in materials (textures)</span>
<span class="token keyword">var</span> f<span class="token operator">=</span><span class="token keyword">new</span>  <span class="token class-name">BABYLON<span class="token punctuation">.</span>StandardMaterial</span><span class="token punctuation">(</span><span class="token string">"material0"</span><span class="token punctuation">,</span>scene<span class="token punctuation">)</span><span class="token punctuation">;</span>
f<span class="token punctuation">.</span>diffuseColor<span class="token operator">=</span><span class="token keyword">new</span>  <span class="token class-name">BABYLON<span class="token punctuation">.</span>Color3</span><span class="token punctuation">(</span><span class="token number">0.75</span><span class="token punctuation">,</span><span class="token number">0</span><span class="token punctuation">,</span><span class="token number">0</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
<span class="token keyword">var</span> ba<span class="token operator">=</span><span class="token keyword">new</span>  <span class="token class-name">BABYLON<span class="token punctuation">.</span>StandardMaterial</span><span class="token punctuation">(</span><span class="token string">"material1"</span><span class="token punctuation">,</span>scene<span class="token punctuation">)</span><span class="token punctuation">;</span>
ba<span class="token punctuation">.</span>diffuseColor<span class="token operator">=</span><span class="token keyword">new</span>  <span class="token class-name">BABYLON<span class="token punctuation">.</span>Color3</span><span class="token punctuation">(</span><span class="token number">0</span><span class="token punctuation">,</span><span class="token number">0</span><span class="token punctuation">,</span><span class="token number">0.75</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
<span class="token operator">...</span>
<span class="token comment">// Combines the materials into a multi-material</span>
multi<span class="token punctuation">.</span>subMaterials<span class="token punctuation">.</span><span class="token function">push</span><span class="token punctuation">(</span>f<span class="token punctuation">)</span><span class="token punctuation">;</span>
multi<span class="token punctuation">.</span>subMaterials<span class="token punctuation">.</span><span class="token function">push</span><span class="token punctuation">(</span>ba<span class="token punctuation">)</span><span class="token punctuation">;</span>
<span class="token operator">...</span>
<span class="token comment">//Applys a material to a set of faces. </span>
<span class="token comment">// The first two parameters define the submesh index. The third is the total</span>
<span class="token comment">// number of vertices, and the fourth and fifth are the bounds of the vertex </span>
<span class="token comment">// that the mesh is applied to by subdivisions. The last is the mesh to be</span>
<span class="token comment">// subdivided.</span>
box<span class="token punctuation">.</span>subMeshes<span class="token punctuation">.</span><span class="token function">push</span><span class="token punctuation">(</span><span class="token keyword">new</span>  <span class="token class-name">BABYLON<span class="token punctuation">.</span>SubMesh</span><span class="token punctuation">(</span><span class="token number">0</span><span class="token punctuation">,</span> <span class="token number">0</span><span class="token punctuation">,</span> verticesCount<span class="token punctuation">,</span> <span class="token number">0</span><span class="token punctuation">,</span> <span class="token number">6</span><span class="token punctuation">,</span> box<span class="token punctuation">)</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
box<span class="token punctuation">.</span>subMeshes<span class="token punctuation">.</span><span class="token function">push</span><span class="token punctuation">(</span><span class="token keyword">new</span>  <span class="token class-name">BABYLON<span class="token punctuation">.</span>SubMesh</span><span class="token punctuation">(</span><span class="token number">1</span><span class="token punctuation">,</span> <span class="token number">1</span><span class="token punctuation">,</span> verticesCount<span class="token punctuation">,</span> <span class="token number">6</span><span class="token punctuation">,</span> <span class="token number">6</span><span class="token punctuation">,</span> box<span class="token punctuation">)</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
<span class="token operator">...</span>
</code></pre>
<p><img src="https://i.imgur.com/r7p2oYC.png" alt="standard cube"><br>
<em>The cube with the material applied to all faces</em></p>
<p>Try changing the <code>6</code> in the line</p>
<pre class=" language-javascript"><code class="prism  language-javascript"> box<span class="token punctuation">.</span>subMeshes<span class="token punctuation">.</span><span class="token function">push</span><span class="token punctuation">(</span><span class="token keyword">new</span>  <span class="token class-name">BABYLON<span class="token punctuation">.</span>SubMesh</span><span class="token punctuation">(</span><span class="token number">0</span><span class="token punctuation">,</span> <span class="token number">0</span><span class="token punctuation">,</span> verticesCount<span class="token punctuation">,</span> <span class="token number">0</span><span class="token punctuation">,</span> <span class="token number">6</span><span class="token punctuation">,</span> box<span class="token punctuation">)</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
</code></pre>
<p>to a <code>5</code>. In this example, we tell the mesh not to apply the material to one of the faces.</p>
<p><img src="https://i.imgur.com/zj1ZdBG.png" alt="sube"><br>
<em>Cube with the 6 changed to a 5</em></p>
<h3 id="transformations">Transformations</h3>
<p>Since the end result of most 3D graphics need to be displayed on a 2D screen from a fixed viewpoint, 3D models need to be able to be rotated and moved in respect to the viewer. This is done with transformation matrices. In essence, these matrices define how any given point would be rotated. This matrix is then multiplied by the matrix containing the vertices of the shape to define new positions for each point. Due to the ease of using transformation matrices, it is far easier to move and transform all 3D models around the viewer than to move the viewer themselves.</p>
<h3 id="wrap-up">Wrap Up</h3>
<p>Polygonal objects are the backbone of 3D graphics, and are worth understanding to better optimize your graphical projects or find ways to increase detail.</p>

