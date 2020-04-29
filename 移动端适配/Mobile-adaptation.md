# 移动端适配

## 视口
    像素  = 组成图片或电子屏幕的最小单位。  
 - 布局视口 （Layout Viewport） 
    -  PC端 一般指浏览器大小  
    - 移动端 默认的布局视口宽度大于移动设备的屏幕宽度，因为没有做适配的网页，如果禁止缩放，页面内容会显得非常小，不便于阅读。移动端的布局视口宽度由设备决定，浏览时会产生滚动条方便阅读。   
    ``` javascript
    document.documentElement.clientWidth/Height //可获取布局视口宽高
    ```
 - 视觉视口 （Visual Viewport）  
    当前设备用户可以看到的区域尺寸
    ``` javascript
    window.innerWidth/innerHeight //获取视觉视口宽高
    ```
 - 理想视口 （Ideal Viewport） 
    chrome浏览器调试的响应式模式的尺寸
    ``` javascript
    screen.width/height //获取理想视口宽高
    ```
页面缩放系数 = 视觉视口 / 理想视觉宽度  
## rem
   根据根元素的`font-size`做比例换算 (相对单位) 
   ```less
      html{
         font-size:14px;
         }
      1rem = 14px;
   ```
## rem目前最佳布局方案  
   > js+rem 方案，对于禁用js的用户通过`<noscript></noscript>`提示开启，实现响应式
   1. 获取布局视口宽度，通过js换算并设置根节点的`fontsize`  
      - 获取元素上的属性
      `window.getComputedStyle(element, [pseudoElt])` 第二个参数可选，指定一个要匹配的伪元素的字符串。必须对普通元素省略（或null）。   
      ```javascript
         function getStyle(obj, attr) {
            if (obj.currentStyle) {
                  return obj.currentStyle[attr];
            }
            else {
                  return getComputedStyle(obj, false)[attr];
            }
         }
      ```
      - 计算
      ```javascript
         // fs 浏览器根元素上的font-size大小
         document.documentElement.style.fontSize = document.documentElement.clientWidth / 10 / fs * 100 + "%";
      ```  
      PS：可在`windows`上绑定监听器，当`resize`变化时，重新设置根元素的`font-size`(需做防抖处理！！！)
   2. 在`body`设置样式`font-size`,修复页面字体大小  
   3. 样式文件中定义rem换算变量   
      ```less
         @unit: 750/10rem;
         .box{
            width: 50/@unit;
            height: 50/@unit;
            background-color: #000000;
         }
      ```