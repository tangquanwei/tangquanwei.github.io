---
title: Unity3D 入门
tags: Unity3D
date: 2023-12-30 23:01:48
cover: 20231231095827.png
---
## GameObject

在Unity中，`GameObject` 是场景中所有实体的基类。它为场景中的对象提供了一个通用的结构和一系列的属性和方法。`GameObject` 本身并不包含任何渲染或物理功能，但它可以包含其他组件，如 `MeshRenderer`、`Collider`、`Rigidbody` 等，这些组件赋予了 `GameObject` 实际的功能和特性。
以下是 `GameObject` 的一些关键特性和常用的属性和方法：

### 属性

- `activeInHierarchy`：定义了 `GameObject` 是否在场景中激活。
- `activeSelf`：表示 `GameObject` 本身的激活状态，不受父物体的影响。
- `transform`：包含了 `GameObject` 的位置、旋转和缩放信息。
- `renderer`：用于渲染 `GameObject` 的网格。
- `collider`：用于处理 `GameObject` 的碰撞。
- `rigidbody`：如果 `GameObject` 拥有刚体属性，这个组件会包含相关的物理信息。
- 
### 方法

- `Find`：在场景中查找具有特定名称的 `GameObject`。
- `FindWithTag`：根据 `GameObject` 的标签来查找。
- `FindAll`：查找与特定查询匹配的所有 `GameObject`。
- `SetActive`：设置 `GameObject` 的激活状态。
- `Destroy`：销毁 `GameObject` 和其上的所有组件。
- `AddComponent`：向 `GameObject` 添加一个新的组件。
- `RemoveComponent`：从 `GameObject` 中移除一个组件。
- 
### 创建和操作

在Unity中，可以通过预制体（Prefab）来创建 `GameObject` 的实例。预制体是一个包含 `GameObject` 结构和组件的模板，可以用于快速创建具有特定功能的对象。创建 `GameObject` 实例后，可以通过脚本或编辑器来操作它们，比如改变它们的位置、旋转、缩放，或者为它们添加新的组件。

### 示例代码

以下是一个简单的示例，展示了如何在Unity中查找和操作 `GameObject`：

```csharp
using UnityEngine;
public class ExampleClass : MonoBehaviour
{
    public GameObject myObject;
    void Start()
    {
        // 在场景中查找名为 "MyObject" 的 GameObject
        myObject = GameObject.Find("MyObject");
        // 如果找到了，设置它为激活状态
        if (myObject != null)
        {
            myObject.SetActive(true);
        }
    }
    void Update()
    {
        // 在场景中查找所有标签为 "MyTag" 的 GameObject
        foreach (GameObject obj in GameObject.FindGameObjectsWithTag("MyTag"))
        {
            // 对找到的对象进行操作
            Debug.Log(obj.name);
        }
    }
}
```

## 坐标系

Unity中的坐标系是用来确定和表示游戏中对象（如Cube、Sphere等）在三维空间中的位置、方向和缩放的一套标准。在Unity中主要有以下几种坐标系：
1. 世界坐标系 (World Space)
世界坐标系是游戏世界中所有物体共同的参照系。它以世界原点为中心，使用左手定则来定义X、Y、Z轴的方向，其中Z轴为正向，X轴向右，Y轴向上。世界坐标系是固定不变的，所有物体在世界坐标系中的位置、旋转和缩放都是相对于这个全局的坐标系来确定的。我们可以通过`transform.position`来获取物体的世界坐标。
2. 局部坐标系 (Local Space)
局部坐标系是相对于物体的父节点来定义的。每个物体都有自己的局部坐标系，这个坐标系会随着物体相对于父节点的移动和旋转而改变。在Unity中，父节点和子节点之间的坐标系是对齐的，子节点的局部坐标可以通过`transform.localPosition`来获取。
3. 屏幕坐标系 (Screen Space)
屏幕坐标系是用于表示屏幕上的像素点的坐标系。它以屏幕的左下角为原点(0,0)，右上角为屏幕的宽度和高度。屏幕坐标系主要用来表示UI元素和屏幕上的交互点（如鼠标和触摸位置）。我们可以通过`Input.mousePosition`来获取当前鼠标的屏幕坐标。
4. 视口坐标系 (ViewPort Space)
视口坐标系是相机视角下的一个坐标系，它定义了相机视野内的点的位置。在视口坐标系中，相机的左下角为(0,0)，右上角为(1,1)，Z轴的位置是以相机的世界单位来衡量的。视口坐标系常用于确定渲染到屏幕上的点的位置。我们可以通过`Camera.ViewportToWorldPoint`和`Camera.WorldToViewportPoint`方法来在视口坐标系和世界坐标系之间进行转换。
5. GUI坐标系
GUI坐标系是在Unity中绘制UI元素时使用的坐标系。它以屏幕的左上角为原点(0,0)，右下角为屏幕的宽度和高度。GUI坐标系与屏幕坐标系类似，但它的原点在左上角。
在Unity中，各种坐标系之间的转换是非常常见的操作。例如，我们可能需要将屏幕坐标系中的点转换为世界坐标系中的点，或者将世界坐标系中的点转换为视口坐标系中的点。Unity提供了相应的数学方法和API来帮助我们进行这些转换，如`Camera.WorldToScreenPoint`、`Camera.ScreenToWorldPoint`、`Camera.ViewportToWorldPoint`和`Camera.WorldToViewportPoint`等。
了解和掌握这些坐标系及其转换方法对于Unity开发者来说是非常重要的，它们是进行游戏开发和实现各种游戏功能的基础。

## Application

Unity 中的 `Application` 类是一个基础组件，它提供了一系列与应用程序运行时管理相关的功能。这个类不是直接实例化的；而是通过 `Application` 类本身作为静态类来访问其成员。
以下是 `Application` 类的一些关键方面：
1. **静态属性**：`Application` 类包含多个静态属性，提供关于应用程序运行时状态及其环境的信息。例如：
   - `dataPath`：返回存储应用程序数据的目录路径。这个路径是相对的，可能会因平台而异。
   - `persistentDataPath`：类似于 `dataPath`，但这个路径用于存储即使在应用程序更新或重新安装后也需要持久的数据。
   - `streamingAssetsPath`：用于从互联网上流式传输或在运行时加载的资产。
   - `temporaryCachePath`：用于临时文件，这些文件不需要在应用程序会话之间持久存在。
2. **互联网可达性**：`Application` 类提供了检查互联网可达性的方法，这可以用于确定网络状态并相应地调整应用程序行为。
3. **场景管理**：`Application` 有用于加载和切换场景的方法，如 `LoadLevel` 和 `ChangeScene`。然而，这些方法已被弃用，转而使用 `SceneManager` 类，后者提供了更强大的场景管理功能。
4. **进程控制**：您可以使用 `Application` 来启动外部进程，打开 URL 或执行其他任务，比如使用 `Process.Start` 方法。
5. **应用程序版本**：`Application.version` 属性提供了创建应用程序时使用的 Unity 编辑器的版本，这与应用程序的版本号可能不同。
6. **装饰器**：在设计模式方面，`Application` 类可以被装饰以扩展其功能，而不修改原始类。这允许动态地添加其他行为，例如日志记录、错误处理或其他跨切关注点。
7. **内存管理**：`Application` 类还有用于管理内存的方法，如 `Memory.Dispose`，尽管这通常由 Unity 自动处理，不需要手动干预。
在使用 `Application` 类时，需要考虑适当的上下文和平台，因为某些方法和属性可能在不同的情况下（如 Unity 编辑器、独立应用程序或 Web 播放器）表现出不同的行为。
总之，Unity 中的 `Application` 类提供了一系列与应用程序运行时交互、管理场景、与外部进程工作以及提供应用程序环境信息的方法和属性。它是 Unity API 的关键部分，广泛应用于游戏开发的各个方面。

## Transform

控制game object的位置、旋转、缩放
控制父子级

## 物理材质

物理材质是计算机图形学中的一个概念，特别是在游戏开发中，它用于模拟物体表面的物理属性，如摩擦力和弹性。物理材质可以影响物体间的相互作用，如物体之间的碰撞、滑动和反弹等。在Unity等游戏引擎中，物理材质是一个重要的资源，它定义了物体表面的动态和静态摩擦力、弹跳力等特性。
创建和使用物理材质的基本步骤如下：
1. 创建物理材质：在Unity中，可以通过Project窗口右键选择Create -> Physics Material来创建一个新的物理材质。
2. 设置物理材质属性：物理材质包含几个关键属性，包括Dynamic Friction（动态摩擦力）、Static Friction（静态摩擦力）、Bounciness（弹跳力）以及用于定义摩擦力和弹跳力如何与其他物体相作用的Friction Combine和Bounce Combine。动态摩擦力是指物体在滑动时的摩擦力，静态摩擦力是指物体静止时的摩擦力，而弹跳力则决定了物体在碰撞后的反弹程度。
3. 应用物理材质：创建物理材质后，需要将其应用到具有物理属性的物体上。这通常是通过将物理材质拖拽到物体的碰撞器组件上实现的。
4. 运行和测试：应用物理材质后，运行游戏并测试物体的物理反应，以确保物理材质的效果符合预期。
物理材质在游戏开发中非常重要，因为它可以显著影响游戏物体的行为和玩家的游戏体验。通过合理设置和使用物理材质，可以创建出更加真实和有趣的游戏环境。