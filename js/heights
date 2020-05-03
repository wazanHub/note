最近用vue重构了个之前用jq写的h5小项目，用到了滚动条的相关的知识点，用了vue自然不用jq的方法，只能用js原生的属性，不熟。查了知识点，大多数讲的很多，但容器属性这块确实不少个，长得差不多一样<br><br>
**谁不常用谁能记得那么准，用得那么熟练?**<br>
这里引用下资料，总结下自己要的
****
#### offsetTop
* 元素到offsetParent顶部的距离（获取不带px）
* 距离元素最近的一个具有定位的祖宗元素（relative，absolute，fixed），若祖宗都不符合条件，offsetParent为body
* 只有元素show（渲染完成）才会计算入offsetTop，否则获取为0

#### offsetHeight 
* 可视高度(padding+conheight+border)

#### scrollTop
* overflow:scroll对高度进行了限制 (conheight+padding) 

#### clientHeight
* 没对高度进行限制(conheight+padding)
