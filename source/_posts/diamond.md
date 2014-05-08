title: 兼容所有浏览器的菱形
date: 2014-04-16 15:53:15
tags: [技术]
---

##介绍菱形的绘制法

    第一种是svg、vml

如图：

![图片](https://raw.githubusercontent.com/fangcao7618/fangcao7618.github.io/master/img/1.jpg)

```
<!doctype html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <title>菱形裁剪</title>
    <script type="text/javascript" charset="utf-8" src="js/raph.js"></script>
    <style type="text/css" charset="utf-8">
    #canvas {
        background-color: #F4F4F4;
        left: 0;
        position: absolute;
        top: 0;
    }
    #paper {
        left: 0;
        position: relative;
        top: 0;
    }
    h2 {
        text-align: center;
    }
    #vic1, #vic2, #vic3 {
        position:absolute;
        left:0;
        top:0;
    }
    shape, span {
        font-size: 16px;
        font-family:'Microsoft YaHei';
        color: #ffffff;
        font-style: oblique;
        display:block;
        height:100%;
        width:100%;
        text-align:center;
        background-color: red;
    }
    </style>
</head>

<body>
    <div id="canvas">
        <div id="paper">
            <div id="vic1"></div>
            <div id="vic2"></div>
            <div id="vic3"></div>
            <div id="vic4"></div>
            <div id="vic5"></div>
            <div id="vic6"></div>
            <div id="vic7"></div>
            <div id="vic8"></div>
            <div id="vic9"></div>
            <div id="vic10"></div>
            <div id="vic11"></div>
            <div id="vic12"></div>
            <div id="vic13"></div>
            <div id="vic14"></div>
            <div id="vic15"></div>
            <div id="vic16"></div>
            <div id="vic17"></div>
            <div id="vic18"></div>
            <div id="vic19"></div>
            <div id="vic20"></div>
            <div id="vic21"></div>
        </div>
    </div>
    <script type="text/javascript">
    (function(Raphael,wh) {
        var attr = {
            fill: "#fff",
            stroke: "#BCBCBC",
            "stroke-width": 1
        },
            attr2 = {
                fill: "#F8F8F8",
                stroke: "#BBBBBB",
                "stroke-width": 1
            },
            attr3 = {
                fill: "#795755",
                stroke: "#BBBBBB",
                "stroke-width": 1,
                "opacity": "0",
                "cursor": "pointer"
            },
            attr4 = {
                fill: "#fff",
                font: "40px Georgia",
                "opacity": "0",
                'text-anchor': 'start'
            },
            x = 30/146*wh, //距离横坐标点 这里也可以根据角度而计算
            y = 30/146*wh, //距离竖坐标点 这里也可以根据角度而计算
            width = wh,
            height = wh,
            xiejiao = parseInt(width * Math.sqrt(2)), //对角线
            plength = document.getElementById("paper").getElementsByTagName("div").length, //总个数
            itemCount = 9, //每行显示的个数
            row = plength % itemCount == 0 ? plength / itemCount : parseInt(plength / itemCount) + 1, //行数
            R = Raphael("paper", xiejiao * (itemCount / 2 + 0.5), xiejiao * (row + 0.5)), //绘制画布宽度
            aus = {}, //声明数组
            pstrs = attr,
            temp,
            H1 = 0,
            L1 = "",
            key = "",
            imagesL1 = 0,
            imagesH1 = 0,
            _index = 0;

        for (var i = 0; i < row; i++) {
            //console.log("第" + i + "行");
            for (var j = 1; j <= itemCount; j++) {
                _index = j + i * itemCount;
                if (_index > plength) {
                    break;
                }
                //console.log(j);
                if (j % 2 != 0) {
                    pstrs = attr;
                    L1 = i * xiejiao + "r45t-0";
                    H1 = j == 1 ? 0 : (j - 1) / 2 * xiejiao;
                    imagesH1 = (xiejiao - 40) / 2 + xiejiao * i;
                    imagesL1 = H1 + 30;

                    //console.log("奇数:" + j + ":" + H1 + "  " + L1 + "图:" +imagesL1+ ":"+ imagesH1);
                } else {
                    pstrs = attr2;
                    L1 = (i * xiejiao + xiejiao) + "r45t-" + width;
                    H1 = j / 2 * xiejiao;
                    imagesH1 = (0.5 * xiejiao + 30 - 40) * 2 + xiejiao * i;
                    imagesL1 = (j / 2 - 0.5) * xiejiao + 35;

                    //console.log("偶数:" + j + ":" + H1 + "  " + L1+ "图:" +imagesL1+ ":"+ imagesH1);
                }
                //绘制最底层的形状
                R.path(Raphael.format("M{0},{1}h{2}v{3}h{4}z", x, y, width, height, -width)).transform("t" + H1 + "," + L1 + ",0s1").attr(pstrs);
                //绘制内容图片
                R.image("images/big_1.jpg", imagesL1, imagesH1, 133, 40); //图片的位置及大小
                //绘制遮罩层
                temp = R.path(Raphael.format("M{0},{1}h{2}v{3}h{4}z", x, y, width, height, -width)).transform("t" + H1 + "," + L1 + ",0s1").attr(attr3);
                //绘制遮罩层上面的说明文字
                R.text(imagesL1 + 20, imagesH1 + 20, "60." + _index+"a").attr(attr4).data("i", _index+"a");

                key = "vic" + _index;
                aus[key] = temp;
            }
        }
        for (var state in aus) {
            aus[state].color = Raphael.getColor();
            (function(st, state) {
                st[0].style.cursor = "pointer";
                //console.log(st.next[0].childNodes[0].innerHTML.length*parseInt(st.next[0].style.fontSize)/2);
                st.next[0].onmouseover = function() {
                    st.animate({
                        "opacity": ".5"
                    });
                    st.next.animate({
                        "opacity": "1.0"
                    });
                };
                st.next[0].onmouseout = function() {
                    st.animate({
                        "opacity": "0"
                    });
                    st.next.animate({
                        "opacity": "0"
                    });
                };
                st[0].onmouseover = function() {
                    st.animate({
                        "opacity": ".5"
                    });
                    st.next.animate({
                        "opacity": "1.0"
                    });
                };
                st[0].onmouseout = function() {
                    st.animate({
                        "opacity": "0"
                    });
                    st.next.animate({
                        "opacity": "0"
                    });
                };
            })(aus[state], state);
        };
    })(Raphael,146);
    </script>
</body>

</html>

```