# Bounding Volumes & Frustum Culling
# Demo Video
![FrustumCulling-ezgif com-crop](https://github.com/user-attachments/assets/8f1d417b-118c-4c7d-ac7c-0d2973af9ed1)


# What’s Implemented

# 1) AABB generation (load time, once per mesh)

- Where: `MeshManager::getAsset()` in `MeshManager.cpp`, after the mesh is loaded.

- What:

  - Traverse the position buffer to find the minimum and maximum x, y, and z values, forming a local-space AABB.

  - Store:

    - `Vector3 m_AABBPoints[8]`: the eight corners used for visibility testing.

  - Enable culling for the mesh with `m_performBoundingVolumeCulling = true`.

# 2) Frustum plane construction (every camera update)

- Where: `CameraSceneNode::do_CALCULATE_TRANSFORMATIONS()`.

- What:

  - Build the camera’s view and projection matrices.

  - Extract six frustum planes—left, right, top, bottom, near, and far—into `m_frustumPlanes[6]`.

  - Use a slightly narrower FOV (`verticalFov2`) for the culling frustum in the demo. This makes objects disappear near the screen edge so the culling transition is visible.

# 3) Runtime culling & debug rendering

- Where: `SingleHandler_DRAW::do_GATHER_DRAWCALLS()` in `SH_DRAW.cpp`.

- What:

  - For each `MeshInstance`, obtain its parent `SceneNode` world transform.

  - Transform the eight AABB corners into world space and test them against the six frustum planes.

  - Set `m_culledOut`, update `m_numVisibleInstances`, and skip culled instances in the drawing path.

