# Встроенные функции
 - **length(float a)** - получить длину вектора  
 - **dot(vec2 a, vec2 b)** - получить скалярное произведение двух векторов  
 - **cross(vec2 a, vec2 b)** - векторное произведение  
 - **reflect(vec2 I, vec2 N)** - получить отраженный вектор (I - падающий, N - вектор нормали)  
 - **refract(vec2 I, vec2 N, float eta)** - получить преломленный вектор  
   Пример: свет проходящий через границу воды  
   eta - коэффициент преломления  
 - **faceforward(vec2 N, vec2 I, vec2 Nref)** - dot(Nref, i) < 0 ? N : -N  
 - **step(float base, float value)** - return value < base ? 1. : 0.;  
 - **abs, sin, cos, tan, ...**  
 - **float distance(vec2 p1, vec2 p2)** - возвращает расстояние между двумя точками  
 - **smoothstep(float from, float to, float x)** - позволяет интерполировать значения  
  Например, с помощью этой функции можно создавать размытые края у шариков  
  from - минимальное значение, если x меньше него, будет возвращен 0  
  to - максимальное значение, если x меньше него, будет возвращен 1  
  между from и to будут возвращаться значения от 0 до 1  
 - **texture(texture, coords)**  
 - **clamp(x, minVal, maxVal)** - получить среднее значение (зажатое между двумя остальными)  
   Например: min(max(x, minVal), maxVal)  
 - **ddx(x)** - Возвращает частную производную x относительно screen-space x-координаты  
 - **ddx(y)** - Возвращает частную производную y относительно screen-space y-координаты  
 - **lerp(a, b, s)** - Возвращает a + s (b - a)  
 - **mul(a, b)** - Делает матричное умножение между a и b  


## Полезные ссылки
 - [https://gamedev.ru/code/articles/HLSL?page=2](https://gamedev.ru/code/articles/HLSL?page=2)  
 - [http://www.shaderific.com/glsl-functions/](http://www.shaderific.com/glsl-functions/)  
 - [https://thebookofshaders.com/05/](https://thebookofshaders.com/05/)  
 - [Мануал по многим функциям](http://www.shaderific.com/glsl-functions)  
