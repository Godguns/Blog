### align-items   
它可能取5个值。具体的对齐方式与交叉轴的方向有关，下面假设交叉轴从上到下。   
flex-start：交叉轴的起点对齐。
flex-end：交叉轴的终点对齐。
center：交叉轴的中点对齐。
baseline: 项目的第一行文字的基线对齐。
stretch（默认值）：如果项目未设置高度或设为auto，将占满整个容器的高度。   
这里有一个尝试的网站可以预览每个属性的效果   
https://www.runoob.com/try/playit.php?f=playcss_align-items&preval=stretch      
### align-self属性   
align-self属性允许单个项目有与其他项目不一样的对齐方式，可覆盖align-items属性。默认值为auto，表示继承父元素的align-items属性，如果没有父元素，则等同于stretch。   
```javascript   

.item {
  align-self: auto | flex-start | flex-end | center | baseline | stretch;
}   
```   

