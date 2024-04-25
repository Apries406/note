	## 盒子模型
![[Pasted image 20240312221945.png]]

>**盒子模型 = 外边距 + 内边距 + 边框 + 内容**

### border-sizing属性

- `border-box`：元素设定的`width`和`height`决定了元素的边框盒。也就是说，为元素指定的任何内边距和边框都将在已经设定的宽度和高度内进行绘制。通过已经规定了的`width`和`height`值减去`padding`和`margin`才能等到`content`部分的宽高，也被称为**IE盒子模型**
- `content-box`：**默认值**，`width`和`height`属性分别应用到元素的内容框。在宽度和高度之外绘制元素的`margin`、`border`、`padding`，称为**标准盒子模型**
- `inherit`：规定应从父元素继承

### border

```css
#border {
	/* 简写 */
	border: 1px solid #eee;
	/* 四个方向单独 */
	border-top: 1px solid #eee;
	border-bottom: 1px solid #eee;
	border-left: 1px solid #eee;
	border-right: 1px solid #eee;
}
```

### padding

```css
#padding {
	/* 四个方向简写 */
	padding: 10px;
	/* 上下 左右 简写*/
	padding: 10px 10px;
	/* 上右下左(顺时针) 简写 */
	padding: 10px 10px 10px 10px;
	/* 上右下左单独写 */
	padding-top: 10px;
	padding-right: 10px;
	padding-bottom: 10px;
	padding-left: 10px;
}
```

### margin

```css
#margin {
	/* 四个方向简写 */
	margin: 10px;
	/* 上下 左右 简写 */
	margin: 10px 10px;
	/* 上右下左 四方向简写*/
	margin: 10px 10px 10px 10px;
	/* 上右下左 四方向单独写*/
	margin-top: 10px;
	margin-right: 10px;
	margin-bottom: 10px;
	margin-left: 10px;
} 
```

