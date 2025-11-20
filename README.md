# houdini-ing
Houdini tips and snippets in Python and VEX


# COPY FILE PATH TO CLIPBOARD - PYTHON
```
import hou
hip_path = hou.hipFile.path()
hou.ui.copyTextToClipboard(hip_path)
```

# CHANGE SELECTED NODES/NETBOXES COLOR - PYTHON
```
import hou
selected_color = hou.ui.selectColor()
if selected_color:
    # Get the current selected items
    selected_items_list = hou.selectedItems()
    # Change color of each selected element
    for selected_item in selected_items_list:
        selected_item.setColor(selected_color)
```

# JUMP UP - PYTHON
Script to override the `u` shortcut, to jump a level up on the Network View
It changes the Update Mode to Manual before Jumping Up
```
import hou
hou.setUpdateMode(hou.updateMode.Manual)
network = hou.ui.curDesktop().paneTabUnderCursor()
networkPath = network.pwd().path()
parent_node = hou.node(networkPath)
network.setCurrentNode(parent_node)
```

# UPDATE MODE SWITCH SCRIPT - PYTHON
Script to be used on a shortcut to toggle the Update Mode between Manual/AutoUpdate
```
import hou
mode = hou.updateModeSetting().name()
if mode == 'AutoUpdate':
    hou.setUpdateMode(hou.updateMode.Manual)
if mode == 'Manual':
    hou.setUpdateMode(hou.updateMode.AutoUpdate)
```


# ATTACH GUIDES TO CLOSEST SURFACE - VEX - Run Over Primitives
```
int points[] = primpoints(0, @primnum);
vector rootpos = point(0, "P", points[0]);
vector nearpos = minpos(1, rootpos);
vector dist = nearpos-rootpos;
foreach(int point; points){
    vector pointpos = point(0, "P", point);
    setpointattrib(0, 'P', point, pointpos+dist);
}
```

# CURVEU -  VEX - Run Over Points
```
float u = vertexcurveparam(0, @vtxnum);
@curveu = primuvconvert(0, u, @primnum, PRIMUV_UNIT_TO_UNITLEN);
```


# COMPUTE CENTER OF MASS / CENTROID
Several methods to calculate the center of mass.

Just with VEX - Run Over Detail
```
//COMPUTE CENTER OF MASS
#include "pbd_granular.h"
int total = npoints(0);
int points[] = array();
for (int i = 0; i < total; i++) {
    append(points, i);
}
v@centerOfMass = computeCenterOfMass(0, points);
```
