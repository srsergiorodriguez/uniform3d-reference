# Uniform 3D Unofficial Reference Manual

Uniform 3D is a comprehensive, multi-purpose 3D modeling and texturing application designed specifically for the iPad by Sparseal, a company founded by former Blender sculpting tools lead Pablo Dobarro and Godot Engine developer Joan Fons. Billed as an "all-in-one tool for creating game-ready assets", Uniform discards the fragmented workspace approach of traditional desktop software in favor of a unified pipeline architecture. Within a single environment, artists can generate high-poly sculpts, build low-poly polygon cages, sketch gesture-based retopology (incorporating Sparseal's CozyBlanket toolset), unwrap UVs, and paint multi-channel PBR textures. The application features a real-time, GPU-accelerated renderer supporting physical lighting and materials, and allows users to apply the exact same brush toolset across dense displacement sculpts, lightweight low-poly meshes, and 2D canvas layers simultaneously. Engineered around a strict two-handed hardware paradigm—separating touch navigation from Apple Pencil spatial manipulation—Uniform aims to deliver a desktop-class asset creation workflow optimized entirely for tablet ergonomics.

## Table of Contents

- [Uniform 3D Unofficial Reference Manual](#uniform-3d-unofficial-reference-manual)
  - [Table of Contents](#table-of-contents)
  - [Part I: Core Philosophy \& Data Architecture (Foundations)](#part-i-core-philosophy--data-architecture-foundations)
    - [1. The Core Philosophy: Pipelines in a Box](#1-the-core-philosophy-pipelines-in-a-box)
      - [Conceptual Tools vs. Data-Restricted Tools](#conceptual-tools-vs-data-restricted-tools)
    - [2. Structural Ontology: Dissecting Uniform's Building Blocks](#2-structural-ontology-dissecting-uniforms-building-blocks)
      - [1. Scene Object vs. Data Object (The "Tree" vs. "Teapot" Tools)](#1-scene-object-vs-data-object-the-tree-vs-teapot-tools)
      - [2. What is a "Graph"?](#2-what-is-a-graph)
    - [3. Geometry Sub-Elements: Regions, Seams, and Masks](#3-geometry-sub-elements-regions-seams-and-masks)
    - [4. The Viewport Overlay System: Context-Driven Rendering](#4-the-viewport-overlay-system-context-driven-rendering)
  - [Part II: Hardware, Interface \& Workspace Configuration (The Environment)](#part-ii-hardware-interface--workspace-configuration-the-environment)
    - [1. The Input Paradigm: Hand Separation](#1-the-input-paradigm-hand-separation)
    - [2. The Core Workspace Interface](#2-the-core-workspace-interface)
    - [3. The Left Rail \& Hardware Modifiers](#3-the-left-rail--hardware-modifiers)
      - [Session \& Environment Management](#session--environment-management)
      - [Parameter Tweaking: The Factor Slider](#parameter-tweaking-the-factor-slider)
      - [Dynamic Hardware Modifiers (Mod 0, Mod 1, Mod 2)](#dynamic-hardware-modifiers-mod-0-mod-1-mod-2)
    - [4. Workspace Customization \& HUD Ergonomics](#4-workspace-customization--hud-ergonomics)
      - [Customizing the Interface](#customizing-the-interface)
      - [Ergonomics \& Left-Handed Drawing](#ergonomics--left-handed-drawing)
    - [5. Multi-Camera Viewports \& Reference Boards](#5-multi-camera-viewports--reference-boards)
      - [Multi-Camera Viewports](#multi-camera-viewports)
      - [Reference Boards \& Palettes](#reference-boards--palettes)
  - [Part III: Geometry Creation, Editing \& Retopology (Modeling)](#part-iii-geometry-creation-editing--retopology-modeling)
    - [1. Base Geometry Types \& The Conversion Pipeline](#1-base-geometry-types--the-conversion-pipeline)
      - [Base Generation](#base-generation)
      - [The Explicit Conversion Operations](#the-explicit-conversion-operations)
      - [Understanding ALBs (Surface Data Points)](#understanding-albs-surface-data-points)
    - [2. The Curve Pipeline (PolyCurve \& GeoCurve)](#2-the-curve-pipeline-polycurve--geocurve)
      - [PolyCurves (Structural Graph Curves)](#polycurves-structural-graph-curves)
      - [GeoCurves (Dense Point Curves)](#geocurves-dense-point-curves)
    - [3. Managing Selections \& Loop Transformations](#3-managing-selections--loop-transformations)
      - [Scaling Topological Loops](#scaling-topological-loops)
      - [Gesture-Based Selection](#gesture-based-selection)
    - [4. Implicit Posing: Rigging Without Skeletons](#4-implicit-posing-rigging-without-skeletons)
      - [The Two Implicit Rig Providers](#the-two-implicit-rig-providers)
      - [Pose Transformation Kernels](#pose-transformation-kernels)
    - [5. Gesture-Driven Retopology Architecture](#5-gesture-driven-retopology-architecture)
      - [The Underlying Pipeline](#the-underlying-pipeline)
      - [Initializing the Retopology Mesh](#initializing-the-retopology-mesh)
      - [Surface Quad Creation \& Expansion](#surface-quad-creation--expansion)
      - [Interactive Topology Modification](#interactive-topology-modification)
  - [Part IV: Surfacing, UVs \& PBR Pipelines (Texturing)](#part-iv-surfacing-uvs--pbr-pipelines-texturing)
    - [1. Topological Seams \& Automated UV Unwrapping](#1-topological-seams--automated-uv-unwrapping)
      - [Marking \& Erasing Seams](#marking--erasing-seams)
      - [Global Gestures (Background Swipes)](#global-gestures-background-swipes)
    - [2. Executing the Unwrap \& The UV Layout Scene](#2-executing-the-unwrap--the-uv-layout-scene)
    - [3. The Layer Stack \& Physical Domains](#3-the-layer-stack--physical-domains)
      - [Layer Types](#layer-types)
      - [The Physical Domains Architecture](#the-physical-domains-architecture)
    - [4. Integrating External Images](#4-integrating-external-images)
    - [5. Masking \& Domain Protection](#5-masking--domain-protection)
      - [Interactive Painting \& Viewport Clipping](#interactive-painting--viewport-clipping)
      - [Algorithmic Mask Operators \& Domain Filters](#algorithmic-mask-operators--domain-filters)
      - [Procedural Influence Providers (Geometry-Aware Masking)](#procedural-influence-providers-geometry-aware-masking)
      - [Domain Safety \& Cross-Contamination Overrides](#domain-safety--cross-contamination-overrides)
  - [Part V: Procedural Systems \& Advanced Tool Customization (Under the Hood)](#part-v-procedural-systems--advanced-tool-customization-under-the-hood)
    - [1. Evaluator Stack vs. Global Overrides: The Rosetta Stone](#1-evaluator-stack-vs-global-overrides-the-rosetta-stone)
    - [2. Essential Module Configurations Matrix](#2-essential-module-configurations-matrix)
    - [3. Advanced Tool Customization: Building Custom Brushes](#3-advanced-tool-customization-building-custom-brushes)
      - [Core Module Library](#core-module-library)
        - [Input Filters (Stroke Interpolation)](#input-filters-stroke-interpolation)
        - [Evaluators (Hardware Dynamics)](#evaluators-hardware-dynamics)
        - [Brush Overlays (Parameter Modulation)](#brush-overlays-parameter-modulation)
        - [Brush Samplers (Stroke Generation)](#brush-samplers-stroke-generation)
        - [Falloff Providers (Brush Tip Profiling)](#falloff-providers-brush-tip-profiling)
        - [Surface Influence Providers (The Masking Engine)](#surface-influence-providers-the-masking-engine)
        - [Brush Kernels (The Execution Payload)](#brush-kernels-the-execution-payload)
      - [Building Stylized Brushes](#building-stylized-brushes)
  - [Appendices](#appendices)
    - [Appendix A: Master Tool \& Modifier Reference](#appendix-a-master-tool--modifier-reference)
  - [Appendix A: Master Tool, Action, and Resource Reference](#appendix-a-master-tool-action-and-resource-reference)


## Part I: Core Philosophy & Data Architecture (Foundations)

### 1. The Core Philosophy: Pipelines in a Box

To navigate Uniform 3D efficiently, you must set aside the segmented workflow typical of traditional desktop 3D software such as Blender, Maya, or 3ds Max. In standard applications, project data is fragmented across isolated modes and dedicated tabbed workspaces:

* Constructing a polygon cage requires entering a dedicated **Edit Mode**.
* Flattening seams requires transitioning to an independent **UV Workspace**.
* Carving high-frequency detail demands switching to a **Sculpt Mode** or initializing a dynamic subdivision grid.
* Surfacing requires entering a **Texture Paint Workspace** to project colors onto pre-allocated 2D bitmaps.
* Integrating these parts requires opening a node-based **Shader Editor** to wire normal maps, roughness channels, and albedo maps into a material shader.

```text
Traditional 3D Software Architecture:
[Polygon Mesh] ---> [UV Islands] ---> [2D Image Textures] ---> [Shader Graph] ---> [Render Engine]
  (Workspace 1)       (Workspace 2)        (Workspace 3)          (Workspace 4)

Uniform 3D: Pipeline-in-a-Box
+-----------------------------------------------------------------------------------------------+
| Object: PolyMesh / GeoMesh (Self-Contained Pipeline Container)                                |
| [ Topology Graph <-> Auto/Manual UVs <-> Multi-Layer PBR Canvas <-> Real-Time Normal Shader ] |
+-----------------------------------------------------------------------------------------------+
                     ^
       Abstract Artistic Intent: "Paint Color" / "Draw Displacement"
       (The tool evaluates directly across the entire internal pipeline)

```

In Uniform 3D, **an Object is not merely a geometric mesh; it is an entire, self-contained production pipeline packaged inside a single container**. An object encapsulates its own topological graph, high-density sculpted displacement data, automated UV layouts, multi-channel PBR layer stacks, and real-time shader evaluation simultaneously.

#### Conceptual Tools vs. Data-Restricted Tools

Because every object operates as an integrated pipeline, tools in Uniform 3D do not target narrow, rigid data types. Instead, they represent **abstract artistic intents** that automatically evaluate across whichever stage of the object pipeline is currently active:

* **The Paint Color Tool:** In traditional software, a brush must be explicitly programmed as either a "Vertex Color Painter" or a "Texture Projection Painter". In Uniform, **Paint Color** simply instructs the engine: *"apply pigment to this asset"*. If targeted at a dense, sculpted **GeoMesh**, the brush writes directly into the underlying surface point colors (the ALB points). If targeted at a low-poly **PolyMesh**, the exact same brush targets the object's active layer stack and paints across the real-time PBR canvas texture map.
* **The Displacement Tool:** If applied to a high-resolution **GeoMesh**, displacement brushes (like **Draw Displacement** or **Clay**) physically offset 3D surface points along normal vectors. If applied to a basic, 8-vertex low-poly **PolyMesh** cube, the brush does not distort the coarse geometry; it evaluates your stroke as virtual geometric height and writes it directly to the active normal map channel in real time.

---

### 2. Structural Ontology: Dissecting Uniform's Building Blocks

Understanding how Uniform organizes scenes prevents workflow friction. Data is structured in a strict hierarchy:

```text
[ Workspace File ]
       │
       └─── [ Scene Object ] (World Stage, Transform Hierarchies, Light Fields)
                    │
                    └─── [ Scene Nodes / Instances ]
                                 │
                                 └─── [ Data Object Pipeline ] (PolyMesh, GeoMesh, Curve)
                                              │
                                              ├─── Graph Layout (Vertices, Edges, Faces, Loops)
                                              ├─── Regions (Integer ID Face Sets / Polygroups)
                                              ├─── Surface / ALBs (High-Res Sculpt Points)
                                              └─── Domains & Layer Stack (PBR Textures, Masks)

```

#### 1. Scene Object vs. Data Object (The "Tree" vs. "Teapot" Tools)

Uniform divides spatial transformations into two distinct levels. You must verify which level you are editing by checking the icon located in the active tool header at the top of the interface:

* **The Scene Transform Tool (Tree Icon):** Represents the world stage and coordinate layout. It governs spatial placement (world position and orientation), parent-child relationships, environment light fields, and scene instances. The "points" evaluated by a Scene Object are not mesh vertices; they are the central origin points of the individual Data Objects placed within the room. Using a Transform tool marked with a **Tree icon** translates or rotates the entire object container as an actor on the stage.
* **The Data Transform Tool (Teapot Icon):** Represents the raw geometric and material content living inside the scene node. Using a Transform tool marked with a **Teapot icon** alters the actual selected geometry vertices, curves, or volumetric fields in local object space, while leaving the object’s world-space origin point completely stationary. Look at Part III section "Gesture-Based Selection" for more about how to select vertices for applying localized transforms.

> **About scaling:**
> In Uniform 3D, **an Object's Scene Transform cannot be scaled**. Scene node transforms are strictly locked to a uniform scale factor of **1.0**. If you attempt to scale an object using the **Scene Transform** (Tree) tool, the engine will safely ignore the operation. To alter the physical dimensions of a model, you must use the **Data Transform** (Teapot) tool, which scales the underlying vertices within the local data container relative to one another.
> 
> 

#### 2. What is a "Graph"?

In Uniform, the term **Graph** refers exclusively to **explicit topological networks** (built on a Doubly Connected Edge List network). A Graph consists of discrete vertices connected by directed edges, bounded into closed polygonal faces and continuous edge loops:

* A **PolyMesh** and a **PolyCurve** possess a Graph: you can insert edge loops, slide vertices along quad rails, dissolve edges, and perform face extrusions.
* A raw **GeoMesh** or **Volume Group** does **not** contain an accessible Graph. Instead, they are represented as a dense spatial continuum of point-sampled surface elements (ALBs) or mathematical Signed Distance Fields (SDF). Graph-specific tools (such as **Loop Slide** or **Extrude**) will not execute on a GeoMesh or Volume Group because there are no structural edge loops or polygon faces to evaluate.

---

### 3. Geometry Sub-Elements: Regions, Seams, and Masks

Within an active Graph, surface selection and tool isolation are governed by three primary sub-element types:

* **Regions:** Polygonal face groupings tagged with unique integer identification values. Regions function identically to "Face Sets" in Blender or "Polygroups" in ZBrush. When using topology tools such as **Extrude**, **Delete Region**, or **Draw Boundary**, the engine evaluates the perimeter of the Region rather than individual face selections. Regions allow complex topological operations to be performed across massive surface patches with a single gesture.
* **Seams:** Specific topological edges tagged with a persistent marker using the **Mark Seam** tool. Marked seams render as prominent red lines across the geometry in the viewport. They serve one primary role: defining the perimeter cuts used by the unwrapping engine when executing an automated **Surface Rebuild** to generate UV layouts.
* **Selection / Masks:** Floating-point greyscale fields ranging from **0.0** to **1.0**, painted directly across vertices, ALB surface points, or layer masks using the **Set Selection** brush. Masks temporarily isolate or modulate tool influence: a value of **1.0** (white) receives full tool deformation or paint intensity, while a value of **0.0** (black) completely protects the geometry from modification.

---

### 4. The Viewport Overlay System: Context-Driven Rendering

Uniform 3D does not rely on a universal, manual viewport display toggle (such as standard Wireframe, Solid, or X-Ray buttons found in the headers of other software). Instead, **Uniform employs an automated, context-driven overlay engine where the active tool’s requirements dictate exactly what geometry representations appear in the viewport**:

* **Solid Shading Only:** Activated automatically whenever high-level sculpt brushes (such as **Draw Displacement**, **Clay**, or **Trim**) are selected. The engine hides all wireframes, edges, and vertex markers to minimize GPU overhead and maximize Apple Pencil responsiveness across dense point fields.
* **Solid with Floating Dots:** Activated when point-level deformers, selection brushes, or direct translation tools (such as **Move** or the **Selection Editor**) are active. The dots represent interactable deformation handles or vertex coordinate targets. Unselected points display as neutral markers, while active selections illuminate as bright blue markers.
* **Highlighted Wire Edges & Face Outlines:** Triggered exclusively by topological mesh editing tools (such as the **EditMesh** toolset, **Graph Layout Cursor**, or **Mark Seam**). Because operations like loop cutting, seam marking, and face extrusions require targeting specific edges and quad boundaries, the tool forces the viewport to render the explicit wire graph over the shaded geometry.
* **Region Color Overlays:** Activated by region-management tools (such as the **Region Editor**). The engine suppresses standard PBR material shading and renders the geometry using distinct, bright flat colors assigned to each individual Region ID. This allows for instant visualization of discrete topological patches before performing broad, region-based extrusions or deletions.
* **Contextual Cursors & Target Gizmos:** Specific tools trigger specialized cursor overlays to preview your exact impact area before committing a stroke. Examples include the **Surface Cursor** (displaying the brush radius and snapping to the closest ALB point), the **EditMesh Element Gizmo** (highlighting the specific face, edge, or vertex actively targeted by the Pencil), and standard transform arrow gizmos for spatial manipulation.


---

## Part II: Hardware, Interface & Workspace Configuration (The Environment)

### 1. The Input Paradigm: Hand Separation

Uniform 3D is engineered specifically for a two-handed mobile and tablet workflow. To prevent accidental mesh modifications while navigating and to eliminate palm-rejection errors, the engine tracks touch inputs and stylus inputs through strictly isolated hardware channels:

* **Touch (Fingers):** Dedicated exclusively to interface interaction and viewport navigation. You use your fingers to orbit, pan, and zoom the camera, drag UI sliders, and hold the modifier pads (**Mod 0–2**).
* **Apple Pencil (Stylus):** Dedicated exclusively to spatial data manipulation. The Pencil handles viewport mesh editing, sculpting strokes, vertex dragging, seam drawing, and direct spatial manipulation, utilizing the hardware's tip pressure to dynamically modulate stroke depth and deformation speed.

---

### 2. The Core Workspace Interface

The default workspace maximizes the central 3D viewport while maintaining immediate access to critical tools through two flanking side rails:

* **The Left Rail (Modifiers & Parameters):** Controlled by your non-dominant off-hand. It houses the **Main Menu**, the **Tool Gallery**, the **Factor Slider** (for brush strength), and the three dynamic hardware modifiers that alter your active tool's behavior on the fly.
* **The Right Rail (Tools & Navigation):** Controlled by your dominant drawing hand. It contains the **Quick Launcher** for pinned tools and folders, the radial **Grid Overlay** toggle, the **Radius Control** strip for real-time brush sizing, and quick viewport toggles like **Undo** and **Surface View**.

---

### 3. The Left Rail & Hardware Modifiers

The left rail functions as a dynamic modulation engine rather than a static toolbar, altering its state and labels based on the specific tool you are currently holding.

*(Note: For an exhaustive breakdown of every individual tool, its specific modules, and complete usage instructions, please refer to the supplementary Uniform 3D Tool Specification & Usage Guide document.)*

> **Note:** For an exhaustive breakdown of every individual tool, its specific modules, and complete usage instructions, please refer to **Appendix A: Master Tool & Modifier Reference** for the complete breakdown.

#### Session & Environment Management

* **Project Management Header:** File operations are exposed directly in the header bar via quick icons rather than a hidden dropdown menu. Use the **Folder** icon to open existing workspaces, the **Diskette** icon to save your current progress, and the **Diskette with a Plus (+)** icon to save a new copy or increment your project version.
* **Tool Gallery & Launchers:** Tapping the database icon opens the complete repository of all tools compiled into the engine, categorized logically by functional pipeline (**Surface**, **Graph**, **Deform**, **Region**, **Scene**). For rapid access during modeling, Uniform relies on customizable interface widgets called **Launchers**. The default widget on the rail is the **Grid Launcher**, but you can customize your interface using **List Launchers**, **Outliner Launchers**, or **Bar Launchers** to fit your preferred ergonomic workflow.

#### Parameter Tweaking: The Factor Slider

Represented by a power icon, this vertical touch drag area sets the working strength, flow, or procedural depth of your current operation.

* Dragging up or down with a finger adjusts the intensity from **0.0** to **1.0** without opening a sub-menu.
* Double-tapping the strip summons a numeric keypad popover for precise mathematical value entry.
* Its function is context-dependent: it scales displacement amplitude during sculpting, controls opacity during canvas painting, and adjusts convergence speed for smoothing filters.

#### Dynamic Hardware Modifiers (Mod 0, Mod 1, Mod 2)

Three dedicated modifier pads are meant to be held by your off-hand while drawing with the Apple Pencil.

* **Dynamic Feedback Labels:** The text on each button automatically updates depending on the selected tool’s specific configuration to reflect its secondary actions (e.g., switching from *Add* to *Subtract*, *Extrude* to *Inset*, or acting as a temporary smoothing override).
* **Keyboard Equivalents:** If using an external Bluetooth keyboard, these pads map directly to standard modifier keys: **Mod 0** maps to Spacebar/Shift, **Mod 1** maps to Option/Alt, and **Mod 2** maps to Command/Control.

> **Note:** Because the secondary actions of the Mod 0, Mod 1, and Mod 2 hardware pads change entirely depending on your active tool, a comprehensive matrix of every tool and its specific modifier overrides is provided at the end of this manual. Please refer to **Appendix A: Master Tool & Modifier Reference** for the complete breakdown.

---

### 4. Workspace Customization & HUD Ergonomics

Uniform 3D features a fully modular interface designed to adapt to specific production stages—such as concept blocking, fine sculpting, UV packing, or multi-angle rendering. The default mobile layout is a flexible starting configuration that can be reorganized through the **HUD Editor**.

#### Customizing the Interface

1. **Entering Edit Mode & Profiles:** Tap the **three dots** next to your profile name located on the right side of the header bar. This menu allows you to manage and create custom UI profiles, and enter edit mode to define exactly which widgets to use and where to place them on your screen.
2. **The Widget Gallery:** Once in edit mode, tap the **Widget Gallery** button (the **+** icon in the header) to open the complete catalog of dockable interface panels (including the Scene Outliner, Evaluator Inspector, Layer Stack, Volume Groups, References, Clock, and Actions-Per-Minute Tracker).
3. **Docking & Arranging:** Drag any widget from the gallery onto the screen. The interface exposes an underlying grid layout that automatically docks panels along the screen rails or anchors them into popovers.
4. **Managing Profiles:** Your layout adjustments are tied to your active UI profile. By returning to the profile menu (the three dots), you can create and switch between distinct profiles tailored for specific workflows, such as dedicated layouts for sculpting, polygon modeling, and lighting.

#### Ergonomics & Left-Handed Drawing

* **Flip UI Horizontally:** Inside the HUD editor options menu or global interface settings, selecting **Flip UI Horizontally** swaps the Left and Right rails across the screen. This positions the modifier pads (**Mod 0–2**) under your right thumb while freeing the left side of the display for unobstructed Apple Pencil drawing. The button labels flip their horizontal alignment so your resting thumb never covers the action description.
* **Active Drawing Area Constraints:** On large tablets, reaching across the entire surface repeatedly can cause hand and shoulder strain. In the HUD editor, you can insert spacer panels or narrow the central viewport boundaries to constrain the active 3D drawing canvas into a compact, ergonomically comfortable zone.

---

### 5. Multi-Camera Viewports & Reference Boards

You can split the central display by dragging additional viewport panels from the Widget Gallery onto the canvas, managing visual references and staging simultaneously.

#### Multi-Camera Viewports

* **Independent Navigation & Camera Types:** Each viewport panel functions independently. You can set one viewport to look through an orthographic camera (such as Front, Top, or Side view) for precise profile alignment, while freely tumbling a perspective camera in an adjacent panel.
* **Scene Targeting (Instancing Workflows):** Crucially, viewports can target entirely different scenes simultaneously within the same workspace file. This facilitates side-by-side instancing workflows: you can isolate a master environment in your right viewport while focusing on a solitary prop in your left viewport. Any sculpting or painting edits made to the prop will instantly propagate to its instances in the master environment on the adjacent screen.
* **Ergonomic Bounding:** The modular layout engine allows you to physically adjust the boundaries of these panels. By scaling viewports or adding spacer widgets, you can force your primary drawing viewport into a specific, comfortable physical zone on your tablet screen, minimizing arm strain during long sessions.

#### Reference Boards & Palettes

Managing visual references and global color palettes is centralized inside the **References Inspector**.

* **Image Reference Overlays:** External concept art, orthographic model sheets, or texture references can be imported into dedicated workspace panels. These panels can be docked alongside your active viewports or overlaid directly on top of the 3D canvas with adjustable transparency, allowing you to trace silhouettes and align proportions directly with your sculpting and modeling tools.
* **Fill Colors & PBR Material References:** The References Inspector contains split tabs for managing workspace-wide color palettes and physically based material definitions (including Roughness, Metalness, Subsurface, Sheen, Clearcoat, and Emission values). Assigning these global references via **Reference Fill Layers** ensures that updating a swatch or material in the inspector dynamically updates every linked object across your workspace.

---

## Part III: Geometry Creation, Editing & Retopology (Modeling)

### 1. Base Geometry Types & The Conversion Pipeline

Uniform 3D enforces a strict, directional geometry pipeline. Tools target specific data architectures, meaning mathematical volumes cannot be sculpted with displacement brushes until they are converted into surface point clouds, and topological graph operations cannot run on un-meshed curves.

To master this pipeline, you must first understand the two primary mesh architectures that Uniform utilizes:

* **PolyMesh (Structural Polygon Cages):** The traditional 3D mesh structure. A PolyMesh contains a distinct topological graph consisting of explicit vertices, edges, and polygon faces. It is lightweight, supports UV unwrapping, and is the standard format used for low-poly modeling, retopology, and exporting to game engines.
* **GeoMesh (Dense Point Clouds):** Uniform's high-resolution sculpting architecture. A GeoMesh abandons the traditional polygon graph entirely in favor of a massive, dense continuum of spatial surface points known as **ALBs**. GeoMeshes do not support standard topological operations (like loop cutting or extruding), but they are optimized to handle millions of points for lag-free organic displacement sculpting and vertex painting.

#### Base Generation

* **Procedural Meshes:** Generated via parametric modules (e.g., **Quad Sphere**, **Cylinder**, **Icosahedron**, or **Disk** primitives) within an Evaluator stack. They remain entirely non-destructive as long as the provider module is active in the stack.
* **Volume Groups:** Mathematical Signed Distance Fields (SDF) used for rapid boolean blocking. Shapes meld additively or carve subtractively in real-time without generating explicit or messy polygons.

#### The Explicit Conversion Operations

Geometry transitions are handled via the **Surface Info** panel using explicit conversion commands. Based on the active object's data class, the engine allows the following conversions:

* **Volume Group to GeoMesh:** Bakes the SDF booleans into a dense **GeoMesh** point cloud via the **Apply as GeoMesh** command (found in Object / Apply Volume group), immediately unlocking surface point (ALB) displacement sculpting.
* **GeoMesh to Subdiv GeoMesh:** A standard **GeoMesh** can be converted into a **Subdiv GeoMesh** to unlock multiresolution sculpting. It can also be collapsed back down to a standard GeoMesh. How to: 1. Select the Object in the Scene Outliner. 2. Open Surface Info via the Widget Gallery if not on screen. 3. Tap **Convert to Subdiv GeoMesh** (or the corresponding subdivision rebuild option) to upgrade the data class.
* **GeoMesh to PolyMesh:** While a dense GeoMesh is designed for high-resolution sculpting without a traditional structural graph, it can be converted back into an editable **PolyMesh**. Because topological graphs require significant memory overhead, you must first drastically reduce the mesh's density using a decimation module (such as the **Simplify Layout** evaluator). Once the vertex count is reduced below the engine's safety threshold, the conversion command becomes available in the Surface Info panel. However, generating a completely new, clean low-poly topology over a GeoMesh requires utilizing the Retopology toolset rather than a simple one-click conversion
* **PolyMesh / GeoCurve to GeoMesh:** Traditional polygon cages (**PolyMesh**) and curves (**GeoCurve**) can be converted into a **GeoMesh** to prepare them for high-resolution organic sculpting. How to: 1. Select the target object in the Scene Outliner. 2. Open Surface Info. 3. Tap **Convert to GeoMesh** to instantly translate the low-poly graph or curve data into a dense, surface point cloud.

#### Understanding ALBs (Surface Data Points)

In Uniform 3D's module architecture, an **ALB** is the fundamental unit of surface data that the engine evaluates during a tool stroke. Rather than processing the mesh as a single continuous surface, the engine breaks the active interaction area down into thousands of these individual, addressable points.

Depending on the active layer and the tool's Brush Kernel, an ALB physically represents one of two things:

* **Vertices (Geometry):** When using a Displacement or Deformation tool (such as carving, inflating, or moving), the ALBs are the actual structural vertices of the 3D mesh.
* **Texels (Texture):** When using a Color or Paint tool, the ALBs are the individual texture elements (pixels) on the 2D canvas wrapped around the model.

**How Modules Interrogate ALBs:**
When you draw a stroke, the tool's logic pipeline does not apply a blanket effect. Instead, modules like the *Falloff Provider* or *Surface Influence Provider* execute mathematical queries for every individual ALB within the brush radius. The engine asks questions like, *"What is the physical curvature at this specific ALB?"* or *"Is this ALB inside the active selection mask?"*. The Action Kernel then calculates and applies the final pigment or displacement to each ALB based on those specific returned values.

---

### 2. The Curve Pipeline (PolyCurve & GeoCurve)

Just as polygonal geometry is divided into structural cages and dense point clouds, Uniform 3D splits curve data into two distinct architectural types: **PolyCurve** and **GeoCurve**.

#### PolyCurves (Structural Graph Curves)

PolyCurves represent editable topological networks consisting of distinct vertices and connected curve segments.

* **Editing:** PolyCurves utilize the graph layout system, allowing you to select individual vertices, manipulate curve flow, and structure topological regions.
* **Procedural Integration:** PolyCurves act as the underlying structural framework for generative evaluation operations. In the Evaluator stack, a PolyCurve can be used as the target path for a **Rope Sequence** (generating dynamic physics-based Inverse Kinematic chains) or a **Contours Sequence** (wrapping and arraying geometry along the curve's path).

#### GeoCurves (Dense Point Curves)

GeoCurves act as the curve equivalent to a high-density GeoMesh. They are dense, continuous point evaluations of a curve rather than a low-level editable graph.

* **Rendering & Materials:** GeoCurves carry full physical material data and can be rendered directly by path-traced engines without needing to be explicitly converted into polygonal tubes first.
* **Conversion:** If organic sculpting or surface point manipulation is required, a GeoCurve can be explicitly converted into a standard **GeoMesh** via the **Convert to GeoMesh** command in the Surface Info panel.

---

### 3. Managing Selections & Loop Transformations

While Uniform 3D offers dedicated modules for complex operations, many common modeling tasks are handled through fast, modifier-driven gestures in the viewport rather than explicit UI buttons.

#### Scaling Topological Loops

Because the standard Transform gizmo (the teapot) targets active selection masks rather than raw topological elements, attempting to manually select and scale a single edge loop can be tedious.

To scale a loop quickly, bypass the transform gizmo entirely: use the **Loop** tool and activate the **Inflate** modifier. This forces the targeted loop to expand or contract evenly along its surface normals, perfectly mimicking a uniform scale operation without requiring any prior masking.

#### Gesture-Based Selection

When using tools that evaluate selected vertices (such as the Transform gizmo or Move deformers), the engine recognizes specific stylus gestures drawn in the empty background space of the viewport:

* **Select All:** Drawing a straight line **upwards** in the background selects every vertex on the active object.
* **Lasso Select:** Starting from outside the object, drawing an almost-closed circle around a specific area masks all vertices within that drawn boundary.
* **Invert Selection:** Drawing a straight line **downwards** in the background inverts your current active selection.

> **Troubleshooting Unresponsive Tools:** Because a downward stroke inverts the mask, drawing an upward line (Select All) followed immediately by a downward line (Invert) will leave you with exactly zero vertices selected. If your Transform gizmo or Move tool suddenly appears broken or refuses to manipulate the mesh, you have likely accidentally cleared your selection using this gesture sequence. Ensure you have active vertices masked before attempting to transform.
> 
> 

---

### 4. Implicit Posing: Rigging Without Skeletons

Uniform 3D does not use explicit bone armatures, skeletal hierarchies, or manual vertex weight-painting editors. Instead, posing is handled through **Implicit Pose Rigs**.

When you select a posing tool, the engine computes a dynamic, temporary deformation chain directly from your mesh structure. The pivot point and deformation zone are derived either from your pre-defined Region IDs (Polygroups) or from geodesic surface distance.

```text
Method A: Region Graph Pose Rig (ID Boundaries)
[ Torso (Region 1) ] ========= Joint Pivot Loop ========= [ Arm (Region 2) ]
                                      ▲
                Pivot automatically snaps to Region boundary

Method B: Topology Pose Rig (Geodesic Graph Distance)
[ Pencil Touch Point ] ──────── Geodesic Falloff Distance ────────> [ Boundary ]
                                      ▲
                Dynamic rotational origin calculated along surface

```

#### The Two Implicit Rig Providers

* **Region Graph Pose Rig:**
    * **Mechanism:** Evaluates the integer Region IDs assigned to faces across the object.
    * **Operation:** When you tap a limb or section of your mesh, the engine detects adjacent Region IDs and snaps the joint pivot directly to the boundary loop separating them. For example, if an upper arm is assigned Region 1 and the torso is assigned Region 2, placing your Apple Pencil on the arm sets the rotational origin at the shoulder boundary interface.
* **Topology Pose Rig:**
    * **Mechanism:** Operates on models that have not been divided into discrete Region IDs.
    * **Operation:** Tapping a point on the geometry causes the engine to measure geodesic topological distance outward along connected edges. The joint origin is placed at the specified falloff boundary, allowing organic limbs, tails, or fingers to be posed without prior polygrouping.

#### Pose Transformation Kernels

Once an implicit joint origin and deformation segment are established, you manipulate the limb or feature using dedicated pose transformation modes:

* **Rotate Pose:** Rotates the isolated mesh segment around the implicit joint pivot relative to the current camera viewing angle.
* **Bend Pose:** Applies progressive, non-linear curvature along the length of the topological segment, bending organic structures like fingers, tentacles, or hair clumps.
* **Roll Pose:** Rotates the target segment axially along its own internal central axis, allowing you to twist a forearm, wrist, or leg without altering its primary angle of bend.
* **Translate Pose:** Displaces the unmasked segment along a directional vector, stretching or offsetting joints for stylized cartoon deformations or anatomical adjustments.
* **Scale Pose:** Adjusts the uniform or directional volume of the active segment outward from the joint pivot.

---

### 5. Gesture-Driven Retopology Architecture

Retopology in Uniform 3D bypasses traditional point-by-point vertex extrusions in favor of a gesture-driven sketching workflow derived from the CozyBlanket paradigm. Rather than manually placing individual geometric coordinates in empty 3D space, you sketch topological flow directly over a high-density target object.

```text
[ High-Density Target: GeoMesh / Volume ] <--- Snapping Target (Scene Surface Target)
                   │
                   │ Continuous Raycast & Surface Projection
                   ▼
[ Low-Poly Topology: PolyMesh / EditMesh ] <--- Dynamic Sketching (Cozy Pencil)

```

#### The Underlying Pipeline

* **The Target Surface (Scene Surface Target):** Your dense, sculpted GeoMesh or baked Volume Group serves as an active projection surface. The retopology engine continuously raycasts strokes toward this target so that every point, edge, and quad created immediately conforms to the high-poly silhouette.
* **The Retopology Canvas (PolyMesh / EditMesh):** The active low-poly asset maintains a live Doubly Connected Edge List (DCEL) topological graph. As you draw gestures with the Apple Pencil, the engine recalculates quad structures, boundary loops, and vertex coordinates dynamically.

#### Initializing the Retopology Mesh

Because the CozyBlanket gesture engine requires an existing topological base to evaluate strokes, you cannot start sketching in empty space.

1. **The Add Menu:** Open the **Add** tools menu and navigate to the **Retopo** category.
2. **Base Generation:** Create either a **Contour** or a **Single Quad** directly over your target high-poly surface to establish the initial low-poly graph. To use contour, trace a line from one side to the other in the model.
3. **Transition to Sketching:** With the initial geometry established, switch to the **CozyBlanket** tool (located in the **Graph / Retopo** pipeline category) to begin expanding the surface.

#### Surface Quad Creation & Expansion

* **Quad Strip Generation (CozyBlanket):** Drawing parallel lines along the contour of the high-poly surface followed by perpendicular intersecting strokes instructs the CozyBlanket tool to detect intersections and instantly generate connected quad polygons.
* **PolyBuild:** Located alongside CozyBlanket in the Graph / Retopo menu, this tool evaluates edge loops. Drawing outward strokes from an unshared boundary edge generates new quad rows along the boundary rail.
* **Extend Contour:** Also in the Graph / Retopo menu, this tool allows you to grab an existing open boundary and pull it outward to generate continuous new topological bands by tracing a line parallel to the contour.
* **Boundary Bridge:** Drawing a connection stroke between two separate, open boundary edge loops generates a bridge of quad elements that physically connects the two vertex islands.

#### Interactive Topology Modification

* **Poly Knife (Face Splitting):** Drawing a continuous stroke across existing quad faces activates the face splitting module. The engine raycasts the stroke path onto the target surface, splits the intersected polygon faces, and places new vertices and edge loops along the drawn line.
* **Gesture Deletion:** Quick zigzag or scrubbing gestures drawn over a vertex, edge, or face invoke deletion operations. The engine collapses or dissolves the underlying graph elements and heals the surrounding quad flow.
* **Surface-Constrained Sliding:** Grabbing vertices while holding secondary modifiers activates neighbor-constrained sliding. This moves vertices along the target surface curvature—constrained by active output loops—without lifting them off the high-poly reference volume.

---

## Part IV: Surfacing, UVs & PBR Pipelines (Texturing)

### 1. Topological Seams & Automated UV Unwrapping

Uniform 3D eliminates the need for a separate 2D UV workspace window by embedding seam definition directly into the 3D viewport using the topological graph.

#### Marking & Erasing Seams

* **Active Tool:** Select the **Mark Seam** tool.
* **Manual Drawing:** Drawing your Apple Pencil across the edges of a PolyMesh flags them as UV boundaries, rendering them as solid red lines in the viewport.
* **Erasing Seams:** To remove a UV boundary, draw directly over an existing red seam with the Apple Pencil. The tool acts as a direct toggle.
* **Auto-Loop Selection:** **Double-tap** directly on a highlighted edge to automatically project along the graph rail and select an entire continuous topological edge loop.

#### Global Gestures (Background Swipes)

Uniform includes gesture shortcuts for rapid, global seam management without needing to target specific edges. You perform these strokes in the empty background space of the viewport:

* **Select All Seams:** Draw a single, continuous line **upwards** in the empty background space to automatically flag every hard topological boundary on the mesh as a seam.
* **Clear All Seams:** Draw a single, continuous line **downwards** in the empty background space to instantly erase all red seams across the entire object.

---

### 2. Executing the Unwrap & The UV Layout Scene

Once your boundary seams are defined, the UV generation process transitions from the 3D viewport into a dedicated 2D manipulation space.

1. **Setting Density:** In the **Surface Info** panel, set the desired texel resolution using the **Detail Size** slider, or use the **Detail Size** tool.
2. **The Rebuild:** Tap **Create Texture Set / Rebuild**. The engine's Surface Rebuilder cuts the mesh along your marked seams and computes conformal UV projections.
3. **The UV Layout Scene:** Upon executing the rebuild, the engine generates a 2D scene layout displaying the flattened UV texture map.
4. **Island Manipulation (Apple Pencil Gestures):** To pack and arrange the separated UV islands, Uniform 3D relies on spatial proximity gestures relative to the island's bounding area rather than traditional UI transform gizmos:
    * **Translate (Move):** Tap and drag directly from the **center** of the UV island to move it freely across the 2D canvas.
    * **Rotate:** Tap and drag in the empty space slightly **above** the UV island (without touching the island itself) to rotate the geometry.
    * **Scale:** Tap and drag in the empty space slightly **below** the UV island (without touching the island itself) to scale the geometry up or down to optimize your texture space.

---

### 3. The Layer Stack & Physical Domains

Uniform 3D manages texturing through the **Layers Inspector**. To understand how texturing data is stored and evaluated, it is critical to distinguish between the structural geometry, the allocated canvas, and the physical data channels:

* **The Mesh:** The actual 3D structural data (the topological graph of a PolyMesh or the dense ALB point cloud of a GeoMesh) that dictates the physical shape and volume in the viewport.
* **The Texture Set:** The 2D UV layout and allocated pixel resolution generated during a Surface Rebuild. It acts as the master canvas size and spatial map where all 2D layer data is physically stored for that specific mesh.
* **The Domain:** The specific physical property channels evaluated within that Texture Set. If the Texture Set is the canvas, Domains are the distinct types of data written onto it—such as Color, Displacement, Roughness, or Metalness.

A single layer is not a flat bitmap; it is a container holding multiple independent Domains simultaneously.

#### Layer Types

When adding a new layer, the gallery presents specific layer variants tailored to different surfacing, painting, and procedural tasks:

* **Canvas Layers:** Standard bitmap layers configured for freehand painting and manual sculpting. To optimize performance and prevent cross-contamination, these are divided into specific sub-types:
* **Color Layers:** Dedicated strictly to editing RGB base pigment values (for use with tools like the **Paint Color** brush).
* **Displacement Layers:** Dedicated purely to physical geometric elevation and height manipulation, ensuring your structural sculpting data remains isolated from pigment.
* **Color & Displacement Layers:** Combined canvas layers that enable you to paint color and sculpt physical surface details simultaneously with a single stroke.
* **Procedural Layers:** Non-destructive layers that evaluate algorithmically rather than storing manual brush strokes. These include:
    * **Color Fills:** Floods the active domain with a solid, uniform color value.
    * **Corrections:** Adjusts existing underlying layer data non-destructively (such as modifying HSV values, contrast, or levels across the asset).
    * **PBR Material Applications:** Applies comprehensive, multi-channel physically based materials dynamically across the layer stack.
    * **Image Layers:** Integrates external 2D image textures directly into the stack, allowing them to be evaluated as displacement or color data across the mesh.
    * **Reference Fill Layers:** Generated via the **Reference Layer** module. These assign the global active workspace palette color across the entire object. Because they are linked to the master palette, altering the global color dynamically updates every assigned Reference Fill Layer across your workspace non-destructively.

*(Note: All layer types support user-controllable masking options, allowing you to isolate procedural fills, corrections, and canvas strokes to specific regions or cavities of the model.)*

#### The Physical Domains Architecture

Defined by the engine's PBR Material Definition, physical properties are split into distinct, togglable evaluation channels:

* **Color (Albedo):** Encapsulates the RGB base pigment values.
* **Displacement:** Governs surface elevation and physical surface point height.
* **Roughness:** Regulates microfacet scattering (glossy vs. matte).
* **Metalness:** Governs conductive versus dielectric reflectance.
* **Subsurface (Strength & Color):** Controls internal light scattering for organic materials like skin or wax.
* **Sheen:** Adds a soft backscatter effect for fabrics.
* **Clearcoat (Strength & Roughness):** Adds a secondary, independent specular reflection layer above the base material (e.g., car paint).
* **Emission (Strength & Color):** Governs self-illuminating surface luminance.

Disabling an unneeded domain bypasses texture map allocation for that channel, defaulting to a single global value to heavily save GPU overhead.

---

### 4. Integrating External Images

Uniform 3D provides three distinct workflows for applying external 2D images to 3D surfaces, depending on the required precision and application method.

1. **Direct Texture Import (Layer Generator)**
This method maps an image perfectly to a mesh's UV coordinates, bypassing manual brush strokes entirely.

* **Preparation:** Ensure your target object (such as a spawned Quad or PolyMesh) has an initialized Texture Set. You must execute a **Rebuild** in the Surface Info panel to generate the necessary UVs. *(Note: standard Image Objects do not support this generator)*.
* **Execution:** In the Layers Inspector, add an **Import Color Texture** layer generator. Tap the texture property slot to open the file browser and select your external image. The engine evaluates the image directly across the UV layout. Be aware that alpha transparency support may depend on the layer's blending mode and active domains.

2. **Color Stamp Tool**
This method is ideal for manual, localized placement of decals, logos, or specific texture patches.


* **Preparation:** Ensure your object has a Texture Set and create an active Color or Color & Displacement Canvas Layer.
* **Execution:** Select the **Color Stamp** tool from the Tool Gallery. Assign your external image to the brush in the tool settings, then tap or drag directly on the 3D model to stamp the image onto the active canvas.

3. **Projection Painting Override**
This method projects an image across the surface based strictly on the camera's current perspective.

* **Preparation:** Ensure your object has a Texture Set and create an active Canvas Layer.
* **Execution:** Activate the **Projection painting override** and load your external image. Align your camera viewport to the desired angle and stroke across the mesh. The engine raycasts the image from the camera plane directly into the 2D layer domains, allowing the texture to seamlessly contour over complex physical geometry.

---

### 5. Masking & Domain Protection

Masking in Uniform 3D operates on scalar floating-point fields (**0.0** to **1.0**), stored either within a Geometry Selection Domain or an individual layer's Mask Channel. A value of **1.0** (white) indicates full tool influence, **0.0** (black) represents total protection from operations, and fractional values provide proportional falloff transitions.

#### Interactive Painting & Viewport Clipping

* **Set Selection Brush:** Directly paints selection weights onto the active domain using the Apple Pencil. Stroke intensity and pressure modulate the target weight between **0.0** and **1.0**.
* **Add / Subtract Float Filter:** Dynamically increments or decrements existing values in the float domain, allowing you to build up or carve away masked areas progressively without overwriting previous stroke gradients.
* **Shape & Boundary Clipping:** Uses viewport projection shapes to execute instant, silhouette-based masking across large clusters of vertices or surface points. Tools utilizing this method combine screen-space shapes to define the boundary:
* **Lasso:** Freehand continuous screen-space loops.
* **Rectangle:** Two-point bounding boxes.
* **Circle & Polygon / Star:** Geometric shapes with real-time radius and radial orientation control.

#### Algorithmic Mask Operators & Domain Filters

Float domain filters are algorithmic operators that adjust, smooth, or procedurally transform existing selection masks or layer masks:

* **Invert Selection:** Executes an algebraic (1.0 - X) inversion across the entire domain, instantly swapping masked and unmasked areas.
* **Clamp Selection:** Snaps intermediate grey values against defined low and high thresholds, collapsing soft gradients into hard-edged, aliased selection borders.
* **Grow / Shrink Filter:** Evaluates neighboring topological connections or pixel neighborhoods to dilate or erode the active mask boundary along the surface geometry.
* **Smooth & Sharpen Filters:** Applies Laplacian smoothing across the topology graph to soften harsh mask edges, or sharpens fuzzy boundaries.
* **Float Gradient:** Projects directional linear mathematical gradients across the floating-point domain.
* **Conway Filter:** Executes the Conway Game of Life cellular automata algorithm directly across the float domain to generate procedural, organic patterns.

#### Procedural Influence Providers (Geometry-Aware Masking)

Influence Providers are modules that construct real-time, dynamic masks procedurally by querying the physical and geometric attributes of the underlying surface, bypassing the need to paint masks manually:

* **Curvature Influence:** Samples the mean curvature of the surface points to automatically isolate crevices, cavities, or sharp ridges without manual painting (ideal for cavity dirt or edge wear).
* **Normal Alignment:** Calculates the dot product between the surface normals and a directional reference vector (e.g., restricting paint or displacement exclusively to upward-facing surfaces to simulate snow or dust).
* **Region ID Influence:** Automatically constrains tool execution strictly to geometry sharing the specific topological Region ID located directly beneath the initial Apple Pencil touch point.
* **Selection Influence:** Pipes the active global selection domain directly into a tool's internal influence filter, applying tool actions exclusively where the selection is active.

**Locating Filters and Providers in the UI:**
Because Uniform uses a modular node architecture, these are not standard buttons on the main toolbar. You apply them through three primary locations:

1. **The Tool Editor:** Open the **Main Menu**, select **Tool Editor**, and use the **Swap / Replace Module** button to swap a standard influence for a Curvature, Normal Alignment, or Region ID influence provider within your active brush.
2. **The Tool Gallery:** Look for pre-configured procedural tools (like the **Surface Filter** tool) categorized under the **Surface** or **Region** pipelines in the Gallery. Activating these loads the corresponding float filter kernel automatically.
3. **Global Overrides Inspector:** Plug an Influence Provider directly into an active slot within the **Global Overrides** panel to force every single tool (transforms, extrusions, paint brushes) to respect that mask globally.

#### Domain Safety & Cross-Contamination Overrides

Because Uniform integrates 3D geometry with 2D texture channels, the engine enforces strict domain safeguards to prevent destructive mistakes:

* **Displacement Isolation:** High-frequency displacement sculpting brushes are hard-blocked from firing if the active target is a Color-only canvas layer, ensuring base geometric point coordinates cannot be distorted while painting pigment.
* **Channel Incompatibility:** Texture-space operations cannot write to topological graph structures, and polygon-editing kernels cannot execute on SDF volume groups or float layers, eliminating cross-contamination between structural, volumetric, and surfacing data.

---

## Part V: Procedural Systems & Advanced Tool Customization (Under the Hood)

### 1. Evaluator Stack vs. Global Overrides: The Rosetta Stone

Uniform 3D processes procedural modifications and mathematical overrides in two entirely separate domains. Understanding the difference between them is critical for predictable modeling, especially for users migrating from traditional desktop software.

| Feature | Uniform 3D: Evaluator Stack | Uniform 3D: Global Overrides | Traditional Software Equivalent |
| --- | --- | --- | --- |
| **Where it lives** | Attached to a specific **3D Scene Node**. | Lives globally in the **Workspace HUD**. | **Evaluator:** Modifier Stack. **Overrides:** User Preferences / Active Tool Header. |
| **What it alters** | The **Geometry / Mesh** of the target object. | The **Brush / Apple Pencil Input**. | **Evaluator:** Non-destructive geometry evaluation. **Overrides:** Global tool settings/snapping. |
| **Destructive?** | Non-destructive until baked or applied. | Non-destructive runtime overrides. | Same. |
| **Typical Use** | Arrays, live mirroring, procedural subdivision, SDF voxel meshing. | Grid snapping, stroke smoothing, custom alphas, symmetry planes.
| **Evaluator:** Mirror Modifier, Subdivision Surface, Booleans. **Overrides:** Stroke Stabilizer, Snap to Grid, Brush Texture Falloff.|

---

### 2. Essential Module Configurations Matrix

Because Uniform relies on modular node logic rather than hardcoded monolithic features, advanced behaviors are achieved by plugging specific building blocks into either the **Global Overrides** panel or an object's **Evaluator Inspector**.

Below is the definitive matrix of essential modules translated to their practical applications.

| System | Desired Effect | UI Module Name | Practical Application |
| --- | --- | --- | --- |
| **Global Override** | **Grid Snapping (Hard Surface)** | **Absolute Value Snapping** | Forces all active tools (transforms, primitive placement, extrusions) to lock to rigid incremental spatial units. |
| **Global Override** | **Stroke Stabilizer (Lazy Mouse)** | **Stabilizer Input Filter** | Averages and applies damping to the Apple Pencil input to draw perfectly smooth, sweeping lines without hand jitter. |
| **Global Override** | **Straight Line Lock** | **Adaptive Line Input Filter** | Projects the Pencil input into a straight line based on the initial angle of your stroke. |
| **Global Override** | **Custom Brush Alphas** | **Image Falloff** | Replaces the standard round brush tip with a custom image texture for stamping specific shapes, pores, or mechanical patterns. |
| **Global Override** | **Scatter / Noise Brush** | **Jitter Input Filter** | Randomizes the position and pressure of the Pencil input to create unpredictable, organic scatter effects. |
| **Global Override** | **Input Playback / Macros** | **Input Replay** | Records and overrides the panel input with a previous tool execution, allowing you to replay complex strokes exactly as originally drawn. |
| **Evaluator Stack** | **Live Mirroring** | **Multipass Geometry** | Copies, transforms, and merges geometry across a defined symmetry axis non-destructively. |
| **Evaluator Stack** | **SDF Voxel Meshing** | **Volume Mesher** | Converts an underlying mathematical SDF voxel grid (Volume Group) into an editable polygonal surface mesh. |
| **Evaluator Stack** | **Non-Destructive Decimation** | **Simplify Layout** | Reduces the polygon count of the target object dynamically while preserving the base silhouette and physical volume. |
| **Evaluator Stack** | **Even Topology Spacing** | **Resample Layout** | Procedurally evens out stretched topology into uniform spacing without permanently altering the base mesh. |
| **Evaluator Stack** | **Surface Stamping** | **Stamp Evaluator** | Projects high-frequency mesh details or alphas onto a surface procedurally without permanently baking them into the ALB surface points. |
| **Evaluator Stack** | **Array / Curve Wrap** | **Sequence Deformer + Contours Sequence** | Takes a base mesh and arrays it progressively along a generated transform sequence or a target surface. |
| **Evaluator Stack** | **Live Physics Ropes** | **Rope Sequence** | Deforms an object based on the physics of an Inverse Kinematics (IK) chain, useful for generating hanging cables, chains, or ropes. |
| **Evaluator Stack** | **Stroke-Based Arrays** | **Hook Sequence** | Generates a transform sequence based on your Apple Pencil movement snapped to the screen plane, arraying objects along your manual stroke. |
| **Evaluator Stack** | **Surface-Aligned Arrays** | **Scene Rake Sequence** | Arrays objects or deformations across a surface while conforming to the underlying normal direction, perfect for scales, tank treads, or zippers. |

---

### 3. Advanced Tool Customization: Building Custom Brushes

Uniform 3D exposes its underlying brush architecture directly through the **Tool Editor**. Rather than offering a rigid set of pre-packaged brushes, tools are constructed as vertical pipelines of interchangeable logic blocks called **Modules**. By swapping these modules, a basic digital airbrush can be transformed into a gritty natural-media pencil or a generative smart material mask.

Data flows through a tool in a specific sequence:

1. **Input Filter:** Intercepts the physical Apple Pencil stroke and alters its path.
2. **Evaluator:** Reads hardware dynamics, mapping physical pressure and tilt to brush properties.
3. **Brush Sampler & Falloff Provider:** Translates the 2D stroke into 3D execution points and dictates the shape, hardness, and footprint of the brush tip.
4. **Surface Influence Provider:** Acts as a smart mask, restricting the stroke based on active selections, textures, or underlying surface geometry.
5. **Brush Kernel:** The final payload that executes the color application or geometric deformation on the active layer.

#### Core Module Library

When building or modifying tools, use this reference to configure the pipeline.

##### Input Filters (Stroke Interpolation)

* **Raw Input Filter:** Passes the exact 1:1 panel input without any modifications.
* **Stabilizer Input Filter:** Averages and applies damping to the panel input, pulling the stroke along a weighted line to create smooth curves free from hand jitter.
* **Jitter Input Filter:** Adds randomized mathematical offsets to both the input's position coordinates and its pressure readings. This breaks up the digital uniformity of a stroke to mimic dry chalk, rough charcoal, or a damaged brush tip.
* **Fixed Line / Adaptive Line Input Filter:** Snaps the panel input into a straight line. The adaptive version projects the input position based on the angle detected from your initial touch samples.
* **Cycloid Input Filter:** Generates repeating, looping geometric paths, functioning similarly to a spirograph or kaleidoscopic tracer.

##### Evaluators (Hardware Dynamics)

* **Simple Brush Evaluator:** Modifies the brush sample by adding dynamic size and strength curves based on hardware pressure. This links Apple Pencil pressure directly to stroke radius and opacity.
* **Raw Brush Evaluator:** Sets up a brush sample evaluation with no dynamic modifications to its values, creating a uniform, unvarying stroke regardless of how hard you press.

##### Brush Overlays (Parameter Modulation)

The **Brush Overlay** module acts as a dynamic property modulator. Rather than acting as a standalone pipeline stage, overlays are attached to specific individual properties within a tool (such as radius, influence, hue, or saturation) to drive their values procedurally during a stroke. This system is essential for programming highly dynamic, natural-media pencils.

* **Distance Oscillator:** Modulates the target property cyclically based on the physical distance the stroke travels, allowing for repeating pulse or dashed effects.
* **Invert:** Mathematically reverses the input values driving the property.
* **Jitter:** Injects randomized noise directly into the assigned parameter, instantly creating chaotic, frame-by-frame variations in properties like color hue or saturation.
* **Pressure:** Links the target property explicitly to the hardware pressure data from the Apple Pencil.
* **Stack:** A container module that allows you to layer multiple distinct overlays together, compounding their mathematical effects onto a single property.
* **Tip Orientation:** Drives the parameter based on the physical tilt angle and directional rotation of the stylus.

##### Brush Samplers (Stroke Generation)

* **Scene Cursor Brush Sampler:** The default engine for standard continuous painting and sculpting. Generates continuous brush samples dynamically as the cursor moves across raycast-enabled nodes in the scene.
* **Surface Scatter Brush Sampler:** Generates brush samples randomly scattered throughout the surface instead of along an unbroken line. This is ideal for adding organic noise, airbrush spatter, skin pores, or stippling effects without visible repetition.
* **Mesh Vertex / Edge / Face Brush Sampler:** Snaps the brush evaluation specifically to discrete topological elements. This allows you to sequentially paint or displace individual vertices, edges, or polygon faces cleanly based on the underlying mesh structure rather than freehand strokes.

##### Falloff Providers (Brush Tip Profiling)

* **Constant Falloff Provider:** Returns a uniform maximum falloff value across the entire brush radius. This produces a completely solid, hard-edged brush footprint with no soft fading or gradient at the perimeter, functioning like an inking pen or hard marker.
* **Prism Falloff Provider:** Returns a falloff profile relative to the distance to a rounded prism, generating a blocky, chisel-tip marker profile.
* **Sphere Falloff Provider:** Calculates falloff based on physical Euclidean distance from the center to the edge, creating a standard soft-edged round digital airbrush.
* **Random Falloff Provider:** Returns a completely randomized falloff value for each individual query, breaking the smooth gradient to produce a chaotic, staticky footprint.
* **Texture Falloff Provider:** Returns a falloff profile by sampling a surface-oriented alpha image. The brush tip adopts the shape of the external image loaded into the tool's **Texture Provider** module.

##### Surface Influence Providers (The Masking Engine)

* **Constant Surface Influence Provider:** Returns a uniform influence value everywhere within the brush radius, applying pigment or displacement equally while ignoring masks and geometric angles.
* **Curvature Surface Influence Provider:** Returns the mean curvature of the surface as its influence value. This reads physical sharpness and concavity, allowing you to procedurally paint rust or dirt strictly into deep crevices (cavity masking) or apply scraped-metal effects strictly along exposed ridges (edge wear).
* **Normal Surface Influence Provider:** Evaluates the angle of the surface normal relative to a specified test vector. By setting the test vector, you can restrict the brush to affect geometry facing a specific direction (such as pointing along the Z-axis for accumulating snow or overhead lighting highlights). *(Note: The effect relies on the exact coordinate space and vector values set in the module's properties, which must be calibrated to your object's orientation)*.
* **Region Surface Influence Provider:** Restricts full tool influence strictly to surface points sharing the exact Region ID (Polygroup) located directly under your initial stylus touch. This creates an automatic boundary mask, allowing you to paint rapidly across a complex asset while staying within the borders of a specific mechanical part or clothing seam.
* **Selection Surface Influence Provider:** Returns the active manual selection state as the influence value. Unlike the Constant provider, which applies tool actions across the entire radius, this module evaluates your drawn masks—applying full brush intensity to selected ALBs and completely protecting unselected or masked areas.

##### Brush Kernels (The Execution Payload)

* **Color Kernels:**
    * **Paint Color Brush Kernel:** Blends a new color on top of the current surface color.
    * **Texture Color Brush Kernel:** Sets surface colors by sampling a surface-oriented image assigned in the tool's **Texture Provider** module.
    * **Erase Color Brush Kernel:** Applies alpha erasing directly to surface color channels.
    * **Smooth Color Brush Kernel:** Averages and smooths neighboring surface color values.
* **Displacement Kernels:**
    * **Draw Displacement Brush Kernel:** Displaces geometry along the averaged surface normal direction.
    * **Clay Displacement Brush Kernel:** Displaces geometry toward a dynamically positioned plane, simulating the feel of traditional clay buildup.
    * **Drag Displacement Brush Kernel:** Displaces geometry in the direction of cursor movement.
    * **Pinch Displacement Brush Kernel:** Displaces geometry inward toward the cursor center.
    * **Sharp Displacement Brush Kernel:** Combines pinching and inflating to produce crisp, cut-like creases.
    * **Trim Displacement Brush Kernel:** Flattens geometry by projecting vertex positions onto a dynamically adjusted cutting plane.
    * **Texture Height Displacement Brush Kernel:** Displaces geometry perpendicularly along normals based on the scalar height values of an image assigned in the **Texture Provider** module.
    * **Texture Vector Displacement Brush Kernel:** Displaces geometry in full 3D space (across X, Y, and Z axes) by interpreting the RGB color channels of an image as directional vectors. Unlike standard height displacement, vector displacement can recreate complex overhanging structures (such as scales, horns, or ears) from a single brush stamp. Requires an assigned image in the **Texture Provider** module.
    * **Laplacian Smooth Displacement Brush Kernel:** Applies Laplacian smoothing that averages adjacent vertex positions while actively preserving the overall volume and shape of the model. This prevents geometric shrinkage and mesh collapse common with basic smoothing filters.

#### Building Stylized Brushes

To construct a custom natural-media brush (such as a graphite pencil or rough chalk) that responds to pressure and exhibits organic surface scatter, assemble the following pipeline in the Tool Editor:

1. **Initialize a Custom Tool:** Open the **Tool Gallery**, select a basic brush (such as **Paint Color**), open the **Tool Editor**, and tap **Duplicate** to preserve default presets.
2. **Add Scatter (Input Filter):** Replace the default Input Filter with the **Jitter Input Filter**. Tune the X/Y spread properties to scatter incoming touch coordinates into a textured, dry-brush stroke path.
3. **Map Pressure (Evaluator):** Ensure the Evaluator slot contains the **Simple Brush Evaluator**. Configure the pressure curves to link stylus force directly to brush **Radius** and **Opacity**.
4. **Apply Texture (Falloff Provider):** Replace the standard mathematical sphere with the **Texture Falloff Provider**. In the tool's **Texture Provider** module, tap the image slot to import a greyscale texture of pencil grit, sponge noise, or charcoal grain.
5. **Define Output (Brush Kernel):** For drawing and painting, set the Brush Kernel to **Paint Color Brush Kernel**. If you want this identical gritty, scattered stroke to carve structural grooves into a sculpt rather than apply pigment, swap the kernel to **Draw Displacement Brush Kernel**.

---

## Appendices

### Appendix A: Master Tool & Modifier Reference

This appendix details the primary tools available in the Tool Gallery, outlining their core execution modules, practical applications, and how the dynamic hardware modifiers (Mod 0, Mod 1, Mod 2) alter their behavior in real time. Ensure your active object matches the Required Target data type before attempting to execute a tool.
## Appendix A: Master Tool, Action, and Resource Reference

This appendix details the tools, actions, and resources available within the Uniform 3D workspace. The default layout exposes the primary categories through the interface launchers, while specific advanced modifiers, overrides, and array generators must be accessed via the Tool Gallery or the global Search modal.

| Category | UI Name | Practical Description | Mod 0 (M0 / Shift) | Mod 1 (M1 / Alt) | Mod 2 (M2 / Ctrl) |
| --- | --- | --- | --- | --- | --- |
| Object | Interaction | Shows the default interaction tool for the active object. Required to edit arrays and curves. | N/A | N/A | N/A |
| Object | Reference | Assigns a previously selected visual reference directly into an object. | N/A | N/A | N/A |
| Object | Symmetry | Shows an axis symmetry gizmo so actions executed on one side apply to the other dynamically. | N/A | N/A | N/A |
| Object | Apply Curve | Bakes active curve deformations permanently into the target geometry. | N/A | N/A | N/A |
| Scene | Merge | Combines multiple selected scene nodes into a single node. | N/A | N/A | N/A |
| Scene | Convert | Converts geometry between core architecture types (e.g. Volume to GeoMesh). | N/A | N/A | N/A |
| Scene | Hierarchy | Manages parent and child relationships within the global node tree. | N/A | N/A | N/A |
| Scene | Pivot | Adjusts the spatial pivot origin point of the active scene node. | N/A | N/A | N/A |
| Scene | Transfer | Transfers specific attributes or structural data between selected scene objects. | N/A | N/A | N/A |
| Scene | Replace | Replaces the current object entirely with an object previously saved using the Capture tool. | N/A | N/A | N/A |
| Scene | Duplicate | Creates an instanced duplicate of the selected object linked to the original data. | N/A | N/A | N/A |
| Scene | Symmetrize Node | Mirrors the object transform at the macro scene node level rather than the vertex layout level. | N/A | N/A | N/A |
| Scene | Delete Object | Removes the selected object and its pipeline container from the workspace entirely. | N/A | N/A | N/A |
| Scene | Capture | Writes an Id Storage Object Provider to the overrides panel so the Replace tool can target it. | N/A | N/A | N/A |
| Scene | Origin Object | Resets or defines the localized spatial origin point of the active object geometry. | N/A | N/A | N/A |
| Graph | Deform | Standard topological deformation and layout adjustment tools. | N/A | N/A | N/A |
| Graph | Loop | Selects or manipulates continuous topological edge loops along quad rails. | Inflate Loop | Dissolve Loop | Insert Loop |
| Graph | Extrude | Pulls a face or defined region outward to generate new 3D volume from a flat surface. | Local Inset | Delete Region | Draw Region In-Place |
| Retopo | Poly Build | Extends faces outward organically from open boundary edges. | N/A | N/A | N/A |
| Retopo | CozyBlanket | Dedicated retopology tool for drawing quad grids directly on surfaces using gestures. | N/A | N/A | N/A |
| Retopo | Extend Contour | Pulls an existing open boundary outward to generate continuous new topological bands. | N/A | N/A | N/A |
| Retopo | Bridge Fill | Closes gaps by bridging open topological borders. | N/A | N/A | N/A |
| Retopo | Merge Split | Welds nearby vertices together or splits existing topology at intersections. | N/A | N/A | N/A |
| Edit Mesh | Mark Region | Defines a specific polygonal area with a unique integer ID for localized operations. | N/A | N/A | N/A |
| Edit Mesh | Mark Sharp | Tags specific structural edges to render with hard creased shading. | N/A | N/A | N/A |
| Edit Mesh | Knife | Manually slices through existing geometry to redraw edge flow destructively. | N/A | N/A | N/A |
| Edit Mesh | Mark Seam | Marks boundaries indicated by red lines for automated UV unwrapping calculations. | N/A | Erase Seam | Auto-Loop |
| Edit Mesh | Transform Uv Mapping | Transforms and packs 2D UV coordinates using proximity-based stylus gestures. | N/A | N/A | N/A |
| Edit Mesh | Pattern Uv Mapping | Projects the exact same UV texture scale onto different but equivalent UV islands. | N/A | N/A | N/A |
| Edit Mesh | Boundary | Selects or modifies open mesh borders. Can be used to extend quad lines during retopology. | N/A | N/A | N/A |
| Region | Init Region | Resets or assigns fresh selection groupings to raw geometry. | N/A | N/A | N/A |
| Region | Region Expand | Dilation tool that grows the active region boundary outward across connected faces. | N/A | N/A | N/A |
| Region | Duplicate Region | Clones the specific faces contained within a targeted region ID. | N/A | N/A | N/A |
| Region | Copy Region | Copies the selected region data coordinates to the clipboard buffer. | N/A | N/A | N/A |
| Draw | Draw Object | Draws a new region ID boundary directly onto the active object surface geometry. | N/A | N/A | N/A |
| Draw | Draw Scene | Draws a region boundary projecting through the global scene depth. | N/A | N/A | N/A |
| Draw | Draw Precise | Paints region masks with exact hard-edged stroke boundaries. | N/A | N/A | N/A |
| Draw | Delete Region | Removes targeted faces grouped by region ID. Useful to join shapes with Bridge Fill. | N/A | N/A | N/A |
| Draw | Region Clip | Slices or clips underlying geometry strictly based on a manually drawn region shape. | N/A | N/A | N/A |
| Draw | Split Region | Detaches a specific region into its own independent topological island. | N/A | N/A | N/A |
| Add | Scene Instance | Spawns a lightweight referenced instance of a master scene. | N/A | N/A | N/A |
| Add | Quad | Spawns a single polygon quad directly onto a high-res surface to begin retopology. | N/A | N/A | N/A |
| Add | Contour | Traces a line from one side of a mesh to the other to generate an initial retopology loop. | N/A | N/A | N/A |
| Mesh | Extract | Pulls a new physical mesh surface outward from an existing masked area. | N/A | N/A | N/A |
| Mesh | Grid | Spawns a flat subdivided 3D plane. | Snap Transform | Subtractive | Lock Origin |
| Mesh | Disk | Spawns a flat circular polygon plane primitive. | Snap Transform | Subtractive | Lock Origin |
| Mesh | QuadSphere | Spawns a sphere primitive optimized entirely with even quad topology. | Snap Transform | Subtractive | Lock Origin |
| Mesh | Light Field | Spawns physical scene illumination such as point lights or HDRI environments. | N/A | N/A | N/A |
| Curve | Circle Curve | Spawns a circular curve network primitive. | N/A | N/A | N/A |
| Curve | Radial Curve | Spawns a curve with active radial symmetry properties. | N/A | N/A | N/A |
| Curve | Draw Curve | Freehand sketching tool to generate custom curve paths. | N/A | N/A | N/A |
| Curve | Line Curve | Spawns a rigid straight line curve primitive. | N/A | N/A | N/A |
| Curve | Variant Curve | Spawns a curve with varying thickness or dynamic attributes. | N/A | N/A | N/A |
| Volume Sketch | Rope Volume | Draws a 3D volume stroke that calculates physics rope mechanics dynamically. | N/A | N/A | N/A |
| Volume Sketch | Blob Volume | Draws freehand 2D silhouettes that are instantly converted into solid 3D blocks. | N/A | N/A | N/A |
| Volume Sketch | Draw Volume | Standard freehand 3D volume sketching generation. | N/A | N/A | N/A |
| Surface | Transfer | Transfers specific surface data domains like colors or masks between objects. | N/A | N/A | N/A |
| Surface | Detail Size | Displays a visual grid preview to set the target texel density prior to generation. | N/A | N/A | N/A |
| Surface | Rebuild | Recalculates the geometric manifold and topology to match the set sculpting resolution. | N/A | N/A | N/A |
| Selection | Select Expand | Dilation tool that grows the active selection mask outward. | N/A | N/A | N/A |
| Selection | Region Select | Instantly masks an entire area based purely on its assigned Region ID. | N/A | N/A | N/A |
| Selection | Select Surface | Standard freehand surface masking tool. | Smooth Mask | Erase Mask | Invert Stroke |
| Selection | Select Filter | Applies algorithmic filtering adjustments to the active selection mask. | N/A | N/A | N/A |
| Selection | Color Select | Isolates a mask boundary based on sampled base color pigments. | N/A | N/A | N/A |
| Displacement | Round Clay | Scatters noise or distinct alpha shapes across the surface mesh. | Smooth | N/A | N/A |
| Displacement | Sharp | Carves deep crisp V-shaped scratches or structural panel lines. | Smooth | N/A | N/A |
| Displacement | Draw Displacement | Standard sculpting brush that builds up physical volume layer by layer. | Smooth | Subtractive | Sample Height |
| Displacement | Trim | Scrapes off lumpy geometry by projecting vertices to leave flat hard-surface planes. | Smooth | Fill Divots | N/A |
| Displacement | Square Clay | Builds up flat layered strips of geometry with a distinct square blocky profile. | Smooth | N/A | N/A |
| Displacement | Surface Filter | Procedurally smooths or sharpens the entire surface displacement globally. | Smooth All | Sharpen All | N/A |
| Displacement | Mask Filter | Modifies the geometric displacement strictly based on floating point mask data. | N/A | N/A | N/A |
| Color | Fill Clip | Fills a manually clipped viewport shape with solid pigment. | N/A | N/A | N/A |
| Color | Color Expand | Dilates the painted color outward along the connected surface topology. | N/A | N/A | N/A |
| Color | Gradient | Projects a linear or radial color gradient across the domain. | N/A | N/A | N/A |
| Color | Average | Blurs and mathematically averages surrounding pigment values. | N/A | N/A | N/A |
| Color | Color Stamp | Stamps a pre-made 2D external image onto the active 3D surface layer. | N/A | N/A | N/A |
| Color | Color Filter | Applies global algorithmic color adjustments like hue or saturation shifts to the layer. | N/A | N/A | N/A |
| Color | Paint | The primary brush for coloring the model or revealing procedural layer masks. | Blend Smear | Eraser Mode | Eyedropper |
| Precision | Tex Rebuild | Repacks or rebuilds the 2D texture UV space independently of the geometric rebuild. | N/A | N/A | N/A |
| Precision | Fill Expand | Precision flood fill algorithm for painting specific cavities or enclosed regions. | N/A | N/A | N/A |
| Precision | Pencil | Hard-edged pixel-perfect drawing tool with zero falloff gradient. | N/A | N/A | N/A |
| Precision | Sampler | Eyedropper tool to sample specific surface data parameters. | N/A | N/A | N/A |
| Deformer | Geodesic Move | Moves vertices considering the topological surface distance rather than flat screen space. | N/A | N/A | N/A |
| Extra | Scene Flip | Flips the deformation target physically across the global scene axis. | N/A | N/A | N/A |
| Extra | Elastic | Physics-based elastic grab tool that preserves overall object volume while dragging. | N/A | N/A | N/A |
| Extra | Hook | Pulls a specific clustered grouping of vertices outward following the cursor. | N/A | N/A | N/A |
| Extra | Tweak | Grabs and shifts single vertices or very small local clusters for micro-adjustments. | N/A | N/A | N/A |
| Extra | Boundary | Deforms only the open edge boundaries of a mesh without affecting internal geometry. | N/A | N/A | N/A |
| Extra | Inflate | Puffs out the base geometry mesh radially outward from the normals. | Smooth | Subtractive | N/A |
| Extra | Relax | Smooths and evens out vertex spacing without shrinking the underlying volume. | N/A | N/A | N/A |
| Transform | Scene Transform | Standard 3D gizmo that moves the entire macro object transform globally. | Snap Transform | N/A | Select Object |
| Transform | Object Transform | Standard 3D gizmo that scales or translates geometry vertices in local space. | Snap Transform | Scale Local | Select Object |
| Transform | Move | Physically grabs and translates masked vertices around the screen space. | Smooth | Normal Constrain | Direct Selection |
| Transform | Deform Filter | Applies algorithmic smoothing or sharpening directly to the deformation coordinates. | N/A | N/A | N/A |
| Transform | Pose | Calculates implicit rig origins to bend or rotate mesh segments cleanly from a joint. | Smooth Bend | N/A | N/A |
| Transform | Mesh Kernel | Executes low-level vertex displacement payloads directly on the graph. | N/A | N/A | N/A |
| Curve | Attract Curve | Pulls surrounding geometry locally towards a target curve path. | N/A | N/A | N/A |
| Curve | Radius Tilt | Adjusts the twist and physical thickness of a curve along its trajectory. | N/A | N/A | N/A |
| Curve | Insert Curve | Adds new editable control points to an existing curve line. | N/A | N/A | N/A |
| Curve | Rope Curve | Applies inverse kinematics physics directly to curve control points. | N/A | N/A | N/A |
| Curve | Spiral Curve | Generates helical and corkscrew deformations along a path. | N/A | N/A | N/A |
| Layout | Subdivide | Uniformly increases the active polygon count globally across the mesh layout. | N/A | N/A | N/A |
| Layout | Symmetrize | Projects a mesh across an axis based on object transform. Move object to one side first. | N/A | N/A | N/A |
| Layout | Flip | Reverses the topological layout orientation and normal direction. | N/A | N/A | N/A |
| Layout | Trim Layout | Cuts away geometry and topology physically outside a defined boundary. | N/A | N/A | N/A |
| Search | Search | Opens a search modal window to locate tools, actions, and resources not in launchers. | N/A | N/A | N/A |
| View | Top | Snaps the viewport camera to an orthographic top perspective. | N/A | N/A | N/A |
| View | Left | Snaps the viewport camera to an orthographic left perspective. | N/A | N/A | N/A |
| View | Front | Snaps the viewport camera to an orthographic front perspective. | N/A | N/A | N/A |
| View | Right | Snaps the viewport camera to an orthographic right perspective. | N/A | N/A | N/A |
| View | Back | Snaps the viewport camera to an orthographic back perspective. | N/A | N/A | N/A |
| View | Flip X | Mirrors the viewport camera rendering horizontally. | N/A | N/A | N/A |
| View | Bottom | Snaps the viewport camera to an orthographic bottom perspective. | N/A | N/A | N/A |
| View | Scene | Resets the camera focus to the default global scene perspective. | N/A | N/A | N/A |
| View | Note | Opens workspace text notes or structural annotations. | N/A | N/A | N/A |
| N/A | Chart Surface View | Specialized rendering display mode for analyzing surface topology. | N/A | N/A | N/A |
| Not in Launchers | Viewport Projector Editor | Manages 2D image projection overlays floating in the viewport. | N/A | N/A | N/A |
| Not in Launchers | Projection Triangle | Uses a triangle primitive shape for viewport shape masking or projection operations. | N/A | N/A | N/A |
| Not in Launchers | Selection Clip | Hides geometry completely outside of the active selection mask visually. | N/A | N/A | N/A |
| Not in Launchers | Scene Selection Draw Clip | Draws a clip shape using scene depth projection to isolate selections. | N/A | N/A | N/A |
| Not in Launchers | Deformation Selection Draw Clip | Masks deformation influence strictly using a manually drawn shape. | N/A | N/A | N/A |
| Not in Launchers | Light Tweak Surface View | Adjusts how lighting interacts with the surface preview without altering light nodes. | N/A | N/A | N/A |
| Not in Launchers | Snap Scene Transform Delta Deformer | Snaps deformation offsets strictly to incremental scene coordinate units. | N/A | N/A | N/A |
| Not in Launchers | Region Extract Build Mesh | Converts a specific region ID boundary into a completely separate physical mesh layout. | N/A | N/A | N/A |
| Not in Launchers | Ambient Occlusion Surface Process | Bakes or calculates cavity shadowing globally across the surface domains. | N/A | N/A | N/A |
| Not in Launchers | Anchor Deformer | Pins specific target vertices in place while surrounding unmasked geometry deforms. | N/A | N/A | N/A |
| Not in Launchers | Tweak EditMesh | Micro-adjusts individual EditMesh vertices on the fly without leaving the current tool. | N/A | N/A | N/A |
| Not in Launchers | Distance Measure | Ruler utility to measure specific spatial scale units between two points. | N/A | N/A | N/A |
| Not in Launchers | Retopology Override | Forces the active tool to evaluate exclusively against retopology snapping targets. | N/A | N/A | N/A |
| Not in Launchers | Snapping Override | Global toggle module for overriding spatial layout snapping. | N/A | N/A | N/A |
| Not in Launchers | Pixel Snap Override | Forces 2D canvas operations to snap perfectly to integer pixel coordinates. | N/A | N/A | N/A |
| Not in Launchers | Grid Snap Override | Forces operations and transforms to snap perfectly to the workspace background grid. | N/A | N/A | N/A |
| Not in Launchers | Paint Color Override | Forces multi-purpose tools to execute strictly in the Color domain. | N/A | N/A | N/A |
| Not in Launchers | Paint Material Override | Destructive PBR painting mode that writes to multiple material channels simultaneously. | N/A | N/A | N/A |
| Not in Launchers | Smooth Displacement Override | Forces the active brush footprint to act as a smoothing algorithm globally. | N/A | N/A | N/A |
| Not in Launchers | Projection Painting Override | Projects 2D images seamlessly from the camera perspective onto the 3D surface. | N/A | N/A | N/A |
| Not in Launchers | Brush Backface Culling Override | Prevents brushes from affecting geometry facing away from the camera angle. | N/A | N/A | N/A |
| Not in Launchers | Simulation Override | Forces standard layout tools to evaluate dynamically with physics properties. | N/A | N/A | N/A |
| Not in Launchers | Rope Stabilizer Override | Adds heavy inverse kinematics damping to stroke inputs for perfectly smooth lines. | N/A | N/A | N/A |
| Not in Launchers | Cycloid Stroke Override | Generates looping spirograph geometric paths from standard freehand strokes. | N/A | N/A | N/A |
| Not in Launchers | Straight Line Override | Snaps panel input into rigid straight lines based on the initial drawing angle detected. | N/A | N/A | N/A |
| Not in Launchers | Circle Stroke Override | Forces the input stroke path into a perfect geometric circle. | N/A | N/A | N/A |
| Not in Launchers | Star Stroke Override | Forces the input stroke path into a geometric star shape. | N/A | N/A | N/A |
| Not in Launchers | Scatter Brush Override | Randomizes brush sample coordinates for stippling or organic noise generation. | N/A | N/A | N/A |
| Not in Launchers | Curvature Surface Influence Override | Constrains brush effects strictly to sharp crevices or exposed physical ridges. | N/A | N/A | N/A |
| Not in Launchers | Braid Curve Override | Wraps geometric curve generation into an intertwining braid pattern. | N/A | N/A | N/A |
| Not in Launchers | Clone Curve Override | Duplicates curve data evenly along a defined spatial offset. | N/A | N/A | N/A |
| Not in Launchers | Helix Curve Override | Forces continuous curve generation into a corkscrew shape. | N/A | N/A | N/A |
| Not in Launchers | Radius Spike Override | Injects randomized thickness variations along a curve trajectory. | N/A | N/A | N/A |
| Not in Launchers | Twist Curve Override | Applies axial rotation twisting along a curve length continuously. | N/A | N/A | N/A |
| Not in Launchers | Target Align Curve Override | Snaps curve normals to face a specific global target object. | N/A | N/A | N/A |
| Not in Launchers | Square Profile Override | Replaces standard round brush falloffs with a hard square tip. | N/A | N/A | N/A |
| Not in Launchers | Circle Profile Override | Standard round digital airbrush falloff override. | N/A | N/A | N/A |
| Not in Launchers | Line Profile Override | Forces the brush tip into a sharp linear chisel for edge carving. | N/A | N/A | N/A |
| Not in Launchers | Rectangle Profile Override | Forces the brush tip into an elongated horizontal or vertical rectangle. | N/A | N/A | N/A |
| Not in Launchers | Random Spike Oval Profile Override | Generates chaotic staticky brush footprints for heavy damage effects. | N/A | N/A | N/A |
| Not in Launchers | Grid Array | Essential procedural array that duplicates objects in an X/Y grid structure. | N/A | N/A | N/A |
| Not in Launchers | Radial Array | Essential procedural array duplicating objects in a circular radial pattern. | N/A | N/A | N/A |
| Not in Launchers | Curve Array | Essential procedural array wrapping objects continuously along a curve. | N/A | N/A | N/A |
| Not in Launchers | Subdivide Poly Mesh Overlay | Applies a non-destructive subdivision surface modifier visually to PolyMeshes. | N/A | N/A | N/A |
| Not in Launchers | Shell Poly Mesh Overlay | Applies non-destructive physical thickness geometry to flat PolyMeshes. | N/A | N/A | N/A |
| Not in Launchers | Geo Curve Volume | Generates solid physical volume continuously around a GeoCurve. | N/A | N/A | N/A |
| Not in Launchers | Geo Curve Custom | Loads custom parameterization overrides for GeoCurves. | N/A | N/A | N/A |
