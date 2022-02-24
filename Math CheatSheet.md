```julia
deg_to_rad(deg) = deg * Math.PI / 180
rad_to_deg(rad) = rad * 180 / Math.PI
radians(deg) = deg_to_rad(deg)
degrees(rad) = rad_to_deg(rad)


```
[![unit circle](https://camo.githubusercontent.com/56e6f1e389c443363ea9a302a915dbd8e23953190bc1b8ea54e53410595203c2/68747470733a2f2f6f63772e6d69742e6564752f616e73373837302f31382f31382e303133612f74657874626f6f6b2f48544d4c2f6368617074657230322f696d616765732f747269676f5f66756e6374696f6e732e676966)](https://camo.githubusercontent.com/56e6f1e389c443363ea9a302a915dbd8e23953190bc1b8ea54e53410595203c2/68747470733a2f2f6f63772e6d69742e6564752f616e73373837302f31382f31382e303133612f74657874626f6f6b2f48544d4c2f6368617074657230322f696d616765732f747269676f5f66756e6374696f6e732e676966)

```julia
sin(ø) = opposite / hypotenuse = y / r = 对边 / 斜边
cos(ø) = adjacent / hypotenuse = x / r = 邻边 / 斜边
tan(ø) = opposite / adjacent = x / y = 对边/邻边



rotate2d(p, ø) = mat2(cos(ø), sin(ø)
					-sin(ø), cos(ø)) * p

sq(x) = x*x
distance(p1, p2) = p1 - p2
distance2Sq(v1, v2) = pow(distance(v1.x, v2.x)),2) + pow(distance(v2.y,v1.y),2)
distance(v1, v2) = sqrt(distaceSq(v1, v2))
v = (x,y,z)
length(v) = sqrt(x*x + y*y + z*z)
length(v) = sqrt(x*x + y*y)

v1+v2 = (x1+x2,y1+y2,z1+z2)
normalize(v) = (x/length(v), y/length(v))
dot(v1,v2) = x1*x2 + y1*y2 + z1*z2
angle_of(x,y) = degrees(atan(x,y))
angle_of(v1, v2) = degrees(acos(dot(v1, v2) / (length(v1) * length(v2)))
cross(v1, v2) = (y1*z2 - z1*y2, z1*x2-x1*z2, x1*y2-y1*x2)
projection(v, ø) = normalize(v) * cos(ø) # 
```