# 学习使用grid布局

**参考**: [CSS Grid 网格布局教程 阮一峰](http://www.ruanyifeng.com/blog/2019/03/grid-layout-tutorial.html)


### 它将网页划分成一个个网格，可以任意组合不同的网格


### 应用在容器上的熟悉：
- display ：grid | inline-grid （设为网格布局以后，容器子元素（项目）的float、display: inline-block、display: table-cell、vertical-align和column-*等设置都将失效）
- grid-template-columns  定义列宽
- grid-template-rows     定义行高
    ```css
    /*三行三列宽高都是100px*/
    .container {
      display: grid;
      grid-template-columns: 100px 100px 100px;
      grid-template-rows: 100px 100px 100px;
    }
    ```
    + fr 是一个关键字，类似表示相对宽度的单位
        ```css
        /*第一列150px,第三列是第二列的两倍宽*/
        .container {
          display: grid;
          grid-template-columns: 150px 1fr 2fr;
        }
        /*左侧定宽两栏布局*/
        .container {
          display: grid;
          grid-template-columns: 250px 1fr;
        }
        /*flex实现*/
        .container {
          display: flex;
          flex: 250px 1;
        }
        ```

- grid-template-areas  定义区域
    ```css
    /*定义好区域后子项目可以通过区域名选择布局位置*/
    .container {
      display: grid;
      grid-template-columns: 100px 100px 100px;
      grid-template-rows: 100px 100px 100px;
      grid-template-areas: 'a b c'
                           'd e f'
                           'g h i';
    }
    ```
- grid-auto-flow 定义项目排序方式
- justify-items属性设置单元格内容的水平位置（左中右），align-items属性设置单元格内容的垂直位置（上中下）
-justify-content属性是整个内容区域在容器里面的水平位置（左中右），align-content属性是整个内容区域的垂直位置（上中下）
-grid-auto-columns属性和grid-auto-rows属性用来设置浏览器自动生成多余的网格时的列宽和行高

### 定义在项目上的属性

- grid-column-start  左边框所在的垂直网格线
- grid-column-end  右边框所在的垂直网格线
- grid-row-start  上边框所在的水平网格线
- grid-row-end  下边框所在的水平网格线
- grid-area   指定项目放在哪一个区域
-justify-self属性设置单元格内容的水平位置（左中右），align-self属性设置单元格内容的垂直位置（上中下）

### 实现一个后台管理的布局

<iframe height="600" style="width: 100%;" scrolling="no" title="poNGKpV" src="https://codepen.io/ding_yuwen/embed/poNGKpV?height=306&theme-id=light&default-tab=html,result" frameborder="no" loading="lazy" allowtransparency="true" allowfullscreen="true">
  See the Pen <a href='https://codepen.io/ding_yuwen/pen/poNGKpV'>poNGKpV</a> by dingyuwen
  (<a href='https://codepen.io/ding_yuwen'>@ding_yuwen</a>) on <a href='https://codepen.io'>CodePen</a>.
</iframe>