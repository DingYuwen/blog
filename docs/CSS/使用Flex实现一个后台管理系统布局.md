# 学习使用Flex弹性布局

## 基本概念：
<img src="https://ftp.bmp.ovh/imgs/2021/03/ed2e02924982b839.png">

- 弹性容器(Flex container)
包含着弹性项目的父元素。通过设置 display 属性的值为 flex 或 inline-flex 来定义弹性容器。

- 弹性项目(Flex item)
弹性容器的每个子元素都称为弹性项目。弹性容器直接包含的文本将被包覆成匿名弹性单元。

- 轴(Axis)
每个弹性框布局包含两个轴。弹性项目沿其依次排列的那根轴称为主轴(main axis)。垂直于主轴的那根轴称为侧轴(cross axis)。

  + flex-direction 确立主轴。
  + justify-content 定义了在当前行上，弹性项目沿主轴如何排布。
  + align-items 定义了在当前行上，弹性项目沿侧轴默认如何排布。
  + align-self 定义了单个弹性项目在侧轴上应当如何对齐，这个定义会覆盖由 align-items 所确立的默认值。
- 方向(Direction)
弹性容器的主轴起点(main start)/主轴终点(main end)和侧轴起点(cross start)/侧轴终点(cross end)描述了弹性项目排布的起点与终点。它们具体取决于弹性容器的主轴与侧轴中，由 writing-mode 确立的方向（从左到右、从右到左，等等）。

- order 属性将元素与序号关联起来，以此决定哪些元素先出现。
flex-flow 属性是 flex-direction 和 flex-wrap 属性的简写，决定弹性项目如何排布。
- 行(Line)
根据 flex-wrap 属性，弹性项目可以排布在单个行或者多个行中。此属性控制侧轴的方向和新行排列的方向。

- 尺寸(Dimension)
根据弹性容器的主轴与侧轴，弹性项目的宽和高中，对应主轴的称为主轴尺寸(main size) ，对应侧轴的称为 侧轴尺寸(cross size)。

  + min-height 与 min-width 属性初始值将为 0。
  + lex 属性是 flex-grow、flex-shrink 和 flex-basis 属性的简写，描述弹性项目的整体的伸缩性。


### 注意点：
- 多栏布局模块的 column-* 属性对弹性项目无效。
- float 与 clear 对弹性项目无效。使用 float 将使元素的 display 属性计为block。
- vertical-align 对弹性项目的对齐无效。


### 实现一个后台管理系统布局
<iframe height="1000" style="width: 100%;" scrolling="no" title="qBqLZVO" src="https://codepen.io/ding_yuwen/embed/qBqLZVO?height=1000&theme-id=light&default-tab=html,result" frameborder="no" loading="lazy" allowtransparency="true" allowfullscreen="true">
  See the Pen <a href='https://codepen.io/ding_yuwen/pen/qBqLZVO'>qBqLZVO</a> by dingyuwen
  (<a href='https://codepen.io/ding_yuwen'>@ding_yuwen</a>) on <a href='https://codepen.io'>CodePen</a>.
</iframe>

```html
    <style>
        * { margin: 0; padding: 0; list-style: none; box-sizing: border-box;}
        :root {
            --blue: #346ee0;
            --indigo: #3F51B5;
            --purple: #6d61ea;
            --pink: #ff4bb2;
            --red: #ee5455;
            --orange: #fd7e14;
            --gray: #f3f3f3;
            --gray-dark: #222a3e;
            --white: #ffffff;
            --secondwhite: #dee2e6;
            --secondary: #747a80;
            --nav-width: 220px;
            --shadow:rgba(130,122,122,0.1)
        }
        body {background-color: var(--gray)}
        .wrap {display: flex; min-height: 100vh;}
        .nav {flex: 0 0 220px; background-color: var(--white); box-shadow: 0 0 17px 0 var(--shadow);}
        .main {display: flex; align-items: center; flex-direction: column;}
        footer {border-top:1px solid var(--secondwhite); width: 100%; flex: 0 0 60px;}
        header {position: fixed; top: 0; right: 0; z-index: 1001; left: 0;}
        .header{margin: 10px 10px 10px 230px; height:60px; border-radius: 5px; background-color: var(--white); box-shadow: 0 0 17px 0 var(--shadow); display:flex; align-items: center; justify-content: space-between;}
        .content {flex: 1; width: calc(100vw - 220px); padding: 80px 20px;}
        .row {display: flex; flex-wrap: wrap;}
        .row-item {position: relative; padding: 10px; width: 100%;}
        .row-4 {flex: 0 0 40%; max-width: 40%;}
        .row-6 {flex: 0 0 60%; max-width: 60%;}
        .card {position: relative; display: flex; flex-direction: column; min-height: 1px; background-color: var(--white);  min-width: 0;border-radius: 5px;box-shadow: 0 0 17px 0 var(--shadow);background-clip: border-box; border: 0 solid #e9edf3;}
        .card-body {flex: 1 1 auto; min-height: 1px;}
        .table-responsive {overflow-x: auto;}
        .table th, .table td {padding: 1rem; vertical-align: top;}
    </style>


    <body>
        <div class="wrap">
            <div class="nav">
                <h2>nav</h2>
            </div>
            <header id="page-topbar">
                <div class="header">
                    header
                </div>
            </header>
            <div class="main">
                <div class="content">
                    <div class="row">
                        <div class="row-item row-4">
                            <div class="card">
                                <h3>card</h3>
                                <div class="card-body">
                                    <div class="table-responsive">
                                        <table class="table table-borderless table-hover mb-0">
                                            <thead class="thead-light">
                                                <tr>
                                                    <th>No</th>
                                                    <th>Project Name</th>
                                                    <th>Client Name</th>
                                                    <th>Deadline</th>
                                                    <th>Income</th>
                                                    <th>Status</th>
                                                </tr>
                                            </thead>
                                            <tbody>
                                                <tr>
                                                    <th>1</th>
                                                    <td>App design and development</td>
                                                    <td>Arthur Powell</td>
                                                    <td>Sun, 08/10</td>
                                                    <td>$8,532</td>
                                                    <td class="text-warning">In Process</td>
                                                </tr>
                                                <tr>
                                                    <th>2</th>
                                                    <td>Coffee detail page - Main Page</td>
                                                    <td>Joan Lennox</td>
                                                    <td>Fri, 17/10</td>
                                                    <td>$5,350</td>
                                                    <td class="text-success">Done</td>
                                                </tr>
                                                <tr>
                                                    <th>3</th>
                                                    <td>Poster illustation design</td>
                                                    <td>Ronald Roesler</td>
                                                    <td>Tue, 13/08</td>
                                                    <td>$2,250</td>
                                                    <td class="text-danger">Hold</td>
                                                </tr>
                                            </tbody>
                                        </table>
                                    </div>
                                </div>
                            </div>
                        </div>
                        <div class="row-item row-6">
                            <div class="card">
                                <h3>card</h3>
                                <div class="card-body">
                                    <div class="table-responsive">
                                        <p>fsdfdfdf</p>
                                        <p>fsdfdfdf</p>
                                        <p>fsdfdfdf</p>
                                        <p>fsdfdfdf</p>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
                <footer class="footer">
                    <p>footer</p>
                </footer>
            </div>
        </div>
    </body>
```

