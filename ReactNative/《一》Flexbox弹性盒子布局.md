Flexbox弹性布局是React Native中的布局，功能丰富，能够满足页面大量的排版需求。RN的FlexBox和Html5的差别不大，原理基本相同。


它拥有三个属性 **flexDirection**、**justifyContent**、**alignItems**

### flexDirection
>flexDirection是确定子元素的主轴方向，是沿着水平轴还是垂直轴排列，默认值是column。

有四个属性值:
- row：水平从左往右
- row-reverse：水平从右往左
- column：垂直从上往下
- column-reverse：垂直从下往上。

```javascript
import React, { Component } from 'react';
import { AppRegistry, View } from 'react-native';

export default class FlexDirectionBasics extends Component {
  render() {
    return (
      // 尝试把`flexDirection`改为`column`看看
      <View style={{flex: 1, flexDirection: 'row'}}>
        <View style={{width: 50, height: 50, backgroundColor: 'red'}} />
        <View style={{width: 50, height: 50, backgroundColor: 'green'}} />
        <View style={{width: 50, height: 50, backgroundColor: 'blue'}} />
      </View>
    );
  }
};
```

### justifyContent
>justifyContent是确定子元素在主轴上的对齐方式

有下面几个属性值：
- flex-start：沿着主轴确定的方向，起始点对齐排列（默认值）
- center：沿着主轴确定的方向，居中对齐排列
- flex-end：沿着主轴确定的方向，终点对齐排列
- space-around：沿着主轴确定的方向，元素均分排列，每个元素周围分配相同的空间
- space-between：沿着主轴确定的方向，元素均分排列，两端对齐
- space-evenly：沿着主轴确定的方向，元素均分排列，每个元素之间的间隔相等


```javascript
<View style={{
    flex: 1,
    flexDirection: 'row',
    justifyContent: 'space-evenly',
}}>
    <View style={{width: 50, height: 50, backgroundColor: 'red'}}/>
    <View style={{width: 50, height: 50, backgroundColor: 'green'}}/>
    <View style={{width: 50, height: 50, backgroundColor: 'blue'}}/>
</View>
```

### alignItems
>alignItems是与主轴垂直的轴的对齐方式

有下面几个属性值：
- flex-start：和垂直轴方向，起点对齐排列
- flex-end：和垂直轴方向，终点对齐排列
- center：和垂直轴方向，居中对齐排列
- baseline：和垂直轴方向，基线对齐
- stretch：当元素没有设置（宽）高度，或者（宽）高度为auto，会撑满（默认值）

```javascript
<View style={{
    flex: 1,
    flexDirection: 'row',
    justifyContent: 'flex-start',
    alignItems: "stretch"
}}>
    <View style={{width: 50, height: 200, backgroundColor: 'red'}}/>
    <View style={{width: 50, height: 200, backgroundColor: 'green'}}/>
    <View style={{width: 50, height: 200, backgroundColor: 'blue'}}/>
</View>
```

说了这麽多，这不是LinearLayout和RelativeLayout的结合体吗
