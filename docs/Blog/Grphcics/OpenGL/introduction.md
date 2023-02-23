`glCreateShader` 不在GL2.0 的`gl.h` 中， 需要切换到gl3（切换头文件为 `glew.h` 或 `GLES3/ *`)


## Stack of Matrices
`glPushMartix()和 glPopMatrix` 来保存前面图像的位置(用stack来保存不进行 transition、scale、translation 的图形)

## VBO
```cpp
glGenBuffers(1, &bufId);
glBindBuffer(GL_ARRAY_BUFFER, bufId);
glBufferData(GL_ARRAY_BUFFER, sizeof(vertices),
				vertices, GL_STATIC_DRAW);
```


## Attribute 输入
- 区分步长，顶点坐标与颜色信息
![](attachments/Pasted%20image%2020221114204050.png)

```cpp
Gluint attribPos;
attribPos = glGetAttribLocation(shaderProgram, "inPosition");
glEnableVeretexAttribArray(attribPos);
glBindBuffer(GL_ARRAY_BUFFER, {vertexBuffer/PositionsData});
glVertexAttribPointer(attribPosition, 3
					 , GL_FLOAT, GL_FALSE,
					 0, 0);
```

## Uniforms
```cpp
Gluint uniformMat;
uniformMat = glGetUniformLocation(shaderprogram,"matrix");
glUniformMatrix4fv(uniformMat,1,  GL_FALSE, &matrix);
```
![](attachments/Pasted%20image%2020221114211342.png)


## 关键字

input : attribute
output: varying

![](attachments/Pasted%20image%2020221114202514.png)


## Full screen rendering

gl_FragCoord.xyz


## Translation
- 顺次相乘在相加
$b_ij = a_i0*b_j0+ ...+ a_in *b_jn$

![](attachments/Pasted%20image%2020221116103553.png)

## 齐次坐标 homogeneous coordinated
![](attachments/Pasted%20image%2020221116104101.png)


### Rotation

![](attachments/Pasted%20image%2020221116104407.png)
![](attachments/Pasted%20image%2020221116104400.png)
![](attachments/Pasted%20image%2020221116104322.png)