[PHP设计模式(二)：抽象类和接口](https://segmentfault.com/a/1190000004699158)
https://segmentfault.com/q/1010000008801822
[mysql5.7 安装后无法设置密码](https://segmentfault.com/q/1010000008801677)
```js
对于MySQL 5.7：在服务器的初始启动时，出现以下情况，假定服务器的数据目录为空：
服务器已初始化。
SSL证书和密钥文件在数据目录中生成。
该 validate_password插件安装并启用。
'root'@'localhost' 创建 超级用户帐户。超级用户的密码被设置并存储在错误日志文件中。要显示它，请使用以下命令：
shell> sudo grep 'temporary password' /var/log/mysqld.log
通过使用生成的临时密码登录并为超级用户帐户设置自定义密码，尽快更改root密码：
shell> mysql -uroot -p 
mysql> ALTER USER 'root'@'localhost' IDENTIFIED BY 'MyNewPass4!';
注意
默认情况下 安装MySQL的 validate_password插件。这将要求密码至少包含一个大写字母，一个小写字母，一个数字和一个特殊字符，并且总密码长度至少为8个字符。
```
定时跑数据，每次1000
```js
$lastKey = 'lastid';
$minId = \RedisFacade::get($lastKey);
        if (!$minId) {
            $minId = 0;
            \RedisFacade::set($lastKey, $minId);
        }
while (true) {
            $demands = user::where('id', '>', $minId)->where('user_id', 14)
                ->select('id', 'vid', 'user_id')
                ->orderBy('id')
                ->take($limit)
                ->get();

            $selectCount = count($demands);
            if (!$selectCount) {
                \RedisFacade::set($lastKey, $minId);
                break;
            }

            foreach ($demands as $value) {
            	// if (!($record = RedisFacade::get('demand:records:push:'. $value['id']))) {
                	\Queue::push(new DemandRecords($value['vid'], $value['baoli']['baoli_id'], $value['baoli']['secretkey']));
                	// RedisFacade::setEx('demand:records:push:'. $value['id'], 86400, $value['id']);
            	// }
            }

            if ($selectCount < $limit) {
                break;
            }

            $minId = $demands->last()->id;
            unset($demands);

            if ($usleep > 0) {
                usleep($usleep);
            }
        }

```



[清博指数的API接口](https://segmentfault.com/q/1010000008796281)
清博指数获取公众号文章API
[算法问题，2个数组，数组a保存1000W条手机号，数组b保存5000W条，找出两个数组相同的手机号](https://segmentfault.com/q/1010000008169527)
[假定有json数据多条记录，如何根据KEY的值返回一条记录](https://segmentfault.com/q/1010000008793933)
```js
s = """
[
  {
    "Name": "A1", 
    "No": "3111", 
    "createDate": "9999/12/31 00:00:00", 
    "lastUpdDate": "9999/12/31 00:00:00"
  }, 
  {
    "Name": "B2", 
    "No": "2222", 
    "createDate": "9999/12/31 00:00:00", 
    "lastUpdDate": "9999/12/31 00:00:00"
  }, 
  {
    "Name": "C3", 
    "No": "1444", 
    "createDate": "9999/12/31 00:00:00", 
    "lastUpdDate": "9999/12/31 00:00:00"
  }, 
  {
    "Name": "C4", 
    "No": "0542", 
    "createDate": "9999/12/31 00:00:00", 
    "lastUpdDate": "9999/12/31 00:00:00"
  }
]
"""
# code for python3

import json

def search(json_str, no):
    return [datum for datum in json.loads(s) if datum['No']==no]

datum = search(s, '0542')
print(datum)
```
[微信中，复制内容添加到剪贴板！](https://segmentfault.com/q/1010000008789082)
clipboard实际是调用 document.execCommand('copy'); 来达到复制的目的。

而对于微信内的浏览器嘛，你自己看移去端的兼容性吧，IOS下并不支持，总之支持的极少。
[document mouseup 事件](https://segmentfault.com/q/1010000008794799)
```js
 你每按下一次鼠标就加了一个mouseup事件，然后就越来越多越来越多。
可以在mouseup的回调里把mouseup事件绑定解除了。

或者这样,做个简单的判断

document.addEventListener('mousedown',function(){
    if(document.eventMouseup){
        return false
    }else{
        document.addEventListener('mouseup',(event)=>{
            console.log(1);
        },false);
        document.eventMouseup = true;
    }   
},false)

```
[如何判断浏览器是否支持localstorage](https://segmentfault.com/q/1010000008793630)
```js
typeof localStorage 和 if(window.localstorage) 的检测都太弱
假如我就故意设置 window.localStorage = {} 一样没有存储效果。

另外据说Safari隐私模式下 localStorage 依然有效但是无法存储，没验证过，有mac的同学可以验证一下我的道听途说。

window.localStorage && (window.localStorage.setItem('a', 123) , window.localStorage.getItem('a') == 123)
```
[Sql union 操作](https://segmentfault.com/q/1010000008793674)
```js
(select  du.day,du.apptoken ,du.version, du.channel,du.city,du.count,concat(apptoken, 

version,channel,city) as joinkey from day_new_users_count du where day='20170319') as dayUsers 

 union 

(select tu.day,tu.apptoken,tu.version,tu.channel,tu.city,tu.count,concat(apptoken,version,channel,city) as joinkey from total_users tu where day='20170318') as toUsers

select  du.day,du.apptoken ,du.version, du.channel,du.city,du.count,concat(apptoken, version,channel,city) as joinkey from day_new_users_count du where day='20170319'
 union 

select tu.day,tu.apptoken,tu.version,tu.channel,tu.city,tu.count,concat(apptoken,version,channel,city) as joinkey from total_users tu where day='20170318'

select * from (
select
du.day, du.apptoken , du.version, du.channel, du.city, du.count,
concat(apptoken, version,channel,city) as joinkey
from day_new_users_count du
where day='20170319'
) as dayUsers
union
select * from (
select
tu.day,  tu.apptoken, tu.version, tu.channel, tu.city, tu.count,
concat(apptoken,version,channel,city) as joinkey
from total_users tu
where day='20170318'
) as toUsers
```
[php 登录界面session问题](https://segmentfault.com/q/1010000008793648)
```js
$.ajax({
   url : 'functions/php/login.php?action=get_login_info',
   type : 'get',
   success : function(data){
     if( '' != data){
       //更新导航
       $(".user-menu").append(data);
     }
   }
});
functions/php/login.php 中：

if( isset($_GET['action']) && 'get_login_info' == $_GET['action'] ){
    if( isset($_SESSION['id']) ){
        //链接数据库
        $sql = "SELECT * FROM user WHERE email= '" . $_SESSION['id'] . "'";
        //查找用户信息
        
        //你的代码，输出登陆后的导航
    }
    die();
}
```
[]()


[前端图片压缩成base64文件上传问题](https://segmentfault.com/q/1010000008790807)
```js
var canvas = document.getElementById("canvas");

canvas.toBlob(function(blob) {
  var formData = new FormData();
  formData.append('img', blob, 'canvas.jpg');
  //然后把这个formData扔给ajax就好了。babababa
});https://www.noway.pub/p/94.html
```

[js从数组中取出来数组的一半让他们的和最接近整个数组的和的一半](https://segmentfault.com/q/1010000008789474)
https://www.zhihu.com/question/27896075
[浏览器的两个页面之间通信的问题](https://segmentfault.com/q/1010000008792568)
postMessage API
支持两个页面跨域；只能传递字符串数据；参考 window.open；

直接引用
适用于两个页面在同一域；可以传递对象数据（对象数据使用 instanceof 做类型判断时有坑）；参考 window.open；

WebSocket 服务器中转
需要页面都与服务器建立 WebSockets 连接；支持跨域；参考 WebSocket

localStorage 事件
要求两页面在同一域；数据可以通过 localStorage 传递；参考 localStorage 的 'storage' 事件；
[为什么递归里 return后方法会继续循环,用dd打印反倒是可以输出正确的数组形式?](https://segmentfault.com/q/1010000008755555)

```js
    public static function foreachComments($comments, $arr_id = array(0), $arr_comments = [])
    {
        $arr = $comments;
        $s_comments = &$arr;
//        //一个全局的变量 保存循环出来的数组,
//        //一个全局的ID变量数组,保存每次循环出来的数组的ID
        foreach ($comments as $key => $value) {
            $a = $value->father_id;
            //数组取值为空??!!
            $b = $arr_id[count($arr_id) - 1];
            if ($a == $b) {
                $arr_comments[count($arr_comments)] = $comments[$key];
                $arr_id[count($arr_id)] = $value->id;
                $s_comments = array_except($comments, $key);
                self::foreachComments($s_comments, $arr_id, $arr_comments);//这里也是在调用自身
            }

        }

        $arr_id = array_except($arr_id, count($arr_id) - 1);
        if (count($arr)==0) {
            dd($arr_comments);//在这里return后会继续往下执行...
        }
       self::foreachComments($s_comments, $arr_id, $arr_comments);//这里也是在调用自身
    }
      
    函数return 后会把执行权交给函数调用处. dd函数是终止程序并打印结果,所以你用DD当然可以打印出结果啊.
```
[做了一题JS面试题](https://segmentfault.com/q/1010000008703575)
```js
var o = [].reduce.call('cbaacfdeaebb',function(p,n){
          return p[n] = (p[n] || 0) + 1,p;
       },{}),
   s = Object.keys(o).reduce(function(p,n){
       return o[p] <= o[n] ? p : n;
   });
   console.log(s,o[s]); 
   
https://segmentfault.com/q/1010000008745355   
 LazyMan('Hank'); 输出
Hi Hank!

LazyMan('Hank').eat('dinner');输出
Hi Hank!
Eat dinner!

LazyMan('Hank').sleep(5).eat('dinner'); 输出：
Hi Hank!
//等待五秒
Eat dinner!

LazyMan('Hank').sleepFirst(5).eat('dinner');输出：
//等待五秒
Hi Hank!
Eat dinner!  
 function LazyMan(nick){
    var obj = {
        task : {
            list : [],
            add : function(fun, timer){
                timer = timer || 0;
                this.task.list.push({
                    fun : fun,
                    timer : timer
                });
                return this;
            },
            run : function(){
                if( this.task.list.length > 0 ){
                    setTimeout( (function(){
                        this.task.list.shift().fun.call(this);
                    }).bind(this), this.task.list[0].timer );
                }else{
                    this.echo("[Task]", "==========Stop==========");
                }
            }
        },
        echo : function( str , str2 ){
            console.log( str + ' ' + str2 );
            return this;
        },
        hello : function( nick ){
            return this.task.add.call(this, function(){
                this.echo('Hi', nick);
                this.task.run.call(this);
            });
        },
        eat : function( eat ){
            return this.task.add.call(this, function(){
                this.echo('Eat', eat);
                this.task.run.call(this);
            });
        },
        sleep : function( timer ){
            return this.task.add.call(this, function(){
                this.echo("[Timer( sleep )]", timer);
                this.task.run.call(this);
            }, timer * 1000);
        },
        sleepFirst : function( timer ){
            var fun = this.task.list[0].fun;
            this.task.list[0].fun = function(){
                setTimeout((function(){
                    this.echo("[Timer( sleepFirst) ]", timer);
                    fun.call(this);
                }).bind(this), timer * 1000);
            };
            return this;
        }
    };
    obj.echo("[Task]", "==========Start==========").hello(nick).task.run.call(obj);
    return obj;
};

LazyMan("A").sleepFirst(1).eat("abc").sleep(4).sleep(5).eat("A").eat("B").eat("C")  
```
[如何对10亿个QQ号进行去重？](https://segmentfault.com/q/1010000008798404)

[mysql将这过去24小时分成10等分，一条SQL查询出10条记录](https://segmentfault.com/q/1010000008098144)
[call_user_func()函数](https://segmentfault.com/q/1010000008797338)
```js
function test() {
    echo "hello test";
}
call_user_func(function(){
    test();
});
//或者以字符串形式传入函数名
call_user_func("test");

class Foo {
    public function bar() {
        echo "Hello bar!";
    }
}
$obj = new Foo();
call_user_func(array($obj, "bar"));

class Foo {
    public static function getInitializer() {
        return "hello";
    }
}

function hello() {
    echo "world!";
}

call_user_func(Foo::getInitializer());
```
[深入学习的前端知识](https://segmentfault.com/q/1010000008796151)
```js
function DecideToLeave(){
    if( Are_U_Sure_U_Are_Not_Gonna_Learn_What_U_Want(in_this_company) == true ){
        if(No_Financial_Dep_On_ThisJob){
            if( Can_U_Find_The_Target_Company(ur_current_background, company_situation) == true ){
                return true;
            } else {
                if( Will_Work_In_This_Company_Affect_Ur_Learning(ur_learning_plan) == true ) {
                    return true
                } else {
                    setTimeout( DecideToLeave, SOMETIME_LATER)
                    return undefined;
                }
            }
        } else {
            return false;
        }
    } else {
        setTimeout( DecideToLeave, SOMETIME_LATER)
        return undefined;
    }
}
《程序员的成长路线》

```
[在国内如何快速部署docker开放环境？](https://segmentfault.com/q/1010000008700675)
[windows下用WPF制作的nginx，php，mysql集成环境](https://github.com/salamander-mh/SalamanderWnmp)
https://segmentfault.com/q/1010000008795166 

[如何根据数据库中的某一个字段查询其在表中相同值所对应的另外一个字段的和？](https://segmentfault.com/q/1010000008798524)
select '藏品编号',sum('交易总额') from tablename where '创建时间'>'时间1' and '创建时间'<'时间2' group by '藏品编号';
[python else](https://segmentfault.com/q/1010000008798723)
```js
n = 17
for d in range(2,n):
    if n % d == 0:
        print(n, '是合数')
        break
else:
    print(n, '是素数')
    
    没有else的话我们应该加个bool变量，for循环后还加个if/else。用for/else的话简单多了。
```
[pandas读取中文的时候乱码 要如何解决](https://segmentfault.com/q/1010000008108689)
```js
#coding=utf-8

import pandas as pd
import sys

reload(sys)
sys.setdefaultencoding("utf-8")

df = pd.read_csv('week1.csv', encoding='utf-8', nrows=10)

print df
```
[python selenium](https://segmentfault.com/q/1010000008796785)
```js
from selenium import webdriver

browser = webdriver.Chrome()
browser.get('http://www.manhuatai.com/doupocangqiong/191.html')
img=browser.find_element_by_xpath('//img[@data-bd-imgshare-binded="1"]')
print img.get_attribute('src')
# 即打印出:
# http://mhpic.zymk.cn/comic/D%2F%E6%96%97%E7%A0%B4%E8%8B%8D%E7%A9%B9%2F191%E8%AF%9DSM%2F1.jpg-mht.middle
```