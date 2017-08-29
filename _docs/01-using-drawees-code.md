---
docid: using-drawees-code
title: 在JAVA代码中使用Drawees
layout: docs
permalink: /docs/using-drawees-code.html
prev: using-drawees-xml.html
next: drawee-branches.html
---

### 设置或更改要显示的图片

最简单的方法就是：

```
mSimpleDraweeView.setImageURI(uri);
```

如果要更加复杂的配置，可使用[ControllerBuilder](using-controllerbuilder.html);

### 自定义DraweeHierarchy

一般情况下，在[XML设置显示效果即可](using-drawees-xml.html), 如果想更多定制化，可以这样:

创建一个 builder 然后设置给 DraweeView:

```java
List<Drawable> backgroundsList;
List<Drawable> overlaysList;
GenericDraweeHierarchyBuilder builder =
    new GenericDraweeHierarchyBuilder(getResources());
GenericDraweeHierarchy hierarchy = builder
    .setFadeDuration(300)
    .setPlaceholderImage(new MyCustomDrawable())
    .setBackgrounds(backgroundList)
    .setOverlays(overlaysList)
    .build();
mSimpleDraweeView.setHierarchy(hierarchy);
```

对于同一个View，请不要多次调用`setHierarchy`，即使这个View是可回收的。创建 DraweeHierarchy 是较为耗时的一个过程，应该多次利用。

> **注意**：一个`DraweeHierarchy` 是不可以被多个 View 共用的！

如果要改变所要显示的图片可使用`setController` 或者 `setImageURI`。

### 运行时修改 DraweeHierarchy

DraweeHierarchy 的一些属性可以在运行时改变。

要改变这些属性，首先获取一个引用:

```java
GenericDraweeHierarchy hierarchy = mSimpleDraweeView.getHierarchy();
```

#### <a name='change_placeholder'></a>修改占位图

修改占位图为资源id:

```java
hierarchy.setPlaceholderImage(R.drawable.placeholderId);
```

你也可以将它修改为一个 [Drawable](http://developer.android.com/reference/android/graphics/drawable/Drawable.html):

```java
Drawable placeholderImage = ...;
hierarchy.setPlaceholderImage(placeholderImage);
```

其他图层也可以修改：

```java
Drawable failureImage = ...;
hierarchy.setFailureImage(failureImage, ScaleType.CENTER);
```

#### 改变图像的显示

修改[缩放类型](scaling.html):

```java
hierarchy.setActualImageScaleType(ScalingUtils.ScaleType.CENTER_INSIDE);
```

如果你选择缩放类型为 `focusCrop`，需要指定一个居中点:

```java
hierarchy.setActualImageFocusPoint(point);
```

你可以为图像添加一个 color filter:

```java
ColorFilter filter;
// 创建filter
hierarchy.setActualImageColorFilter(filter);
```

#### 圆角

除了圆角显示方式（原来为圆角的不能修改为圆圈，反之亦然），其他圆角相关的呈现参数, [具体参见这里](rounded-corners-and-circles.html) 是可以动态修改的。

如下: 获取DraweeHierarchy的圆角显示参数，修改圆角半径为10。

```java
RoundingParams roundingParams = hierarchy.getRoundingParams();
roundingParams.setCornersRadius(10);
hierarchy.setRoundingParams(roundingParams);
```
