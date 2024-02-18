<%* 
let weatherUrl = 'https://restapi.amap.com/v3/weather/weatherInfo'
let key = '3ec42f5d8d394d6a8bc23b348a257ee2'

let tencentIpUrl = 'https://apis.map.qq.com/ws/location/v1/ip';
let tencentKey = 'WX5BZ-POX67-67VXY-HZI4N-4NTVO-6OFMW'

let ipResult = eval("(" + await request({url: tencentIpUrl + `?key=${tencentKey}`, method: "GET"}) + ")").result
console.log("ipResult: ", ipResult)

let adcode = ipResult.ad_info.adcode
console.log("adcode: " + adcode)

let 位置 = ''
let 天气 = ''
let 温度 = ''
let 风向 = ''
await fetch(weatherUrl + `?key=${key}&city=${adcode}&extensions=all`)
.then(res => res.json())
.then((data) => {
	let info = data.forecasts[0]
	
	console.log("info:" + info)
	
	位置 = info.province + '-' + info.city
	天气 = '🌅' + info.casts[0].dayweather + ' / 🌃' + info.casts[0].nightweather
	温度 = '🌅' + info.casts[0].daytemp_float + '℃' + '/ 🌃' + info.casts[0].nighttemp_float + '℃'
})
-%>
---
🌻日期🌻: <% tp.file.creation_date("YYYY MM DD HH:mm:ss") %>
🌙星期🌙: <% tp.file.creation_date("dddd") %>
⌚️时间⌚️: <% tp.file.creation_date("HH:mm:ss") %>
🌍位置🌍: <% 位置 %>
☁️天气☁️: <% 天气 %>
🌡️温度🌡️: <% 温度 %>
---
<%*
let 一言 = ""
let 来源 = ""
let 作者 = ""

await fetch('https://v1.hitokoto.cn/?c=d&c=h&c=i&c=j')
.then(response => response.json())
.then(data => { 
	一言 = data.hitokoto
	来源 = data.from
	作者 = data.from_who === null ? '佚名' : data.from_who
})
-%>
>[!quote] 一言
 <% 一言 %>  —— 《<% 来源 %>》 · <% 作者 %>
## 项目列表
%%项目的今日快照%%
<% PeriodicPARA.Project.snapshot() %>
## 日常记录
%%你的记录%%
- %%你的记录%%
## 习惯打卡
%%打卡不会被统计为任务%%
- [ ] 起床一杯水
- [ ] 早餐
- 喝水
	- [ ] +1
	- [ ] +1
	- [ ] +1
	- [ ] +1
	- [ ] +1
	- [ ] +1
- [ ] 多邻国英语
- [ ] 项目耗时统计
<%* let weekDay = moment(tp.file.title).weekday(); console.log(weekDay); if (weekDay + 1 <= 5) { -%>
- 休息一下
	- [ ] +1
	- [ ] +1
	- [ ] +1
- 站立式办公
	- [ ] +1
	- [ ] +1
	- [ ] +1
	- [ ] +1
	<%* } else { -%>
- [ ] 记账 
<%* } -%>
## 精力分配
%%今日的项目列表，根据耗时情况，自动统计项目耗时占比%%
```PeriodicPARA
ProjectListByTime
```
## 今日完成
%%今日完成的任务列表，从所有的笔记中提取%%


---
<< [[<% tp.date.now("YYYY-MM-DD", -1) %>]] | [[<% tp.date.now("YYYY-MM-DD", 1) %>]] >>