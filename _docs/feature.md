---
title: 功能介绍
category: Nirvana
order: 4
---

### 变量 Variable

<details>
  <summary>全局变量</summary>
全局变量（Global variables）的作用域是在整个工作空间,作为系统默认的变量存在。  
</details>
<details>
  <summary>环境变量</summary>
环境变量（Environment variables）的作用域是用例执行时所选择的环境内，如果一个key即存在全局变量中，又存在环境变量中，优先使用环境变量的值。
  <p>
    <em>环境，在实际测试中会有多套环境，包括测试环境、预生产环境、或者针对不同版本的环境，每个环境对应的一些变量如请求地址、用户信息和中间件地址等都不相同，为了避免每测试一个环境都要手动修改相关数据，引入环境概念。
    </em>
  </p>
</details>
<details>
  <summary>引用变量</summary>
通过特殊符号$引用变量，例如$Variables
</details> 

  
### 脚本 Scripts
<details>
  <summary>概念</summary>

脚本是一种灵活的，强大的辅助接口请求的方式，脚本分为：预定义脚本和自定义脚步。 

<li>预定义脚本：提前内置写好的脚本，我们可以把一些普遍通用的功能写成预定义脚本放在那里，方便大家使用。</li>
<li>自定义脚本：用户自己写的脚本，根据用户自己的实际情况编写脚本，然后上传到服务器，然后进行调用。</li>

</details>
<details>
  <summary>引用脚本</summary>
引用脚本的方法：${get_message_center_token()}，通过此方式平台会去执行get_message_center_token()这个函数。
</details>  

 
### 响应断言 Assert
  Nirvana 可以解析多层嵌套的json数据，从中抽取指定的信息，将“期望值”与“实际值”通过“匹配规则”进行比对，判断接口执行是否成功。
<details>
  <summary>解析响应</summary>
<li>  默认提供:</li>  
  <table>
    <thead>
      <tr>
        <th>Key</th>
        <th>描述</th>
      </tr>
     </thead>
     <tbody>
       <tr>
         <td>content</td>
         <td>响应体全部，json格式多级content.person.name.first_name</td>
       </tr>
       <tr>
         <td>status_code</td>
         <td>响应状态码</td>
       </tr>
       <tr>
         <td>elapsed</td>
         <td>响应时间days, seconds, microseconds, total_seconds</td>
       </tr>
       <tr>
         <td>headers</td>
         <td>响应头headers.content-type</td>
       </tr>
       <tr>
         <td>cookies</td>
         <td>cookies</td>
       </tr>
      </tbody>
   </table>
 <li>  通过jsonpath解析:</li> 
   <table>
    <thead>
      <tr>
        <th>JSONPath</th>
        <th>描述</th>
      </tr>
     </thead>
     <tbody>
       <tr>
         <td>$</td>
         <td>根节点，用于表示一个json数据，可以是数组或对象</td>
       </tr>
       <tr>
         <td>@</td>
         <td>当前节点对象</td>
       </tr>
       <tr>
         <td>.or[]</td>
         <td>取子节点</td>
       </tr>
       <tr>
         <td>..</td>
         <td>不管位置，选择所有符合条件的条件</td>
       </tr>
       <tr>
         <td>*</td>
         <td>匹配所有元素节点</td>
       </tr>
       <tr>
         <td>[]</td>
         <td>迭代器标示（可以在里边做简单的迭代操作，如数组下标，根据内容选值等</td>
       </tr>
       <tr>
         <td>[,]</td>
         <td>支持迭代器中做多选</td>
       </tr>
       <tr>
         <td>?()</td>
         <td>支持过滤操作</td>
       </tr>
       <tr>
         <td>()</td>
         <td>支持表达式计算</td>
       </tr>
      </tbody>
   </table>
   <li>  支持正则表达式</li>
</details>


<details>
  <summary>匹配规则 Match Rules</summary>
<table>
    <thead>
      <tr>
        <th>规则</th>
        <th>描述</th>
      </tr>
     </thead>
     <tbody>
       <tr>
         <td>equals</td>
         <td>判断实际结果和期望结果是否相等</td>
       </tr>
       <tr>
         <td>less_than</td>
         <td>判断实际结果小于期望结果</td>
       </tr>
       <tr>
         <td>less_than_or_equals</td>
         <td>判断实际结果小于等于期望结果</td>
       </tr>
       <tr>
         <td>greater_than</td>
         <td>判断实际结果大于期望结果</td>
       </tr>
       <tr>
         <td>greater_than_or_equals</td>
         <td>判断实际结果大于等于期望结果</td>
       </tr>
       <tr>
         <td>not_equals</td>
         <td>判断实际结果和期望结果不相等</td>
       </tr>
       <tr>
         <td>string_equals</td>
         <td>判断转字符串后，实际结果和期望结果是否相等</td>
       </tr>
       <tr>
         <td>length_equals</td>
         <td>判断长度（字符串、列表、字典）</td>
       </tr>
      </tbody>
   </table>
</details>

<details>
  <summary>示例</summary>
  <pre><code>  
[
  {
    "comment": "DMP测试报告",
    "create_time": "2020-01-19T11:41:37Z",
    "id": 1,
    "key": "ed02f420-63dd-4a45-8816-28c979795d5f",
    "name": "DX-DMP-冲刺看板",
    "type": null,
    "update_time": null,
    "url": "https://qyapi.weixin.qq.com/cgi-bin/webhook/send?key=ed02f420-63dd-4a45-8816-28c979795d5f"
  },
  {
    "comment": "NDX-Quality",
    "create_time": "2020-01-20T19:54:22Z",
    "id": 9,
    "key": "074e0adc-40a5-478a-a7f3-5fbc38c6bc2b",
    "name": "NDX-update",
    "type": null,
    "update_time": "2020-02-03T17:06:03Z",
    "url": "https://qyapi.weixin.qq.com/cgi-bin/webhook/send?key=074e0adc-40a5-478a-a7f3-5fbc38c6bc2b"
  }
]

Nirvana设置响应断言条件时输入Key为$[0].comment，解析结果为[ “DMP测试报告” ]
</code></pre>
</details>  


  
### 任务 Task

测试任务就是把指定的测试用例放在一起，作为一个集合。当用户执行某个测试任务的时候，也就是执行了包含在测试任务中的测试用例。
