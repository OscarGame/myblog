+++
title= "[UE4]优化建议与经验"
date= "2016-12-09T19:17:02+08:00"
categories= ["UnrealEngine4"]
tags= ["UE4", "Performance", "Optimization"]
keywords= ["UE4性能优化", "UE4", "Performance", "Optimization"]
+++

keywords：UE4性能优化、Performance Optimization

1，材质尽量用mask，这样可以节省资源。如果用透明和半透明，会非常耗资源。

2，GPUProfile来统计性能消耗的时候，在editor模式下不是很准，因为编辑器的消耗也算进去了，如果要用，最好以Game模式来查看。

3，UE4不支持640X480的分辨率，如果在这个分辨率下运行程序，会导致程序崩溃。

4，如果角色身上有很多Component需要Attach，尽量在使用时Attach，不要一加载就全部attach，否则当场景中角色很多时，会有严重性能问题。

5，面数对UE4来说不敏感，即使在移动端。ipad 4上，50万的三角面，也能够以30fps帧率稳定运行，移动端主要对贴图大小、材质复杂度较为敏感。

6，地形编辑时，使用Instanced Static Meshes。Intancing会增加GPU的开销，但是可以显著降低CPU的开销。注意：Instancing不会减少CPU draw call次数，要减少draw call次数，需要减少材质种类，提供材质复用率。

7，3种光源的性能消耗从低到高：定向光/平行光(Directional Light) < 点光源(Point Light) < 聚光灯(Spot Light)。这个标准不局限于UE4，其他引擎也是这样。当光源数量在场景中达到一定量级时，3种灯光的性能差距也是数量级上差距。

8，C++ 比 蓝图快100到1000倍  
[Test] Blueprint vs C++ Performance vs Nativized BP  
https://www.reddit.com/r/unrealengine/comments/6qtxy3/test_blueprint_vs_c_performance_vs_nativized_bp/

##### 物理与碰撞优化

1，BoxComponent的 Generate Overlap Events 设置为false。如果不需要Overlap事件，那么就将该属性设置设置为false，默认为true。当BoxCompont达到一定量级时，开启Generate Overlap Events的性能消耗时关闭情况下的两倍。

2，如果不需要物理，将 `Simulate Physics` 设置为false。

3，如果不需要Hit事件，将 `Simulation Generates Hit Events` 设置为false。

4，Collision 的 Object Response 通道设置的越少越好，把可以设置为 Ignore 的通道都设置为 Ignore 。

5，如果大型RTS游戏，场景有海量单位时（比如星际2中大规模的虫族小狗），能不用UE4的 Collision 就不要用 Collision，否则帧数狂泻。  
建议自己实现一个简易的自定义Collision，比如球形Collision，然后计算该 Collision 与单位之间的直线距离，来判断是否是否发生了碰撞，并且降低检测间隔，比如 0.1秒。

##### 动画优化
1，打开角色蓝图 -》 MeshComponent -》 Detail 面板中的 Optimization 类别下 -》 勾选 `Enable Update Rate Optimizations`。

##### UI优化

Epic Games工程师分享：如何在移动平台上做UE4的UI优化？  
http://youxiputao.com/articles/11743

##### Dedicated Server优化
1，服务端cook时剥离动画数据  
Project Settings -> Engine -> Animation -> 勾选 Strip Animation Data on Dedicated Server.  
如果在动画中添加了触发修改数据的 Notify Event，勾选此选项会有问题。请确保动画做挂载的 Notify 只是表现相关，不涉及游戏逻辑。

2，Server模式下禁用角色物理模拟  

`FBodyInstance->bSimulatePhysics` 设置为false。默认为false。

`SkeletalMeshComponent::bEnablePhysicsOnDedicatedServer` 设置为 false ， 默认为 true 。但这样会导致物理验算以客户端为准，有被外挂hack的风险。bEnablePhysicsOnDedicatedServer 在 run-time 修改不生效。

3，Server模式下禁用 Collision  

`UPrimitiveComponent->bGenerateOverlapEvents` 设置为false，角色蓝图中的 CollisionComponent 默认为true。

4，Server模式下Detach角色身上所有的装饰性组件
