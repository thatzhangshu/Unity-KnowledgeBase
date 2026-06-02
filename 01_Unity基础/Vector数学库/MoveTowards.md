你截图里的接口是：

```csharp
Vector2.MoveTowards(...)
```

这是 Unity 中非常常用的一个**向目标点匀速移动**接口。

---

## 接口定义

```csharp
public static Vector2 MoveTowards(
    Vector2 current,
    Vector2 target,
    float maxDistanceDelta
);
```

参数：

|参数|含义|
|---|---|
|current|当前位置|
|target|目标位置|
|maxDistanceDelta|本次允许移动的最大距离|

返回值：

```csharp
返回移动后的新坐标
```

---

## 最简单例子

```csharp
Vector2 pos = Vector2.zero;

pos = Vector2.MoveTowards(
    pos,
    new Vector2(10,0),
    1f
);
```

执行后：

```text
当前位置：(0,0)

目标位置：(10,0)

本次最大移动距离：1
```

结果：

```text
(1,0)
```

---

## 连续执行效果

假设每帧执行：

```csharp
transform.position =
    Vector2.MoveTowards(
        transform.position,
        target.position,
        speed * Time.deltaTime
    );
```

例如：

```csharp
speed = 5;
```

那么：

```text
每秒移动5个单位
```

直到到达目标点。

---

## 为什么经常配合 Time.deltaTime

教程里你大概率会看到：

```csharp
Vector2.MoveTowards(
    transform.position,
    target.position,
    speed * Time.deltaTime
)
```

原因：

### 不乘 deltaTime

```csharp
speed = 5
```

表示：

```text
每帧移动5米
```

60FPS：

```text
60 × 5 = 300米/秒
```

120FPS：

```text
120 × 5 = 600米/秒
```

速度会变。

---

### 乘 deltaTime

```csharp
speed * Time.deltaTime
```

变成：

```text
每秒移动 speed 米
```

与帧率无关。

---

## MoveTowards 与 Lerp 区别（面试常考）

### MoveTowards

```csharp
MoveTowards(current,target,speed)
```

特点：

```text
匀速运动
```

例如：

```text
每秒5米
每秒5米
每秒5米
```

速度恒定。

---

### Lerp

```csharp
Vector2.Lerp(a,b,t)
```

特点：

```text
越来越慢
```

例如：

```text
100%
50%
25%
12%
6%
...
```

会无限接近目标。

---

## MoveTowards 的重要特性

假设：

```csharp
current = 0
target = 10
maxDistanceDelta = 20
```

因为：

```text
本次移动距离 > 剩余距离
```

Unity 会直接返回：

```text
10
```

不会冲过目标点。

所以：

```csharp
MoveTowards
```

天然防止：

```text
Overshoot（越界）
```

---

## 在你的 SLG-UI-Framework 中有哪些应用

以后你做：

### WorldUI 跟随

```csharp
NamePlate
```

平滑跟随城池位置

---

### 行军动画

```csharp
Troop
```

从 A 点移动到 B 点

---

### HUD 动画

例如：

```text
血条缓慢下降
经验条增长
资源数字飞动
```

---

### 摄像机平滑移动

```csharp
Camera.MoveTowards(...)
```

都可以用。

---

## 面试回答

如果面试官问：

> Vector2.MoveTowards 和 Lerp 有什么区别？

可以回答：

> MoveTowards 按固定速度向目标点移动，不会越过目标点，常用于角色移动、摄像机跟随等场景；Lerp 根据插值比例计算位置，运动过程中会逐渐减速，更适合 UI 动画和过渡效果。MoveTowards 常与 Time.deltaTime 配合实现帧率无关的匀速运动。

这是 Unity 面试里比较标准的回答。

---
