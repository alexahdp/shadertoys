# Примеры программ на glsl

## Все примеры кода выполнены для сайта https://shadertoy.com



### Преобразовать координаты текущего пикселя
```c
vec2 uv = fragCoord/iResolution.xy;
uv.x -= 0.5;
uv.x *= iResolution.x / iResolution.y;
```

### Нарисовать сферу
```c
vec2 uv = fragCoord.xy/iResolution.xy;

float aspectRatio = iResolution.x / iResolution.y;
uv.x *= aspectRatio;
uv.x += (0.5 - 0.5 * aspectRatio);

vec2 center = vec2(0.5, 0.5);
float R = 0.4;

vec3 col;
if (distance(uv, center) < R) {
  col = vec3(1.0, 0.0, 0.0);
}

fragColor = vec4(col, 1.0);
```

### Нарисовать сферу (вариант 2)
```c
vec2 uv = fragCoord.xy / iResolution.xy;
uv -= 0.5;
uv.x *= iResolution.x / iResolution.y;

float d = length(uv);

if (d < 0.3) c = 1.0; else c = .0;
float c = smoothstep(r, r - 0.01, d);

fragColor = vec4(vec3(c), 1.0);
```

### Нарисовать сферу с размытой границей
```c
vec2 uv = fragCoord.xy/iResolution.xy;

float aspectRatio = iResolution.x / iResolution.y;
uv.x *= aspectRatio;
uv.x += (0.5 - 0.5 * aspectRatio);

vec2 center = vec2(0.5, 0.5);
float R = 0.4;

vec3 col;
if (distance(uv, center) < R) {
  col = vec3(smoothstep(0.4, 0.1, distance(uv, center)), 0.0, 0.0);
}

fragColor = vec4(col, 1.0);
```

### Нарисовать две точки с размытыми границами
```c
vec2 uv = fragCoord.xy/iResolution.xy;

float aspectRatio = iResolution.x / iResolution.y;
uv.x *= aspectRatio;
uv.x += (0.5 - 0.5 * aspectRatio);

vec2 center = vec2(0.5, 0.5);
vec2 dot1 = vec2(0.2, 0.2);
vec2 dot2 = vec2(0.8, 0.8);
float R = 0.4;

vec3 col;
if (distance(uv, dot1) < 0.2) {
  col = vec3(smoothstep(0.2, 0.1, distance(uv, dot1)), 0.0, 0.0);
}
else if (distance(uv, dot2) < 0.2) {
  col = vec3(smoothstep(0.2, 0.1, distance(uv, dot2)), 0.0, 0.0);
}

fragColor = vec4(col, 1.0);
```

### Перетаскиваемая сфера (таскать с зажатой левой клавишей)
```c
vec2 uv = fragCoord.xy/iResolution.xy;

float aspectRatio = iResolution.x / iResolution.y;
uv.x *= aspectRatio;
uv.x += (0.5 - 0.5 * aspectRatio);

vec3 mouse = vec3(iMouse.xy / iResolution.xy, 0);
vec2 mousePointer = vec2(mouse.xy);

vec3 col;
if (distance(uv, mousePointer) < 0.2) {
  col = vec3(smoothstep(0.2, 0.1, distance(uv, mousePointer)), 0.0, 0.0);
}

fragColor = vec4(col, 1.0);
```

### Нарисовать кольцо
```c
vec2 uv = fragCoord.xy / iResolution.xy;

float aspectRatio = iResolution.x / iResolution.y;
uv.x *= aspectRatio;
uv.x += (0.5 - 0.5 * aspectRatio);

vec2 center = vec2(0.5, 0.5);
float R = 0.4;

vec3 col;
float D = distance(uv, center);
if (D < R && D > R - 0.01) {
  col = vec3(1.0, 0.0, 0.0);
}

fragColor = vec4(col, 1.0);
```

### Делаем расходящиеся кольца
```c
float aspectRatio = iResolution.x / iResolution.y;

vec2 uv = fragCoord.xy / iResolution.xy;
uv.x *= aspectRatio;
uv.x += (0.5 - 0.5 * aspectRatio);

vec2 center = vec2(0.5, 0.5);

// скорость движения кругов
float speed = 0.035;

// ширина кругов
float width = 0.013;

float x = (center.x - uv.x);
float y = (center.y - uv.y);
float r = -sqrt(x*x + y*y);

float z = 1.0 + 0.5 * sin((r + iTime * speed) / width);

// текстура кругов в черно-белом виде
vec3 texcol = vec3(z);

// цвета
vec3 col = vec3(uv, 0.5 + 0.5 * sin(iTime));

fragColor = vec4(col * texcol, 1.0);
```


### Добавляем текстуры
```c
float aspectRatio = iResolution.x / iResolution.y;

vec2 uv = fragCoord.xy / iResolution.xy;
uv.x *= aspectRatio;
uv.x += (0.5 - 0.5 * aspectRatio);

// получаем цвета пикселя из текстуры
vec4 tex = texture(iChannel0, uv.xy);

vec2 center = vec2(0.5, 0.5);

// скорость движения кругов
float speed = 0.035;

// ширина кругов
float width = 0.013;

float x = (center.x - uv.x);
float y = (center.y - uv.y);

float r = -sqrt(x*x + y*y);
float z = 1.0 + 0.5 * sin((r + iTime * speed) / width);

// текстура кругов в черно-белом виде
vec3 texcol = vec3(z);

// цвета
vec3 col = vec3(uv, 0.5 + 0.5 * sin(iTime));

fragColor = vec4(tex.xyz * col * texcol, 1.0);
```

### Смещать текстуру бесконечно в сторону (карусель)
```c
float speed = 100.0;
vec2 prev = fragCoord - vec2(iTime * speed, 0);
vec2 uv = prev / iResolution.xy;

vec4 tex = texture(iChannel0, uv);
fragColor = vec4(tex.rgb, 1.0);
```

### mix - смешивание цветов
```c
vec2 uv = fragCoord / iResolution.xy;
vec3 col1 = vec3(0.5, 0., 0.);
vec3 col2 = vec3(0., 0.5, 0.);

// a - процентное количество цвета
// а = 0.5 - взять цветов поровну
// a = .0 - взять только первый цвет
// a = 1.0 - взять только второй цвет
// mix(color1, color2, a);

fragColor = vec4(
    mix(col1, col2, uv.x).r,
    mix(col1, col2, uv.y).g,
    mix(col1, col2, abs(sin(iTime))).r,
    1.0
);
```

### rgb2hsb
```c
vec3 rgb2hsb( in vec3 c ){
  vec4 K = vec4(0.0, -1.0 / 3.0, 2.0 / 3.0, -1.0);

  vec4 p = mix(vec4(c.bg, K.wz),
  vec4(c.gb, K.xy),
  step(c.b, c.g));

  vec4 q = mix(vec4(p.xyw, c.r),
  vec4(c.r, p.yzx),
  step(p.x, c.r));

  float d = q.x - min(q.w, q.y);
  float e = 1.0e-10;

  return vec3(abs(q.z + (q.w - q.y) / (6.0 * d + e)),
    d / (q.x + e),
    q.x
  );
}
```

### hsb2rgb
```c
vec3 hsb2rgb( in vec3 c ){
  vec3 rgb = clamp(
    abs(mod(c.x*6.0 + vec3(0.0,4.0,2.0),6.0)-3.0) - 1.0,
    0.0,
    1.0
  );

  rgb = rgb * rgb * (3.0 - 2.0 * rgb);
  return c.z * mix(vec3(1.0), rgb, c.y);
}
```

### переход к полярной системе координат
```c
// x = r * cos(phi);
// y = r * sin(phi);

vec2 uv = fragCoord / iResolution.xy;

// вектор от центра к текущему пиксулю
vec2 toCenter = vec2(0.5) - uv;

// угол вектора
float ca = atan(toCenter.x, toCenter.y);

// расстояние от центра до текущего пикселя
float r = length(toCenter) * 2.0;

// abs нужен для цветов, иначе будет черный
vec3 color = vec3(abs(r * cos(ca)), abs(r * sin(ca)), 0.);

// в формуле выше видно, что blue-канал никак не меняется
// и не зависит от координат, вписать его в текущий круг довольно сложно.
// именно для этого и нужна формула hsb2rgb
// vec3 color = hsb2rgb(vec3((ca / TWO_PI) + 0.5, r, 1.0));
```

### психоделическая карусель
```c
#define TWO_PI 6.28318530718

void mainImage( out vec4 fragColor, in vec2 fragCoord ) {
  vec2 uv = fragCoord / iResolution.xy;

  // вектор от центра к текущему пиксулю
  vec2 toCenter = vec2(0.5) - uv;

  // угол вектора
  float ca = atan(toCenter.x, toCenter.y);

  // расстояние от центра до текущего пикселя
  float r = length(toCenter) * 2.0;

  // психоделическая карусель
  // К формуле из предыдущего примера просто добавляем динамику
  vec3 color = hsb2rgb(vec3(((ca + iTime) / TWO_PI) + 0.5, r, 1.0));

  fragColor = vec4(color, 1.0);
}
```

### Белый квадрат Малевича
```c
vec2 uv = fragCoord.xy/iResolution.xy;

float aspectRatio = iResolution.x / iResolution.y;
uv.x *= aspectRatio;
uv.x += (0.5 - 0.5 * aspectRatio);

float x = step(0.1, uv.x);
float y = step(0.1, uv.y);

vec3 color = vec3(x * y);

x = step(0.1, 1.0 - uv.x);
y = step(0.1, 1.0 - uv.y);

vec3 color2 = vec3(x * y * color);

fragColor = vec4(color2, 1.0);
```

### Белые круги
```c
float circle(vec2 coords, float r, vec2 uv) {
    float d = distance(coords, uv);

    float c = smoothstep(1.0 - r - 0.01, 1.0 - r, 1.0 - d);
    vec3 color = vec3(c);

    return c;
}

void mainImage( out vec4 fragColor, in vec2 fragCoord ) {
    vec2 uv = fragCoord.xy/iResolution.xy;

    float aspectRatio = iResolution.x / iResolution.y;
    uv.x *= aspectRatio;
    uv.x += (0.5 - 0.5 * aspectRatio);

    float r = 0.1;
    vec2 center = vec2(0.5);

    float color1 = circle(vec2(0.3), r, uv);
    float color2 = circle(vec2(0.7), r, uv);
    float color3 = circle(vec2(0.15, 0.3), r, uv);

    fragColor = vec4(vec3(color1 + color2 + color3), 1.0);
}
```

### Движение точки по круту в 3D
```c
// Here is description
// https://www.youtube.com/watch?v=dKA5ZVALOhs&list=PLGmrMu-IwbguU_nY2egTFmlg691DN7uE5&index=11

// расстояние от точки до линии (луча)
float distLine(vec3 ro, vec3 rd, vec3 p) {
    return length(cross(p - ro, rd)) / length(rd);
}

void mainImage( out vec4 fragColor, in vec2 fragCoord ) {
    float t = iTime;

    // Normalized pixel coordinates (from 0 to 1)
    vec2 uv = fragCoord/iResolution.xy;
    uv -= 0.5;
    uv.x *= iResolution.x / iResolution.y;

    // ray origin
    vec3 ro = vec3(0, 0, -2.);

    // ray direction
    vec3 rd = vec3(uv.x, uv.y, 0.) - ro;

    vec3 p = vec3(sin(t), 0, 2.0 + cos(t));
    float d = distLine(ro, rd, p);

    d = smoothstep(0.1, 0.09, d);

    fragColor = vec4(d );
}
```

### Куб в 3D с вращением
```c
// Here is description
// https://www.youtube.com/watch?v=dKA5ZVALOhs&list=PLGmrMu-IwbguU_nY2egTFmlg691DN7uE5&index=11

// расстояние от точки до линии (луча)
float distLine(vec3 ro, vec3 rd, vec3 p) {
     return length(cross(p - ro, rd)) / length(rd);
}

float drawPoint(vec3 ro, vec3 rd, vec3 p) {
    float d = distLine(ro, rd, p);
    d = smoothstep(0.06, 0.05, d);
    return d;
}

void mainImage( out vec4 fragColor, in vec2 fragCoord ) {
    float t = iTime;

    // Normalized pixel coordinates (from 0 to 1)
    vec2 uv = fragCoord/iResolution.xy;
    uv -= 0.5;
    uv.x *= iResolution.x / iResolution.y;

    // ray origin
    vec3 ro = vec3(3.0 * sin(t), 2.0, -3.0 * cos(t));

    vec3 lookat = vec3(0.5);

    // r - right - смещение по x (1, 0, 0)
    // u - up - смещение по y (0, 1, 0)
    // f - front - смещение по z (0, 0, 1)
    // forward
    vec3 f = normalize(lookat - ro);

    // right, насколько понял, (0, 1, 0) используется всегда
    vec3 r = cross(vec3(0, 1.0, 0), f);

    vec3 u = cross(f, r);

    float zoom = 1.2;

    // center
    vec3 c = ro + f * zoom;

    // intersection
    vec3 i = c + uv.x * r + uv.y * u;

    // ray direction
    vec3 rd = i - ro;
//    vec3 p = vec3(sin(t), 0, 2.0 + cos(t));
    vec3 p = vec3(0, 0, 0);

    float d = .0;
    d += drawPoint(ro, rd, vec3(0, 0, 0));
    d += drawPoint(ro, rd, vec3(0, 0, 1.0));
    d += drawPoint(ro, rd, vec3(0, 1.0, 0));
    d += drawPoint(ro, rd, vec3(0, 1.0, 1.0));
    d += drawPoint(ro, rd, vec3(1.0, 0, 0));
    d += drawPoint(ro, rd, vec3(1.0, 0, 1.0));
    d += drawPoint(ro, rd, vec3(1., 1.0, 0));
    d += drawPoint(ro, rd, vec3(1.0, 1.0, 1.0));
    fragColor = vec4(d );
}
```

### Мандельброт
```c
void mainImage( out vec4 fragColor, in vec2 fragCoord ) {
  vec2 uv = fragCoord/iResolution.xy;
    uv -= 0.5;
    uv.x *= iResolution.x / iResolution.y;
    
    fragColor = vec4(0, 0, 0, 1.0);
    
    vec2 z = uv;
    
    for (int i=0; i < 25; i++) {
      z = uv + vec2(
          z.x * z.x - z.y * z.y,
            2.0 * z.x * z.y
        );
        
        if (length(z) > 1.6) {
              fragColor = vec4(1.0, 0, 0, 1.0);
        }
    }
    
}
```
