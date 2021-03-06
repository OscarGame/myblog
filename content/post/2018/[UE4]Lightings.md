+++
title= "[UE4]Lightings"
date= "2018-04-27T14:14:02+08:00"
categories= ["UnrealEngine4"]
tags= ["UE4"]
keywords= ["UE4", "Lightings"]
+++

keywords：UE4、Lighting、灯光

以下问题都是基于4.18，不排除后续新版本变动。

##### Lighting Content Examples

https://docs.unrealengine.com/en-us/Resources/ContentExamples/Lighting


##### Light Propagation Volumes
Light Propagation Volumes  
https://docs.unrealengine.com/en-us/Engine/Rendering/LightingAndShadows/LightPropagationVolumes

Dynamic GI : Getting the Most out of LPV ( Light Propagation Volume )  
https://forums.unrealengine.com/community/community-content-tools-and-tutorials/103572-dynamic-gi-getting-the-most-out-of-lpv-light-propagation-volume

##### Lighting Channels
https://docs.unrealengine.com/en-us/Engine/Rendering/LightingAndShadows/LightingChannels

注意事项：

+ Lighting Channels是动态的，意思是：静态光(Static Lights)或者Mobility为Static的Static Mesh Actor不受Lighting Channels影响。要使用Lighting Channels，Static Mesh Actor和Lights的Mobility必须设置为Stationary或者Movable。
+ 材质类型影响：Lighting Channels只影响直接光照(direct lighting)下Opaque类型材质，Translucent或者Masked类型材质没有效果。
+ Lighting Channels对性能开销不大，可以忽略不计。

移动端的Lighting Channels：

+ 4.13版本开始，才支持移动端的Lighting Channel。
+ 移动端多个Directional Light支持不同Lighting Channels。
+ 一个Primitives只受一个Directional Light影响，如果primitves勾选了多个Lighting Channel，那么只会启用第一个。
+ CSM Shadows只会投射到与光源Lighting Channels相同的primitives上。
+ 动态点光源在移动端支持Lighting Channels的所有特性，与桌面级特性相同。

另外：Lighting Channels不支持运行时修改，就是说Lighting Channels在Actor创建时(比如BeginPlay)设置好以后，之后就无法再修改。

##### Occlusion Culling 光源距离裁剪
Project Settings -》 Engine -》 Rendering -》 Culling

Lights view distance  
https://forums.unrealengine.com/unreal-engine/feedback-for-epic/54065-lights-view-distance


##### 移动端的灯光问题

1，移动端不支持Movable的Spotlight，Spotlight在移动端只支持Static和Stationary；但是Movable的Pointlight在移动端也支持。

***
`我只担心一件事，我怕我配不上自己所受的苦难。----陀思妥耶夫斯基`