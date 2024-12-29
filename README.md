# unreal-moss-atlas-tool

A Houdini tool to create moss foliage from atlas texture for Unreal Engine.

## Features

- Automatically finds and sets texture maps for Opacity, Roughness, and BaseColor.
- Configures the FBX export node with the appropriate file path.

## Requirements

- Houdini
- Unreal Engine

## Usage

1. Ensure your texture files are named with "Opacity", "Roughness", and "BaseColor" in their filenames.
2. Run the script to automatically set the texture parameters and configure the FBX export node.

### Unreal Moss Render
![Unreal Moss Render](moss.png)

### Houdini Tool Screenshot
![Houdini Tool Screenshot](screenshot.png)

## Script Details

```python
import os
import sys

default_path = kwargs['node'].parm('folder').evalAsString()
file_name = os.path.basename(os.path.normpath(default_path)) + ".fbx"

# Find files containing "Opacity", "Roughness", "BaseColor"
files = os.listdir(default_path)
filtered_files = {
    "Opacity": None,
    "Roughness": None,
    "BaseColor": None
}

for file in files:
    if "Opacity" in file:
        filtered_files["Opacity"] = os.path.join(default_path, file)
    elif "Roughness" in file:
        filtered_files["Roughness"] = os.path.join(default_path, file)
    elif "BaseColor" in file:
        filtered_files["BaseColor"] = os.path.join(default_path, file)

# Set the parameters if the files are found
if filtered_files["Opacity"]:
    hou.pwd().parm('atlas_opacity').set(filtered_files["Opacity"])
if filtered_files["Roughness"]:
    hou.pwd().parm('atlas_roughness').set(filtered_files["Roughness"])
if filtered_files["BaseColor"]:
    hou.pwd().parm('atlas_base_color').set(filtered_files["BaseColor"])

rop_fbx_node = hou.node('../fbx_export')
rop_fbx_node.parm('sopoutput').set(default_path + file_name)

# print(filtered_files)
```
