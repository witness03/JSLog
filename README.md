# 头条统计：落地页转化目标监测 JSLog

## 一、背景介绍
目的：运用js的方式来标记落地页上的转化目标（广告主目标值，比如线索搜集为目标的广告主，则标记表单提交成功这个动作为转化目标），从而统计广告投放带来的转化率，帮助进行转化优化。

## 二、操作步骤
1. 头部JS代码添加：加入页面头部标记头条 js代码的应用。
2. 事件触发代码添加：添加到特定的动作上标记页面中实际发生的转化事件。

## 三、详细操作
### 1.头部JS代码
在HTML的头部加入如下代码：
```
<script type="text/javascript">
      (function() {
      	_tt_config = true;
        var ta = document.createElement('script'); ta.type = 'text/javascript'; ta.async = true;
        ta.src = document.location.protocol + '//' + 's0.pstatp.com/adstatic/resource/landing_log/dist/1.2.6/static/js/toutiao-tetris-analytics.js';
        var s = document.getElementsByTagName('script')[0]; s.parentNode.insertBefore(ta, s);
    })();
</script>
```
***
### 2.事件触发代码
>> ### 根据应用场景的不同，事件触发代码添加有两种方式：html中触发和JS中触发
> #### a. html方式触发
>> ##### 应用场景：用户点击某个组件一次就发送一次
>> ##### 使用方法：
>> ```
<div tt-data-click tt-data-url="XXX" tt-data-eventtype="XXX" tt-data-eventvalue="XXX" tt-data-options="XXX">
</div>
>>```
>> ##### 注意事项：
>>      1、您可以使用任意的DOM，而不仅仅是div
>>      2、tt-data-click为必填
>>      3、tt-data-url为非必填，主要用于识别您要监测的网页。该段代码可以不写，默认取当前页面的url。
>>      4、tt-data-eventtype为必填，目前可选为：phone、map、form
>>      5、tt-data-eventvalue为非必填，您可以填入以用于识别一个url中的不同的事件，例如您需要统计同一页面中的两个事件“抽奖”和“投票”，那么您可以分别将tt-data-eventvalue定义为lottery和poll
>>      6、tt-data-options为非必填
>>      7、非必填：非必填的代码段均表示可以不写
***
> #### b. JS方式触发
>> ##### 应用场景：某一事件成功了才会统计，比如申请信用卡，表单提交成功后才发送log，而不是每点击提交按钮一次就发送。
>> ##### 使用方法：(在合适的地方执行）
>> ```
var msg = {}; //必写
msg.page_url = 'XXX'; //非必填
msg.event_type = 'XXX';  //必填
msg.event_value = 'XXX'; //非必填，不需要特殊标记的时候该行代码可以不写
msg.target = document.getElementById('XXX'); //非必填，不可使用JQuery对象
_taq.push(msg)
>>```
>> ##### 注意事项：
>>      1、event_type为必填项，目前可选为：phone、map、form
>>      2、page_url为非必填，默认是当前页面的url
>>      3、event_value逻辑与HTML方式触发一样，参见上方'tt-data-eventvalue'
>>      4、target为非必填，不可使用JQuery对象
>>      5、非必填：非必填的代码段均表示可以不写

***

### 三、常见问题
#### Q1、HTML中使用了第一种方式进行触发，还需要使用在JS中触发的方式吗？
         不需要，两种方式分别对应不同的应用场景，第一种适用于点击，第二种适用于非点击，需要在JS中进行处理其他逻辑然后才发送的场景
#### Q2、tt-data-eventtype或event_type目前可以自定义吗？
         不能，目前只能使用已有的form、phone、map三种分类，之后会根据需要不断新增分类
#### Q3、如何确定是否添加成功
         如果是使用的第一种方式触发发送log，点击添加的DOM，看是否发送ajax请求，并返回‘success'
         如果是使用的第二种方式触发发送log，进入相应的逻辑看是否发送请求。（比如如果是提交表单注册成功后发送，则测试时成功注册时会发送）
