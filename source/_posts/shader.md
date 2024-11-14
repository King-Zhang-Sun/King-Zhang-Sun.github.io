---
title: Shader 系列文章（一）：Shader 基础使用
date: 2024-07-02 12:26:48
categories:
- 着色器
tags:
- Shader
---

# 斗之气

听到着色器的第一反应：着色器？what？放弃放弃！！！
然后：
啪！啪！啪！
要想学的广！就得打脸打得响！！！
so !!!
let't go !!!

### Hello World

```glsl
#ifdef GL_ES
precision mediump float; // 中等精度
#endif

uniform float u_time; // 统一值，时间（加载后的秒数）iTime
uniform vec2 u_resolution; // 画布尺寸（宽，高）iResolution
uniform vec2 u_mouse; // 鼠标位置（在屏幕上哪个像素）iMouse xy：当前位置, zw：点击位置

void main() {
    gl_FragColor = vec4(abs(sin(u_time)),0.0,1.0,1.0); // 红绿蓝和透明度通道
}
```

### Uniforms

### gl_FragColor

### GLSL 基本类型

| 类型           | 说明                                    |
| :--------------: | :---------------------------------------: |
| `void`         | 空类型，即：不返回任何值                |
| `bool`         | 布尔类型，值：`true`, `false`           |
| `int`          | 带符号的整数 `signed integer`           |
| `float`        | 带符号的浮点数 `floating scalar`        |
| `vec2`, `vec3`, `vec4` | n维浮点数向量 `n-component floating point vector` |
| `bvec2`, `bvec3`, `bvec4` | n维布尔向量 `Boolean vector`   |
| `ivec2`, `ivec3`, `ivec4` | n维整数向量 `signed integer vector` |
| `mat2`, `mat3`, `mat4` | 2x2, 3x3, 4x4 浮点数矩阵 `float matrix` |
| `sampler2D`    | 2D纹理 `a 2D texture`                   |
| `samplerCube`  | 盒纹理 `cube mapped texture`            |


### GLSL 基本结构和数组

| 类型           | 说明                                    |
| :--------------: | :---------------------------------------: |
| `结构`         | `struct type-name{}` 类似c语言中的 结构体    |
| `数组`         | `float foo[3]` glsl只支持1维数组,数组可以是结构体的成员   |