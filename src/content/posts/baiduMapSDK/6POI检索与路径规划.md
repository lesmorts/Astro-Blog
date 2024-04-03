---
title: POI检索与路径规划
published: 2024-03-29
comment: false 
tags: [百度地图AndroidSDK, Android Studio, java]
category: 百度地图SDK
audio: false
---
POI搜索和路径规划总体实现了搜索+路径规划的功能，所以就一起写了

## POI检索简介

POI（Point of Interest），即“兴趣点”。在地理信息系统中，一个POI可以是一栋房子、一个景点、一个邮筒或者一个公交站等。
百度地图SDK提供三种类型的POI检索：城市内检索、周边检索和区域检索（即矩形区域检索）。下面步骤以城市内检索为例

## 布局文件中加入输入框和搜索按钮

```xml
<TextView
        android:id="@+id/Text_1"
        android:layout_width="30dp"
        android:layout_height="43dp"
        android:textSize="16dp"
        app:layout_constraintTop_toBottomOf="@id/bmapView"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintEnd_toStartOf="@id/editText_search"
        android:gravity="center"
        android:text="@string/zai"/>

    <!-- EditText 1 -->
    <EditText
        android:id="@+id/editText_search"
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        android:textSize="16dp"
        android:inputType="text"
        android:hint='绵阳'
        app:layout_constraintTop_toBottomOf="@id/bmapView"
        app:layout_constraintStart_toEndOf="@id/Text_1"
        app:layout_constraintEnd_toStartOf="@id/guideline" />

    <!-- TextView 2 -->
    <TextView
        android:id="@+id/Text_2"
        android:layout_width="50dp"
        android:layout_height="43dp"
        android:textSize="16dp"
        app:layout_constraintTop_toBottomOf="@id/bmapView"
        app:layout_constraintStart_toEndOf="@id/guideline"
        app:layout_constraintEnd_toStartOf="@id/editText_search_1"
        android:gravity="center"
        android:text="@string/Text_2"/>

    <!-- EditText 2 -->
    <EditText
        android:id="@+id/editText_search_1"
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        android:inputType="text"
        android:layout_marginEnd="16dp"
        android:textSize="16dp"
        android:hint="西南科技大学"
        app:layout_constraintTop_toBottomOf="@id/bmapView"
        app:layout_constraintStart_toEndOf="@id/Text_2"
        app:layout_constraintEnd_toStartOf="@id/button_search"
        app:layout_constraintBottom_toTopOf="@id/mapTypeRadioGroup"
        android:imeOptions="actionSearch"
        android:autofillHints="" />

    <!-- Guideline -->
    <androidx.constraintlayout.widget.Guideline
        android:id="@+id/guideline"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:orientation="vertical"
        app:layout_constraintGuide_percent="0.2"/>

    <!-- Search Button -->
    <Button
        android:id="@+id/button_search"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:gravity="center"
        android:drawableLeft="@drawable/search"
        android:text="@string/Search"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintTop_toTopOf="@id/Text_2"
        app:layout_constraintBottom_toBottomOf="@id/Text_2" />
```

注：使用了相对位置，具体完整布局代码见 ，其中 `android:drawableLeft="@drawable/search"`为搜索图标

添加后样式：

![](https://lesmorts.github.io/picx-images-hosting/1.1aorh9fdf5.webp)

## 创建POI

1.在MainActivity中添加PoiSearch变量

`private PoiSearch mPoiSearch;`

2.创建searchPOI函数

```java
private void searchPOI(){
    //获取两个EditText中的文本，即搜索城市和搜索地址
        EditText editTextCity = findViewById(R.id.editText_search);
        String searchCity = editTextCity.getText().toString();
        EditText editTextKeyword = findViewById(R.id.editText_search_1);
        String searchKeyword = editTextKeyword.getText().toString();
    //实例化mPoiSearch
        mPoiSearch = PoiSearch.newInstance();
    //创建POI检索监听器接收检索结果
        OnGetPoiSearchResultListener listener = new OnGetPoiSearchResultListener() {
            @Override
            public void onGetPoiResult(PoiResult poiResult) {
   
            }

            @Override
            public void onGetPoiDetailResult(PoiDetailSearchResult poiDetailSearchResult) {

            }
            @Override
            public void onGetPoiIndoorResult(PoiIndoorResult poiIndoorResult) {

            }
            //废弃
            @Override
            public void onGetPoiDetailResult(PoiDetailResult poiDetailResult) {

            }

        };
    //设置检索监听器
        mPoiSearch.setOnGetPoiSearchResultListener(listener);
//发起检索
        editTextKeyword.setOnEditorActionListener(new EditText.OnEditorActionListener() {
            @Override
            public boolean onEditorAction(TextView v, int actionId, KeyEvent event) {
                boolean ret=false;
                if(actionId== EditorInfo.IME_ACTION_SEARCH)
                {
                    ret=mPoiSearch.searchInCity(new PoiCitySearchOption()
                            .city(searchCity)
                            .keyword(searchKeyword)
                            .pageNum(1));
                    //搜索后隐藏键盘
                    InputMethodManager imum=(InputMethodManager)getSystemService(INPUT_METHOD_SERVICE);
                    View view=getWindow().peekDecorView();
                    if(view!=null)
                    {
                        imum.hideSoftInputFromWindow(view.getWindowToken(),0);
                    }
                }
                return ret;
            }
        });
}
```

### layout中添加搜索结果布局文件poi_item.xml

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="horizontal">

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:layout_weight="8"
        android:orientation="vertical">
        <TextView
            android:id="@+id/poiname"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:layout_weight="1"
            android:textColor="@color/white"
            android:textSize="24sp"
            tools:ignore="NestedWeights" />

        <TextView
            android:id="@+id/poiaddress"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:layout_weight="1"
            android:textColor="@color/white"
            android:textSize="14sp" />
    </LinearLayout>

</LinearLayout>
```

### 在activity_main.xml中添加listView控件(activity_main.xml完整代码见 )

```xml
<ListView
        android:id="@+id/searchResult"
        android:layout_width="match_parent"
        android:layout_height="550dp"
        android:layout_marginTop="120dp"
        android:background="@color/material_on_background_emphasis_high_type"
        android:visibility="gone"
         />
```

### 在MainActivity同级路径新建PoiAdapter适配器类

```java
package com.example.lmap;
 
import android.content.Context;
import android.view.LayoutInflater;
import android.view.View;
import android.view.ViewGroup;
import android.widget.ArrayAdapter;
import android.widget.TextView;
 
import androidx.annotation.NonNull;
import androidx.annotation.Nullable;
 
import com.baidu.mapapi.search.core.PoiInfo;
 
import java.util.List;
 
public class PoiAdapter extends ArrayAdapter<PoiInfo> {
    private int resourceId;
    public PoiAdapter(@NonNull Context context, int resource, @NonNull List<PoiInfo> objects) {
        super(context, resource, objects);
        resourceId=resource;
    }
 
    @NonNull
    @Override
    public View getView(int position, @Nullable View convertView, @NonNull ViewGroup parent) {
        PoiInfo poi=getItem(position);
        View view= LayoutInflater.from(getContext()).inflate(resourceId,null);
        TextView name=view.findViewById(R.id.poiname);
        TextView address=view.findViewById(R.id.poiaddress);
        name.setText(poi.name);
        address.setText(poi.address);
        return view;
    }
}
```

### 通过POI检索监听器在listView中填充内容

```java
@Override
            public void onGetPoiResult(PoiResult poiResult) {
                List<PoiInfo> poiList=poiResult.getAllPoi();
                PoiAdapter adapter=new PoiAdapter(MainActivity.this,R.layout.poi_item,poiList);
                ListView listView=findViewById(R.id.searchResult);
                listView.setAdapter(adapter);
                listView.setVisibility(View.VISIBLE);
                //当滑动到底部时加载更多搜索结果
                listView.setOnScrollListener(new ListView.OnScrollListener() {
                    @Override
                    public void onScrollStateChanged(AbsListView view, int scrollState) {
                        if (scrollState == AbsListView.OnScrollListener.SCROLL_STATE_IDLE) {
                            // 判断是否滚动到底部
                            if (view.getLastVisiblePosition() == view.getCount() - 1) {
                                //加载更多
                                int curPage = poiResult.getCurrentPageNum();
                                int totalPage = poiResult.getTotalPageNum();
                                if (curPage < totalPage) {
                                    poiResult.setCurrentPageNum(curPage + 1);
                                    mPoiSearch.searchInCity(new PoiCitySearchOption()
                                            .city(searchCity)
                                            .keyword(searchKeyword)
                                            .pageNum(curPage + 1));
                                }
                            }
                        }
                    }
                    @Override
                    public void onScroll(AbsListView view, int firstVisibleItem, int visibleItemCount, int totalItemCount) {
                    }
                });
            }
```

## 点击搜索结果列表外区域时隐藏结果列表

重写dispatchTouchEvent函数，实现点击其他区域时隐藏结果列表

```java
     @Override
    public boolean dispatchTouchEvent(MotionEvent ev) {
     if (ev.getAction() == MotionEvent.ACTION_DOWN) {
         ListView listView = findViewById(R.id.searchResult);
         int left = listView.getLeft(), top = listView.getTop(), right = left + listView.getWidth(), bottom = top + listView.getHeight();
         if (ev.getX() < left || ev.getX() > right || ev.getY() < top || ev.getY() > bottom)
             //点击搜索结果列表之外区域，隐藏搜索结果列表
             listView.setVisibility(View.GONE);
     }
     return super.dispatchTouchEvent(ev);
 }
```

## 下载官方Demo中的Overlay

[下载链接](https://lbsyun.baidu.com/faq/api?title=androidsdk/sdkandev-download)

> 路径：BaiduMap_AndroidSDK_v7_6_1_Sample\BaiduMap_AndroidSDK_v7_6_1_Sample\BaiduMapsApiASDemo\app\src\main\java\com\baidu\mapapi\overlayutil
>
> 用到的文件：
>
> ![](https://lesmorts.github.io/picx-images-hosting/2.1lblag6v5g.webp)

注：作者对上述文件进行了部分重写，改变了图片源和部分函数，详见[github]()

## 实现驾车路线规划

### 在MainActivity中添加RoutePlanSearch变量

`private RoutePlanSearch mRoutePlanSrch;`

### 在addPOI函数中实例化并创建监听

```java
        mRoutePlanSrch = RoutePlanSearch.newInstance();
        //创建路线规划检索结果监听器
        OnGetRoutePlanResultListener routePlanSrchlistener = new OnGetRoutePlanResultListener() {
            @Override
            public void onGetWalkingRouteResult(WalkingRouteResult walkingRouteResult) {

            }

            @Override
            public void onGetTransitRouteResult(TransitRouteResult transitRouteResult) {

            }

            @Override
            public void onGetMassTransitRouteResult(MassTransitRouteResult massTransitRouteResult) {

            }

            @Override
            public void onGetIndoorRouteResult(IndoorRouteResult indoorRouteResult) {

            }

            @Override
            public void onGetBikingRouteResult(BikingRouteResult bikingRouteResult) {

            }

            @Override
            public void onGetDrivingRouteResult(DrivingRouteResult drivingRouteResult) {
                overlay = new DrivingRouteOverlay(mMapView.getMap());
                List<DrivingRouteLine> routelines=drivingRouteResult.getRouteLines();
                if(routelines!=null&&routelines.size()>0)
                {
                    for(DrivingRouteLine routeLine:routelines
                    ) {
                        mBaiduMap.clear();
                        //为DrivingRouteOverlay实例设置数据
                        overlay.setData(routeLine);
                        //在地图上绘制DrivingRouteOverlay
                        overlay.addToMap();
                        MapStatusUpdate u = MapStatusUpdateFactory.newLatLng(overlay.getCenter());  //更新坐标位置
                        mBaiduMap.animateMapStatus(u);
                    }
                    //a关闭搜索结果
                    ListView listView = findViewById(R.id.searchResult);
                    listView.setVisibility(View.GONE);
                }
            }
        };
        mRoutePlanSrch.setOnGetRoutePlanResultListener(routePlanSrchlistener);
```

## 添加路线规划按钮

在poi_item.xml布局中添加按钮

```xml
 <Button
        android:id="@+id/goBtn"
        android:layout_width="wrap_content"
        android:layout_height="match_parent"
        android:layout_weight="2"
        android:background="@color/tinyBlue"
        android:text="@string/goToThr"
        app:backgroundTint="@color/littleBlue" />
```

实现效果如下

![](https://lesmorts.github.io/picx-images-hosting/Screenshot_20240328_163955.6wqhv61u6c.webp){height="550px" width="250px"}

## 发起路线规划

在addPOI最后添加

```
//进行搜索
mPoiSearch.searchInCity(new PoiCitySearchOption()
                .city(searchCity)
                .keyword(searchKeyword)
                .pageNum(1));
//设置键盘隐藏
        InputMethodManager imum=(InputMethodManager)getSystemService(INPUT_METHOD_SERVICE);
        View view=getWindow().peekDecorView();
        if(view!=null)
        {
            imum.hideSoftInputFromWindow(view.getWindowToken(),0);
        }
```

在MainActivity的onCreate函数中对“去这里”按钮进行监听

```java
//通过id获取button
Button buttonSearch = findViewById(R.id.button_search);
//设置监听函数
        buttonSearch.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                searchPOI();
            }
        });
```

## 最终结果

![](https://lesmorts.github.io/picx-images-hosting/Screenshot_20240328_164803.8s32nsp6rl.webp){height="550px" width="250px"}
