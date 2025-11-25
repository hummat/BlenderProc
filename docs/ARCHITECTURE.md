# BlenderProc Architecture Overview

This document gives a high-level map of BlenderProc’s internals plus pointers to key modules. It is intentionally short; follow the links into the codebase for details.

## Execution Modes & Entry Points

- CLI entry is defined in `setup.py` as `blenderproc=blenderproc.command_line:cli`.
- Main script: `blenderproc/command_line.py` (modes: `run`, `debug`, `quickstart`, `vis`, `download`, `extract`, `pip`).
- Module entry: `blenderproc/__main__.py` → `command_line.cli()`.
- External `bpy` mode is controlled via `USE_EXTERNAL_BPY_MODULE=1` (see `docs/tutorials/bpy_module.md` and `blenderproc/__init__.py`).
- Internal Blender installs and env setup live in `blenderproc/python/utility/InstallUtility.py` and `SetupUtility.py`.

## Layering & Public API

- Public API surface:
  - `blenderproc/api/*/__init__.py` – re-exports.
  - `blenderproc/__init__.py` – exposes `bproc.loader`, `bproc.camera`, `bproc.renderer`, `bproc.writer`, `bproc.types`, etc.
- Implementations:
  - `blenderproc/python/*` – real logic (camera, loader, renderer, writer, object, material, sampler, utility, tests).
  - `blenderproc/external/*` – bundled third‑party helpers (e.g. VHACD in `external/vhacd`).

## Core Subsystems (Where to Look)

- **Types & Utilities**  
  - Generic wrappers: `blenderproc/python/types/*` (see `MeshObjectUtility.py`, `LightUtility.py`, `MaterialUtility.py`, `EntityUtility.py`).  
  - Global helpers: `blenderproc/python/utility/Utility.py`, `GlobalStorage.py`, `DefaultConfig.py`, `Initializer.py`.
- **Camera**  
  - API: `blenderproc/api/camera/__init__.py`.  
  - Logic: `blenderproc/python/camera/CameraUtility.py`, `CameraProjection.py`, `CameraValidation.py`, `LensDistortionUtility.py`.
- **Rendering & Postprocessing**  
  - API: `blenderproc/api/renderer/__init__.py`, `blenderproc/api/postprocessing/__init__.py`.  
  - Logic: `blenderproc/python/renderer/RendererUtility.py`, `SegMapRendererUtility.py`, `FlowRendererUtility.py`, `NOCSRendererUtility.py`, plus `python/postprocessing/PostProcessingUtility.py` and `StereoGlobalMatching.py`.
- **Loading & Datasets**  
  - API: `blenderproc/api/loader/__init__.py`.  
  - File/dataset loaders: `blenderproc/python/loader/*.py` (OBJ, BOP, 3D‑FRONT, SUNCG, Replica, ShapeNet, Pix3D, AMASS, Matterport3D, URDF, CC textures, Haven).  
  - Dataset resources: `blenderproc/resources/*`.
- **Objects, Physics, Sampling**  
  - API: `blenderproc/api/object/__init__.py`, `blenderproc/api/sampler/__init__.py`, `blenderproc/api/filter/__init__.py`.  
  - Logic: `blenderproc/python/object/*.py`, `python/sampler/*.py`, `python/filter/Filter.py`, `python/utility/CollisionUtility.py`, `external/vhacd/decompose.py`.
- **Materials & Lighting**  
  - API: `blenderproc/api/material/__init__.py`, `blenderproc/api/lighting/__init__.py`, `blenderproc/api/world/__init__.py`.  
  - Logic: `blenderproc/python/material/*.py`, `python/lighting/*.py`, `python/loader/HavenMaterialLoader.py`.
- **Writers & Visualization**  
  - API: `blenderproc/api/writer/__init__.py`.  
  - Logic: `blenderproc/python/writer/*.py`.  
  - CLI visualization tools: `blenderproc/scripts/visHdf5Files.py`, `vis_coco_annotation.py`, `saveAsImg.py`.

## Examples, Docs & Tests

- Examples by topic: `examples/basics/*`, `examples/advanced/*`, `examples/datasets/*`.  
- Tutorials: `docs/tutorials/*.md` (loader, camera, renderer, writer, key_frames, physics, bpy_module).  
- High-level tests: `tests/*.py` (run via `cli.py run tests/run_all.py`).  
- Test helpers and paths: `blenderproc/python/tests/*.py`.

