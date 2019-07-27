## glsl-intersection-functions

i wish there was just a library of rly fast intersection functions for glsl  - me, 2019

### Ray construction

#### FOV to ray dir

```
const float PI = 3.14159265359;
const float DEG_TO_RAD = PI / 180.0;

vec3 ray_dir(float fov, vec2 size, vec2 pos) {
  vec2 xy = (pos - size * 0.5);

  float cot_half_fov = tan((90.0 - fov * 0.5) * DEG_TO_RAD);
  float z = size.y * 0.5 * cot_half_fov;

  return normalize(vec3(xy, -z));
}
```

### Sphere intersection

#### In and out

```
bool intersectSphere(vec3 origin, vec3 direction, vec3 sphereCentre, float sphereRadius, out vec2 intersectionLength)
{
  float c = length(origin - sphereCentre) - sphereRadius*sphereRadius;
  float dotVal = dot(direction, (origin - sphereCentre));
  float sqrtVal = dotVal*dotVal - dot(origin - sphereCentre, origin - sphereCentre) + sphereRadius*sphereRadius;
  if (sqrtVal <= 0.0) {
    return false;
  }
  if (sqrtVal == 0.0) {
    intersectionLength.x = -dotVal;
    intersectionLength.y = -dotVal;
    return true;
  }
  else {
    float d1 = -(dotVal) + sqrt(sqrtVal);
    float d2 = -(dotVal) - sqrt(sqrtVal);
    if (d1 < 0.0 && d2 < 0.0) {
      return false;
    } else if (d1 < 0.0 || d2 < 0.0) {
      intersectionLength.x = max(d1,d2);
      intersectionLength.y = min(d1,d2);
      return true;
    } else {
      intersectionLength.x = max(d1,d2);
      intersectionLength.y = min(d1,d2);
      return true;
    }
  }
}
```
