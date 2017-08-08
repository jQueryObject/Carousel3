## 横向滑动式轮播图
效果如下：
![](img.gif)

all code:
```
<!DOCTYPE html>
<html>
<head lang="en">
    <meta charset="UTF-8">
    <title></title>
    <style>
        *{
            margin: 0;
            padding: 0;
        }
        html,body{
            width: 100%;
            height: 100%;
            overflow: hidden;
        }
        #box{
            width: 375px;
            height: 600px;
            border: 10px solid #000;
            margin: 0 auto;
            overflow: hidden;
            position: relative;
        }
        ul{
            width: 5000px;
            height: 600px;
            overflow: hidden;
            position: absolute;
        }
        ul li{
            width: 375px;
            height: 600px;
            float: left;
        }
        ul li img{
            width: 100%;
            height: 100%;
        }
        .left,.right{
            width: 30px;
            height: 60px;
            text-align: center;
            font: 30px/2 simsun;
            color: white;
            background: rgba(0,0,0,.5);
            position: absolute;
            z-index: 10;
            top: 50%;
            margin-top: -30px;
        }
        .left{
            left: 10px;
            cursor:pointer;
        }
        .right{
            right: 10px;
            cursor:pointer;
        }
        .disc{
            position: absolute;
            z-index: 10;
            bottom: 10px;
            left: 0;
            right: 0;
            height: 40px;
            text-align: center;
        }
        .disc span{
            display: inline-block;
            width: 20px;
            height: 20px;
            border-radius: 50%;
            background: gray;
            margin: 5px;
        }
        .disc .active{
            background: red;
        }
    </style>
</head>
<body>
<div id="box">
    <ul></ul>
    <div class="left">&lt;</div>
    <div class="right">&gt;</div>
    <div class="disc">
    </div>
</div>
</body>
</html>
<script src="https://cdn.bootcss.com/jquery/2.0.0/jquery.min.js"></script>
<script type="text/javascript">
    var i = 0;
    function getImg(){
        $.ajax({
            type:"get",
            url:"http://gank.io/api/data/%E7%A6%8F%E5%88%A9/10/1",
            async:true,
            success:function(res){
//                console.log(res.results)
                var data = res.results;
                var html = "";
                $.each(data, function(i,val) {
                    html += '<li><img src="'+ val.url +'" alt="'+ i +'" /></li>';
                    $(".disc").append("<span/>")
                });
                html += '<li><img src="'+ data[0].url +'" alt="1" /></li>';
                $(".disc span").eq(0).addClass("active");
                $("ul").prepend(html);
//                console.log(html)
            }
        });
    }
    var w = $("#box").width();
    function change(){
        var length = $("#box li").size();
        i++;
        if (i == length) {
            $('ul').css({left:0});
            i = 1;
        }
        if (i==length-1) {
            $(".disc span").eq(0).addClass("active")
                    .siblings().removeClass("active")
        }
        $(".disc span").eq(i).addClass("active")
                .siblings().removeClass("active");
        $('ul').stop().animate({left:-w*i})
    }
    getImg();
    $("#box").on("click",".right",function(){
        change();
    });
    $("#box").on("click",".left",function(){
        var length = $("#box li").size();
        i--;
        console.log(length,i);
        if (i == -1) {
            i = length-2;
            $('ul').css({left:-w*(length-1)})
        }
        $('ul').stop().animate({left:-w*i});
        $(".disc span").eq(i).addClass("active")
                .siblings().removeClass("active")
    });
    $(".disc").on("click","span",function(){
        i = $(this).index() -1;
        change();
    });
    var timer = setInterval(change,2000);
    $("#box").hover(function(){
        clearInterval(timer);
    },function(){
        timer = setInterval(change,2000);
    })
</script>
```

