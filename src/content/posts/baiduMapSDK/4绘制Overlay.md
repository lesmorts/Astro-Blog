---
title: 绘制Overlay
published: 2024-03-29
comment: false 
tags: [百度地图AndroidSDK, Android Studio, java]
category: 百度地图SDK
audio: false
---
## 以批量添加和删除Overlay为例

1.构建addOverlay函数

百度地图SDK为开发者提供一次性向地图上添加大批量Overlay的接口。
示例代码

```java
    private void addOverlay(){
//创建OverlayOptions的集合
List<OverlayOptions> options = new ArrayList<OverlayOptions>();
//构造坐标数据
        LatLng point1 = new LatLng(31.541778,104.69308);
        LatLng point2 = new LatLng(31.545255,104.694374);
        LatLng point3 = new LatLng(31.544086,104.696422);
//设置图片源
        BitmapDescriptor bdGround1 = BitmapDescriptorFactory.fromResource(R.drawable.overlay_1);
        BitmapDescriptor bdGround2 = BitmapDescriptorFactory.fromResource(R.drawable.overlay_2);
        BitmapDescriptor bdGround3 = BitmapDescriptorFactory.fromResource(R.drawable.overlay_3);
//创建OverlayOptions属性
        OverlayOptions option1 =  new MarkerOptions()
                .position(point1)
                .icon(bdGround1);
        OverlayOptions option2 =  new MarkerOptions()
                .position(point2)
                .icon(bdGround2);
        OverlayOptions option3 =  new MarkerOptions()
                .position(point3)
                .icon(bdGround3);
//将OverlayOptions添加到list
        options.add(option1);
        options.add(option2);
        options.add(option3);
//在地图上批量添加
        mBaiduMap.addOverlays(options);
    }
```

2.在spiner选择器构造函数中添加选项

实现效果如图：

![](https://lesmorts.github.io/picx-images-hosting/Screenshot_20240328_153851.45hfn19mpv.webp){height="550px" width="250px"}

3.批量清除：

```java
//清除地图上的所有覆盖物
mBaiduMap.clear();
```
