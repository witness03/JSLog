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
        ta.src = document.location.protocol + '//' + 's0.pstatp.com/adstatic/resource/landing_log/dist/1.2.1/static/js/toutiao-tetris-analytics.js';
        var s = document.getElementsByTagName('script')[0]; s.parentNode.insertBefore(ta, s);
    })();
</script>
```
***
### 2.事件触发代码
>> ### 根据应用场景的不同，事件触发代码添加有两种方式：html中触发和JS中触发
> #### a.html中触发
>> ##### 应用场景：您想要用户每点击一次就发送一次
>> ##### 使用方法：
>> ```
<div tt-data-click="click" tt-data-url="url" tt-data-eventtype="phone" tt-data-eventvalue="10086" tt-data-options="">
</div>
>>```
>> ##### 注意事项：
>>      1、您可以使用任意的DOM，而不仅仅是div
>>      2、tt-data-click为必填
>>      3、tt-data-eventtype为必填，目前可选为：phone、map、form
>>      4、tt-data-url为非必填，主要用于识别您监测的网页，不填会默认为当前url
>>      5、tt-data-eventvalue为非必填，您可以填入以用于识别一个url中的不同的事件，例如您需要统计同一页面中的两个事件“抽奖”和“投票”，那么您可以分别将tt-data-eventvalue定义为lottery和poll
>>      6、tt-data-options为非必填
***
> #### b.JS中触发
>> ##### 应用场景：某一事件成功了才会统计，比如申请信用卡，成功了之后才会发送log，而不是每点击一次就发送。这样，您就可以在JS中进行触发
>> ##### 使用方法：(在合适的地方执行）
>> ```
var msg = {};
msg.page_url = 'ur url';
msg.event_type = 'phone';  //必填
msg.event_value = '10086'; //非必填
msg.target = document.getElementById('xx'); //不可使用JQuery对象
_taq.push(msg)
>>```
>> ##### 注意事项：
>>      1、event_type为必填项，目前可选为：phone、map、form
>>      2、page_url非必填，默认是当前页面的url
>>      3、event_value与在HTML中触发一样
>>      4、target非必填，不可使用JQuery对象

***

### 三、常见问题
#### Q1、HTML中使用了第一种方式进行触发，还需要使用在JS中触发的方式吗？
         不需要，两种方式分别对应不同的应用场景，第一种适用于点击，第二种适用于非点击，需要在JS中进行处理其他逻辑然后才发送的场景
#### Q2、event_type或tt-data-eventtype可以随意定义吗？
         不能，这些只能使用已有的form、phone、map，后续根据需求不断新增
