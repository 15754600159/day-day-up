---
title: CSS note
date: 2018-06-03
categories: "CSS"
tags:
     - CSS
---
日常积累的CSS笔记！

1. 用rem，JS动态改变html的font-size可以屏幕自适应
2. CSS3的属性pointer-events能控制JS事件的触发：
    ```
    pointer-events：none //禁止所有事件
    pointer-events：all  //启用所有事件监听
    ```
3. 元素垂直居中：

    - 父元素高度确定的单行文本设置  height = line-height
    - inline-block
        ```
        display: inline-block;
        vertical-align: middle;
        ```
    - absolute定位：
        ```
        // 方法1：子元素
        position: relative; /*脱离文档流*/
        top: 50%; /*偏移*/
        transform: translateY(-50%);
        //方法2：
        .c1 { // 父
            height: 100px;
            width: 100px;
            background: #f3c8c8;
            position: relative;
        }
        .c2 { //子
            height: 50px;
            width: 50px;
            background-color: yellow;
            position: absolute;
            top: 0;
            bottom: 0;
            display: inline-block;
            margin: auto;
        }
        ```
    - [flex教程](http://www.ruanyifeng.com/blog/2015/07/flex-grammar.html)

        ```
        //父元素
        display: flex;
        align-items: center; /*定义body的元素垂直居中*/
        justify-content: center; /*定义body的里的元素水平居中*/
        ```
    - 父容器设置为display:table ，然后将子元素也就是要垂直居中显示的元素设置为 display:table-cell （不推荐使用）

4. 元素水平居中：

    - 行内元素：设置  text-align:center
    - 定宽块状元素：设置左右margin值为auto
    - transform
        ```
        // 方法1：子元素
        position: relative; /*脱离文档流*/
        left: 50%; /*偏移*/
        transform: translateX(-50%);
        // 方法2：
        .c1 { // 父
            height: 100px;
            width: 100px;
            background: #f3c8c8;
            position: relative;
        }
        .c2 { // 子
            height: 50px;
            width: 50px;
            background-color: yellow;
            position: absolute;
            left: 0;
            right: 0;
            display: inline-block;
            margin: auto;
        }
        ```
    - flex布局

5. 清除浮动：
    - 父元素：overflow：auto;
    - 父元素内部最后加<br style="clear:both">
6. 元素设置为display:inline-block;之后会往下移：那是因为display:inline-block成了内联，inline box有一个叫做baseline的东西，想要更改很简单只要vertical-align: top;和vertical-align: bottom;
7. div剩余高度自动填充：
    - position:absolute
        ```
        #nav {
            background-color: #85d989;
            width: 100%;
            height: 50px;
        }
        #content {
            background-color: #cc85d9;
            width: 100%;
            position: absolute;
            top: 50px;
            bottom: 0px;
            left: 0px;
        }
        ```
    - 也可利用CSS的calc方法实现

        ```
        height: calc(100% - 3.4986rem);
        ```
8. CSS超出宽度的文本用'...'

    ```
    <!---- 正常 >
    .div {
        max-width: 100px;
        overflow: hidden;
        text-overflow: ellipsis; //裁剪文本 用...
        white-space: nowrap; //不换行
    }
    <!---- 表格 >
    table{

    　　table-layout: fixed;

    }
    td{

    　　white-space: nowrap;
    　　overflow: hidden;
    　　text-overflow: ellipsis;

    }
    ```
9. CSS 设置table下tbody滚动条
    ```
    table tbody {

        display:block;

        height:195px;

        overflow-y:scroll;

    }

    table thead, tbody tr {

        display:table;

        width:100%;

        table-layout:fixed;

    }
    ```
10. pointer-events: none; // 解决元素被覆盖，元素点不上的问题
11. 滚动视差：利用background-attachment: scroll/local/fixed; 实现; (滚动视差？CSS不在话下)
12. 三栏布局
    {% note info no-icon %}
    *参考：[三栏布局](https://blog.csdn.net/mrchengzp/article/details/78573208)*
    {% endnote %}
    - float布局；
    - position: absolute;
    - display: table;
    - display: flex;
    - display: grid;
13. CSS层叠顺序：(层叠次序, 堆叠顺序, Stacking Order) 描述的是元素在同一个层叠上下文中的顺序规则，从层叠的底部开始，共有七种层叠顺序：
    - 背景和边框：形成层叠上下文的元素的背景和边框。
    - 负z-index值：层叠上下文内有着负z-index值的定位子元素，负的越大层叠等级越低；
    - 块级盒：文档流中块级、非定位子元素；
    - 浮动盒：非定位浮动元素；
    - 行内盒：文档流中行内、非定位子元素；
    - z-index: 0：z-index为0或auto的定位元素， 这些元素形成了新的层叠上下文；
    - 正z-index值：z-index 为正的定位元素，正的越大层叠等级越高；
14. flex布局(踩坑)：
    - **问题描述**：flex-grow相同，但是元素宽度不是均分？
    **原因分析**：flex-grow是分配flex容器除内容外剩余空间的比例，并不是整个容器的比例[捂脸]，所以会出现子元素flex-grow都是1，但是实际宽度不同，因为子元素本身内容所占用的宽度不同；
    **解决方案**：所有flex-grow的子元素加上flex-basis: 0%;就是完全等比分布了，这个属性值会让父级主轴在计算剩余空间时忽略子元素的本身宽度，从而实现等比分配。简单写法就是直接定义flex: 1;
    **拓展**：
        ```CSS
        当 flex 取值为一个非负数字，则该数字为 flex-grow 值，flex-shrink 取 1，flex-basis 取 0%
        .item {flex: 1;}
        .item {
            flex-grow: 1;
            flex-shrink: 1;
            flex-basis: 0%;
        }

        .item {flex: none;}
        .item {
            flex-grow: 0;
            flex-shrink: 0;
            flex-basis: auto;
        }

        .item {flex: auto;}
        .item {
            flex-grow: 1;
            flex-shrink: 1;
            flex-basis: auto;
        }
        ```

    - **问题描述**：flex布局的子元素，宽度被其下的子元素撑大！
    **原因分析**：
        - 如果没有设置flex-basis属性，那么flex-basis的大小就是项目的width属性的大小
        - 如果没有设置width属性，那么flex-basis的大小就是项目内容(content)的大小