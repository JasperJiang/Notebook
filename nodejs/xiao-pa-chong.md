# 小爬虫

```javascript
var http = require('http');
var url = 'http://www.imooc.com/learn/348';

http.get(url,function(res){
    var html ='';
    res.on('data',function(data){
        html += data;
    });
    res.on('end',function(){
        console.log(html);
    }).on('error',function(){
        console.log('获取课程出错！')
    })
})
```



cheerio 现在npm里安装cheerio,命令：npm install cheerio   用法：语言用法跟jQuery完全一样。 2.先分析出自己想拿到什么格式的数据 ，然后再一步一步拿到并组装数据--此为第一部分代码  
例子：

```javascript
var cheerio = require('cheerio')
var $ = cheerio.load('<h2 class="title">Hello world</h2>')
$('h2.title').text('Hello there!')
$('h2').addClass('welcome')
$.html()
```



