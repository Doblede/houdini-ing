# houdini-ing
Houdini tips and snippets in Python and VEX


# COPY FILE PATH TO CLIPBOARD - PYTHON
```
import hou
hip_path = hou.hipFile.path()
hou.ui.copyTextToClipboard(hip_path)
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
