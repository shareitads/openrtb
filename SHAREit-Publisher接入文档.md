# SHAREit-Publisher接入文档

目录

1 概述

2 接入步骤

3 实时竞价

3.1 数据传输

3.2 HTTP请求

3.3 HTTP响应

3.4 展示统计和计费

4 实时竞价RTB接口参数说明

4.1 Bid Request

4.1.1 imp object

4.1.2 banner object

4.1.3 native object

4.1.4 native request object

4.1.5 asset request object

4.1.6 title object

4.1.7 img object

4.1.8 video object

4.1.9 data object

4.1.10 app object

4.1.11 publisher object

4.1.12 device object

4.1.13 geo object

4.1.14 user object

4.1.15 data object

4.1.16 segment object

4.2 Bid Response

4.2.1seatbid object

4.2.2 bid object

4.2.3 native广告形式的返回说明

4.2.4 Assets object

4.2.5 title object

4.2.6 Img object

4.2.7 data object

4.2.8 video object

4.2.9 link object

5 RTB接口枚举值列表

5.1 IAB category枚举值列表

5.2 banner广告类型枚举值

5.3 creative属性枚举值列表

5.4 API框架枚举值

5.5 广告位置属性枚举值列表

5.6 视频竞价响应协议枚举值列表

5.7 设备类型枚举值列表

5.8 网络连接类型枚举值列表

5.9 IQG 媒体评级枚举值列表

5.10 Context Type IDs

5.11 Context Sub Type IDs

5.12 Data Asset Types

5.13 Location Type

5.14 IP Location Services

RTB Request示例

1a) native icon image request示例

1b) native large image request示例

1c) native video request示例

1d) banner request示例

RTB Response 示例

2a) native icon image response示例

2b) native large image response示例

2c) native video response示例

2d) banner response示例







# **1 概述**

本文作为SHAREit Midas系统对接Publisher的指导文档。文档描述了基于openRTB进行网络请求交互的接口协议。

 

# **2 接入步骤**

Publisher接入Midas包括以下步骤：

· 签订合作条款：SHAREit和Publisher签订合作的合同条款；

· 注册Publisher账号：联系内部运营人员，在Midas系统注册Publisher账号，获取token；注意，目前Midas暂时不提供Publisher自助注册后台，由内部运营人员帮助Publisher完成账号注册和相关信息的填写；

· 开发对接：Publisher按照本文档开发接口；开发过程有任何问题，Midas会安排产品和技术同事帮助解决产品和技术上的问题；

· 测试联调：开发完成后，双方进行测试联调；

· 正式上线：测试联调通过后，商务运营侧同事会跟进安排开启线上流量进行正式投放，并开始计费；

 

# **3 实时竞价**

## 3.1 数据传输

	Publisher与Midas之间采用HTTP通讯协议，使用POST方式发送BidRequest请求，数据采用JSON格式，BidRequest作为HTTP请求的content来发送。

 








## 3.2 HTTP请求

	在发起竞价请求的时候需要使用HTTP POST方式，因为它比HTTP GET可以附带更多的内容，并且也更容易支持二进制数据。

 








请求头：

|名称|值|说明|
|:----|:----|:----|
|x-openrtb-version|2.5|open rtb版本|

 

##  

## 3.3 HTTP响应

若Midas参与竞价，返回HTTP状态码200并返回BidResponse；若不参与竞价返回状态则是204。

 

## 3.4 展示统计和计费

计费依据的展示数量以nurl链接统计上报的数据为准。Publisher需保证一次广告最多只上报一次展示；同时保证上报的展示在Midas要求的展示延迟有效时间范围内。

 

Midas通过 bidresponse.seatbid.bid.exp 字段限定展示延迟有效时间。

 

# **4 实时竞价RTB接口参数说明**

## 4.1 Bid Request

|**参数名称**|**类型**|**是否必传**|**描述**|
|:----|:----|:----|:----|
|id|string|是|Bid Request 唯一标识|
|imp|object array|是|一次请求只包含一个impression object；impression object 代表一个广告位的请求；具体见 imp 参数说明；|
|app|object |是|Publisher的app对象信息，仅推荐应用于app；具体见 app object 参数说明|
|device|object|是|设备信息，具体见 device object 参数说明|
|user|object|否|设备用户，广告受众，具体见user object参数说明|
|at|integer|是|拍卖结算类型， 值为 1 表示一价结算，值为 2 表示二价结算|
|badv|string array|否|域名黑名单|
|bapp|string array|否|App黑名单|
|regs|object|否|政策法规要求|

### 4.1.1 imp object

|**参数名称**|**类型**|**是否必传**|**描述**|
|:----|:----|:----|:----|
|id|string|是|请求中唯一标识本次出售展示的标识；每次请求仅出售一个展示|
|tagid|string|是|固定广告位唯一标识|
|native|object |Native流量必传|见 native object， 目前仅支持native|
|banner|object|Banner流量必传|见 banner object|
|video|object|Video流量必传|见 video object|
|audio|object|Audio流量必传|见 audio object|
|exp|integer|否|竞价到展示的时长 , 单位秒|
|secure|integer|否|标识展示请求是否需要使用 HTTPS 加密物料信息以及 markup 以保证安全， 0标识不需要使用安全链路， 1标识需要使用安全链路， 如果不填充，则表示未知， 可以认为是不需要使用安全链路；|
|bid floor|float|否|本次展示的最低 cpm 竞拍价格|
|bidfloorcur|string|否|竞价货币单位；当前仅支持USD|

 

### 4.1.2 banner object

| 参数名称 | **类型**     | **是否必传** | **描述**                                                     |
| :------- | :----------- | :----------- | :----------------------------------------------------------- |
| id       | string       | 否           | banner 对象的唯一标识；在一个 Ad 中包含 Banner 与 Video 的时候使用 |
| w        | integer      | 是           | 广告位宽度，单位为设备独立像素                               |
| h        | integer      | 是           | 广告位高度，单位为设备独立像素                               |
| pos      | integer      | 否           | 广告位置，枚举值参考 5.5广告位置属性枚举值列表               |
| mimes    | string array | 是           | 支持的内容 mime-type；包括但不仅限于"application/x-shockwave-flash", “image/jpg”, and “image/gif”。SHAREit 支持 "image/jpg"，"image/png", 和 "image/gif" |
| ext      | object       | 否           | 扩展字段                                                     |



### 4.1.3 native object

|参数名称|类型|是否必传|描述|
|:----|:----|:----|:----|
|request|string|是|符合native广告规范的请求，具体见下文的 native request object 参数说明|
|ver|string|是|采用的 Dynamic Native Ads API 的版本，默认1.2|
|api|integer array|否|该次展示可支持的 API 框架；枚举值参考 5.4 API框架枚举值 ； 该字段缺省表示所有枚举值均不支持；|
|battr|integer array|否|限制的物料属性，枚举值参考 5.3 creative属性枚举值列表；|

 

### 4.1.4 native request object

|**参数名称**|**类型**|**是否必传**|**描述**|
|:----|:----|:----|:----|
|assets|object array|是|通过 Asset object 表示 native 流量对素材和其他要素的要求；广告返回的素材和其他要素必须符合该 object 要求；|
|ver|string|是|Native 标签协议版本（Native Markupversion）；默认值为 1.2;|
|context|integer|否|广告场景；枚举值参考 5.10 Context Type IDs|
|contextsubtype|integer|否|更详细的广告场景；枚举值参考 5.11 Context Sub Type IDs|

 

### 4.1.5 asset request object 

|**参数名称**|**类型**|**是否必传**|**描述**|
|:----|:----|:----|:----|
|id|integer|是|对象唯一id|
|required|integer|是|表示该元素对象是否要求广告主必须返回值；值为 1 表示必须强制返回，值为 0 表示不强制|
|title|object|否|表示标题；见 title object；|
|img|object|否|表示图片；见 img object；|
|video|object|否|表示视频；见 video object；|
|data|object|否|表示其他信息，包括品牌名，描述，评级，价格等；见 data object；|

 

###  

### 4.1.6 title object 

|**参数名称**|**类型**|**是否必传**|**描述**|
|:----|:----|:----|:----|
|len|integer|是|最大长度， 推荐长度 25, 90, 140|

****

### 4.1.7 img object 

|**参数名称**|**类型**|**是否必传**|**描述**|
|:----|:----|:----|:----|
|type|integer|是|表示具体的图片类型，图片类型以及对应的枚举值如下：icon=1； 大图=3；|
|w|integer|否|图片宽度要求|
|wmin|integer|是|图片最小宽度。以像素为单位。|
|h|integer|否|图片高度要求|
|hmin|integer|是|图片最小高度。以像素为单位。|
|mimes|string array|否|支持的图片 mime-type；包括但不仅限于“image/jpg”和“image/gif”。|

****

###  

### 4.1.8 video object 

|**参数名称**|**类型**|**是否必传**|**描述**|
|:----|:----|:----|:----|
|mimes|string array|是|支持的内容mime类型。当前只支持“video/ mp4”。|
|minduration|integer|是|最小的视频广告持续时间;单位：秒；|
|maxduration|integer|是|最大的视频广告持续时间;单位：秒；|
|protocols|integer array|是|Publisher在竞价响应种可以接受的视频协议，目前仅支持类型3，即vast3.0协议。|

****

### 4.1.9 data object 

|**参数名称**|**类型**|**是否必传**|**描述**|
|:----|:----|:----|:----|
|type|string array|是|表示 data object 对应的类型；每个 data object 对应一个 type；type 的枚举值5.12 Data Asset Types|
|len|integer|是|最大字符数限制；|

 

### 4.1.10 app object 

|**参数名称**|**类型**|**是否必传**|**描述**|
|:----|:----|:----|:----|
|id|string|否|内部定义的app id|
|name|string|否|内部的app名称|
|bundle|string|是|包名信息|
|domain|string|否|App 的域名，例如 mygame.foo.com|
|cat|string array|否|APP 的 IAB category；枚举值参见 5.1 IAB category枚举值列表|
|ver|string|否|App的版本号|
|publisher|object|是|Publisher的具体信息，详见 pulisher object|

 

### 4.1.11 publisher object 

|**参数名称**|**类型**|**是否必传**|**描述**|
|:----|:----|:----|:----|
|id|string|是|Publisher id，请求token信息。|
|name|string|否|Publisher名称|
|domain|string|否|Publisher的最高域名，例如“publisher.com”|
|cat|string array|否|APP的IAB category；枚举值参见 5.1 IAB category枚举值列表|

****

### 4.1.12 device object 

|**参数名称**|**类型**|**是否必传**|**描述**|
|:----|:----|:----|:----|
|ua|string|否|用户设备 HTTP 请求头中的 User-Agent 字段；|
|ip|string|是|用户当前网络的ipv4地址|
|geo|object|否|用户当前的地理位置信息，详见 geo object|
|didsha1|string|否|硬件设备ID(例如，IMEI);通过SHA1散列。|
|didmd5|string|否|硬件设备ID(例如，IMEI);通过MD5散列。|
|dpidsha1|string|否|平台设备ID(如Android ID);通过SHA1散列。|
|dpidmd5|string|否|平台设备ID(如Android ID);通过MD5散列。|
|macsha1|string|否|设备的MAC地址;通过SHA1散列|
|macmd5|string|否|设备的MAC地址;通过MD5散列|
|make|string|否|设备制造商(例如，“Apple”)。|
|model|string|否|设备型号(例如，“iPhone”)。|
|os|string|是|操作系统。例如Android、iOS|
|osv|string|否|操作系统版本|
|language|string|否|设备语言；使用 ISO-639-1-alpha-2；未知传unknown|
|connectiontype|integer|否|网络连接类型；枚举值参考 5.8 网络连接类型枚举值列表；|
|devicetype|integer|否|设备类型；枚举值参考 5.7 设备类型枚举值列表；|
|h|integer|否|以像素为单位的屏幕物理高度|
|w|integer|否|以像素为单位的屏幕物理宽度|
|js|integer|否|是否支持js ，0 - 不支持， 1 - 支持|
|ppi|integer|否|屏幕大小，英寸像素|
|dnt|integer|否|浏览器在 HTTP 头中设置的标准的 “Do NotTrack"标识， 0 表示不限制追踪， 1 表示限制（不允许）追踪；|
|lmt|integer|否|“限制广告追踪”表示用户对商业追踪行为的授权， 值为 0 表示不限制追踪，值为 1 表示限制追踪；|
|ifa|string|是|广告主标识， 明文表示；Android 传 gaid，iOS传idfa。|
|mccmnc|string|否|移动运营商|

****

### 4.1.13 geo object 

|**参数名称**|**类型**|**是否必传**|**描述**|
|:----|:----|:----|:----|
|lat|float|否|纬度信息，取值范围-90.0 到+90.0， 负值表示南方；|
|lon|float|否|经度信息， 取值返回-180.0 到+180.0， 负值表示西方；|
|type|integer|否|位置信息的源；值为 1 表示 GPS/定位服务；值为 2 表示 IP 地 址； 值为 3 表示用户提供,枚举值参考 5.13 Location Type|
|accuracy|integer|否|精度，详细到米；当经纬度是通过定位服务获取时，上报该字段；|
|ipservice|integer|否|ip服务提供者，适用于type = 2;枚举值参考 5.14 IP Location Services|
|country|string|否|Country code using ISO-3166-1-alpha-3.|
|region|string|否|Region code using ISO-3166-2; 2-letter state code if USA|
|city|string|否|使用联合国贸易运输代码的城市位置。参见附录代码链接|
|zip|string|否|邮政编码|
|utcoffset|integer|否|本地时间相对UTC时间的+/-分钟数|

 

### 4.1.14 user object 

|**参数名称**|**类型**|**是否必传**|**描述**|
|:----|:----|:----|:----|
|id|string|否|用户id|
|buyeruid|string|否|买方定义的用户id|
|gender|string|否|性别，“M” = 男, “F” = 女, “O” = 其他|
|geo|object|否|用户地理位置信息；|
|yob|integer|否|出生年份，4位整数。|
|keywords|string|否|逗号分隔的关键字列表，兴趣，或意图。|
|customdata|string|否|自定义数据|
|data |object array|否|额外用户数据。每个数据对象表示不同的数据源。|


### 4.1.15 data object 

|**参数名称**|**类型**|**是否必传**|**描述**|
|:----|:----|:----|:----|
|id|string|否|数据提供方id|
|name|string|否|数据提供方名称|
|segment|object array|否|包含实际数据信息的数据段|


### 4.1.16 segment object 

|**参数名称**|**类型**|**是否必传**|**描述**|
|:----|:----|:----|:----|
|id|string|否|数据提供方定义的数据段id|
|name|string|否|数据提供方定义的数据段名称|
|value|string|否|数据值|

 

## 4.2 Bid Response

|**参数名称**|**类型**|**是否必传**|**描述**|
|:----|:----|:----|:----|
|id|string|是|竞价请求的标识，即请求中携带的request id|
|seatbid|object array|是|一组 SeatBid 对象， 如果出价，则至少应该填充一个seatbid；|
|bidid|string|否|竞拍者生成的响应 ID, 辅助日志或者交易追踪；|
|cur|string|否|出价货币单位，使用 ISO-4217 码；不传默认 USD|
|ext|object|否|特定交易的协议的扩展信息占位符|

### 4.2.1 seatbid object

Midas只支持一次返回一个seatbid对象；每个seatbid下只支持返回一个bid

|**参数名称**|**类型**|**是否必传**|**描述**|
|:----|:----|:----|:----|
|bid|object array|是|至少一个 Bid 对象的数组，每个对象关联一个展示。|
|seat|string|否|出价者席位标识， 代表本次出价的出价人|
|group|interger|否|默认值为0. 代表所有的曝光竞价是否同时胜出或失败。0表示可以独立竞价。1表示作为一组同时胜出或失败。|
|ext|object|否|特定交易的协议的扩展信息占位符|

 

### 4.2.2 bid object

每个bid object 必须对应一个imp id，表示对该impression出价购买。

|**参数名称**|**类型**|**是否必传**|**描述**|
|:----|:----|:----|:----|
|id|string|是|竞拍者生成的竞价 ID，用于记录日志或行为追踪|
|impid|string|是|关联的竞价请求中的 Imp 对象的 ID|
|price|float|是|对该次展示的出价，以 CPM 表示|
|adid|string|否|预加载的广告ID。可以在竞价胜出的时候使用|
|nurl|string|是|胜出通知地址，如果竞价胜出的时候由交易平台调用。|
|adm|string|是|传输广告的内容字段。 native广告形式通过adm字段返回广告内容。详情参考native广告形式的返回说明。 视频通过adm字段返回vast形式的广告内容。详情参考vast video返回说明；|
|adomain|string array|否|用于限制检测的广告主域名， 对于旋转的物料可以是一个数组， 交易平台可以限制只允许一个域名。|
|bundle|string|是（下载类广告必传）|应用的包名信息；安卓包名示例 com.foo.mygame；ios 包名示例 id907394059|
|iurl|string|否|用于质量或者安全监测的表示广告活动内容的图像地址|
|cid|string|否|广告 id，辅助广告审核；iurl 代表的一组素材|
|crid|string|否|一组素材的id；辅助广告审核|
|cat|string array|是|creative的IAB内容类型；枚举值参考IAB category枚举值列表。表5.1|
|attr|integer array|否|描述creative的属性集合；枚举值参考creative属性枚举值列表 5.3|
|api|integer|否|该次展示可支持的 API 框架；枚举值参考 api 框架枚举值列表5.4|
|protocol|integer|否|支持的视频竞价响应协议；枚举值参考视频竞价响应协议枚举值列表；|
|qagmediarating|integer|是|表示根据 IAB IGQ 标准的素材内容等级； 枚举值参考 IQG 媒体评级枚举值列表；|
|dealid|string|否|如果出价从属于某个私有市场直接交易规则， 则指向竞价请求中该交易规则的deal.id|
|w|integer|否|创意的宽度，以像素为单位|
|h|integer|否|创意的高度，以像素为单位|
|exp|integer|否|广告从返回到实际展示的有效延迟时间，单位为秒；默认值为 3600；|
|ext|object|否|特定交易的协议的扩展信息占位符|

 

### 4.2.3 native广告形式的返回说明

Native广告返回的物料放在adm字段里面；物料内容为一个native object；native格式支持native 1.2版本的协议。

 

Native object字段结构如下：

|**参数名称**|**类型**|**是否必传**|**描述**|
|:----|:----|:----|:----|
|ver|string|否|Native 标签协议的版本|
|assets|array of objects|是|Native 广告素材列表；具体见 assets object；|
|link|object|是|广告的点击跳转链接；具体见 link object|
|imptrackers|array of string|是|展示监测链接数组在客户端发生广告展示时调用展示监测 url 上报展示；双方根据该字段的上报统计展示，并作为计费的依据；|
|ext|Object|否|扩展字段|

 

### 4.2.4 Assets object

严格按照请求时的asset对象返回asset，每个对象对应一个id，id与请求时的id一一对应。

|**参数名称**|**类型**|**是否必传**|**描述**|
|:----|:----|:----|:----|
|id|int|是|Asset 的唯一识别 ID,与 exchange 请求时的 asset id 必须一一对应；|
|required|int|否|如果asset是必须的则为1。|
|title|object|否|回传标题信息；具体见 title object；|
|img|object|否|回传图片信息；具体见 img object；|
|video|object|否|回传视频信息；具体见 video object；|
|data|object|否|回传其他数据信息，例如评分，价格等。具体见 data object；|
|ext|Object|否|扩展字段|

### 4.2.5 title object

|**参数名称**|**类型**|**是否必传**|**描述**|
|:----|:----|:----|:----|
|text|string|是|标题文字|
|len|integer|否|标题文字的长度|
|ext|object|否|扩展字段|

 

### 4.2.6 Img object

|**参数名称**|**类型**|**是否必传**|**描述**|
|:----|:----|:----|:----|
|type|integer|否|对于assetsurl或dcourl响应是必须的。对于嵌入式asset是非必须的。枚举值见 Image Asset Types列表。|
|url|string|是|图片素材的 url 地|
|w|integer|是|图片的宽度，单位为像素|
|h|integer|是|图片的高度，单位为像素|
|ext|object|否|扩展字段|

 


### 4.2.7 data object

|**参数名称**|**类型**|**是否必传**|**描述**|
|:----|:----|:----|:----|
|type|integer|否|Data值的类型。若传则严格按照Data type表的类型传|
|label|string|否|Data 对应的名称；若传则严格按照 Data type 表的 name 传值；|
|value|string|是|Data 的具体内容；根据具体每个 data type 的要求返回值|
|ext|object|否|扩展字段|

 

### 4.2.8 video object

通过 vasttag 参数返回 vast 格式的视频内容；

 

注意，native 广告形式返回的视频只是作为一个素材类型，因此不支持针对视频单独的展示和点击监测； 可支持针对视频的播放进度监测；

|**参数名称**|**类型**|**是否必传**|**描述**|
|:----|:----|:----|:----|
|vasttag|string|是|该参数值为 vast 格式的视频素材|

 

### 4.2.9 link object

|**参数名称**|**类型**|**是否必传**|**描述**|
|:----|:----|:----|:----|
|url|string|是|点击跳转 url 地址|
|clicktrackers|array of strings|否|第三方点击监测链接数组|
|ext|object|否|扩展字段|

 

# **5 RTB接口枚举值列表**

## 5.1 IAB category枚举值列表

|**Value**|**Description**|
|:----|:----|
|**IAB1**|**Arts & Entertainment**|
|IAB1-1|Books & Literature|
|IAB1-2|Celebrity Fan/Gossip|
|IAB1-3|Fine Art|
|IAB1-4|Humor|
|IAB1-5|Movies|
|IAB1-6|Music|
|IAB1-7|Television|
|IAB2|**Automotive**|
|IAB2-1|Auto Parts|
|IAB2-2|Auto Repair|
|IAB2-3|Buying/Selling Cars|
|IAB2-4|Car Culture|
|IAB2-5|Certified Pre-Owned|
|IAB2-6|Convertible|
|IAB2-7|Coupe|
|IAB2-8|Crossover|
|IAB2-9|Diesel|
|IAB2-10|Electric Vehicle|
|IAB2-11|Hatchback|
|IAB2-12|Hybrid|
|IAB2-13|Luxury|
|IAB2-14|Minivan|
|IAB2-15|Motorcycles|
|IAB2-16|Off-Road Vehicles|
|IAB2-17|Performance Vehicles|
|IAB2-18|Pickup|
|IAB2-19|Road-Side Assistance|
|IAB2-20|Sedan|
|IAB2-21|Trucks & Accessories|
|IAB2-22|Vintage Cars|
|IAB2-23|Wagon|
|**IAB3**|**Business**|
|IAB3-1|Advertising|
|IAB3-2|Agriculture|
|IAB3-3|Biotech/Biomedical|
|IAB3-4|Business Software|
|IAB3-5|Construction|
|IAB3-6|Forestry|
|IAB3-7|Government|
|IAB3-8|Green Solutions|
|IAB3-9|Human Resources|
|IAB3-10|Logistics|
|IAB3-11|Marketing|
|IAB3-12|Metals|
|**IAB4**|**Careers**|
|IAB4-1|Career Planning|
|IAB4-2|College|
|IAB4-3|Financial Aid|
|IAB4-4|Job Fairs|
|IAB4-5|Job Search|
|IAB4-6|Resume Writing/Advice|
|IAB4-7|Nursing|
|IAB4-8|Scholarships|
|IAB4-9|Telecommuting|
|IAB4-10|U.S. Military|
|IAB4-11|Career Advice|
|**IAB5**|**Education**|
|IAB5-1|7-12 Education|
|IAB5-2|Adult Education|
|IAB5-3|Art History|
|IAB5-4|College Administration|
|IAB5-5|College Life|
|IAB5-6|Distance Learning|
|IAB5-7|English as a 2nd Language|
|IAB5-8|Language Learning|
|IAB5-9|Graduate School|
|IAB5-10|Homeschooling|
|IAB5-11|Homework/Study Tips|
|IAB5-12|K-6 Educators|
|IAB5-13|Private School|
|IAB5-14|Special Education|
|IAB5-15|Studying Business|
|**IAB6**|**Family & Parenting**|
|IAB6-1|Adoption|
|IAB6-2|Babies & Toddlers|
|IAB6-3|Daycare/Pre School|
|IAB6-4|Family Internet|
|IAB6-5|Parenting - K-6 Kids|
|IAB6-6|Parenting teens|
|IAB6-7|Pregnancy|
|IAB6-8|Special Needs Kids|
|IAB6-9|Eldercare|
|**IAB7**|**Health & Fitness**|
|IAB7-1|Exercise|
|IAB7-2|ADD|
|IAB7-3|AIDS/HIV|
|IAB7-4|Allergies|
|IAB7-5|Alternative Medicine|
|IAB7-6|Arthritis|
|IAB7-7|Asthma|
|IAB7-8|Autism/PDD|
|IAB7-9|Bipolar Disorder|
|IAB7-10|Brain Tumor|
|IAB7-11|Cancer|
|IAB7-12|Cholesterol|
|IAB7-13|Chronic Fatigue Syndrome|
|IAB7-14|Chronic Pain|
|IAB7-15|Cold & Flu|
|IAB7-16|Deafness|
|IAB7-17|Dental Care|
|IAB7-18|Depression|
|IAB7-19|Dermatology|
|IAB7-20|Diabetes|
|IAB7-21|Epilepsy|
|IAB7-22|GERD/Acid Reflux|
|IAB7-23|Headaches/Migraines|
|IAB7-24|Heart Disease|
|IAB7-25|Herbs for Health|
|IAB7-26|Holistic Healing|
|IAB7-27|IBS/Crohn’s Disease|
|IAB7-28|Incest/Abuse Support|
|IAB7-29|Incontinence|
|IAB7-30|Infertility|
|IAB7-31|Men’s Health|
|IAB7-32|Nutrition|
|IAB7-33|Orthopedics|
|IAB7-34|Panic/Anxiety Disorders|
|IAB7-35|Pediatrics|
|IAB7-36|Physical Therapy|
|IAB7-37|Psychology/Psychiatry|
|IAB7-38|Senior Health|
|IAB7-39|Sexuality|
|IAB7-40|Sleep Disorders|
|IAB7-41|Smoking Cessation|
|IAB7-42|Substance Abuse|
|IAB7-43|Thyroid Disease|
|IAB7-44|Weight Loss|
|IAB7-45|Women's Health|
|**IAB8**|**Food & Drink**|
|IAB8-1|American Cuisine|
|IAB8-2|Barbecues & Grilling|
|IAB8-3|Cajun/Creole|
|IAB8-4|Chinese Cuisine|
|IAB8-5|Cocktails/Beer|
|IAB8-6|Coffee/Tea|
|IAB8-7|Cuisine-Specific|
|IAB8-8|Desserts & Baking|
|IAB8-9|Dining Out|
|IAB8-10|Food Allergies|
|IAB8-11|French Cuisine|
|IAB8-12|Health/Low-Fat Cooking|
|IAB8-13|Italian Cuisine|
|IAB8-14|Japanese Cuisine|
|IAB8-15|Mexican Cuisine|
|IAB8-16|Vegan|
|IAB8-17|Vegetarian|
|IAB8-18|Wine|
|**IAB9**|**Hobbies & Interests**|
|IAB9-1|Art/Technology|
|IAB9-2|Arts & Crafts|
|IAB9-3|Beadwork|
|IAB9-4|Bird-Watching|
|IAB9-5|Board Games/Puzzles|
|IAB9-6|Candle & Soap Making|
|IAB9-7|Card Games|
|IAB9-8|Chess|
|IAB9-9|Cigars|
|IAB9-10|Collecting|
|IAB9-11|Comic Books|
|IAB9-12|Drawing/Sketching|
|IAB9-13|Freelance Writing|
|IAB9-14|Genealogy|
|IAB9-15|Getting Published|
|IAB9-16|Guitar|
|IAB9-17|Home Recording|
|IAB9-18|Investors & Patents|
|IAB9-19|Jewelry Making|
|IAB9-20|Magic & Illusion|
|IAB9-21|Needlework|
|IAB9-22|Painting|
|IAB9-23|Photography|
|IAB9-24|Radio|
|IAB9-25|Roleplaying Games|
|IAB9-26|Sci-Fi & Fantasy|
|IAB9-27|Scrapbooking|
|IAB9-28|Screenwriting|
|IAB9-29|Stamps & Coins|
|IAB9-30|Video & Computer Games|
|IAB9-31|Woodworking|
|**IAB10**|**Home & Garden**|
|IAB10-1|Appliances|
|IAB10-2|Entertaining|
|IAB10-3|Environmental Safety|
|IAB10-4|Gardening|
|IAB10-5|Home Repair|
|IAB10-6|Home Theater|
|IAB10-7|Interior Decorating|
|IAB10-8|Landscaping|
|IAB10-9|Remodeling & Construction|
|**IAB11**|**Law, Government, & Politics**|
|IAB11-1|Immigration|
|IAB11-2|Legal Issues|
|IAB11-3|U.S. Government Resources|
|IAB11-4|Politics|
|IAB11-5|Commentary|
|**IAB12**|**News**|
|IAB12-1|International News|
|IAB12-2|National News|
|IAB12-3|Local News|
|**IAB13**|**Personal Finance**|
|IAB13-1|Beginning Investing|
|IAB13-2|Credit/Debt & Loans|
|IAB13-3|Financial News|
|IAB13-4|Financial Planning|
|IAB13-5|Hedge Fund|
|IAB13-6|Insurance|
|IAB13-7|Investing|
|IAB13-8|Mutual Funds|
|IAB13-9|Options|
|IAB13-10|Retirement Planning|
|IAB13-11|Stocks|
|IAB13-12|Tax Planning|
|**IAB14**|**Society**|
|IAB14-1|Dating|
|IAB14-2|Divorce Support|
|IAB14-3|Gay Life|
|IAB14-4|Marriage|
|IAB14-5|Senior Living|
|IAB14-6|Teens|
|IAB14-7|Weddings|
|IAB14-8|Ethnic Specific|
|**IAB15**|**Science**|
|IAB15-1|Astrology|
|IAB15-2|Biology|
|IAB15-3|Chemistry|
|IAB15-4|Geology|
|IAB15-5|Paranormal Phenomena|
|IAB15-6|Physics|
|IAB15-7|Space/Astronomy|
|IAB15-8|Geography|
|IAB15-9|Botany|
|IAB15-10|Weather|
|**IAB16**|**Pets**|
|IAB16-1|Aquariums|
|IAB16-2|Birds|
|IAB16-3|Cats|
|IAB16-4|Dogs|
|IAB16-5|Large Animals|
|IAB16-6|Reptiles|
|IAB16-7|Veterinary Medicine|
|**IAB17**|**Sports**|
|IAB17-1|Auto Racing|
|IAB17-2|Baseball|
|IAB17-3|Bicycling|
|IAB17-4|Bodybuilding|
|IAB17-5|Boxing|
|IAB17-6|Canoeing/Kayaking|
|IAB17-7|Cheerleading|
|IAB17-8|Climbing|
|IAB17-9|Cricket|
|IAB17-10|Figure Skating|
|IAB17-11|Fly Fishing|
|IAB17-12|Football|
|IAB17-13|Freshwater Fishing|
|IAB17-14|Game & Fish|
|IAB17-15|Golf|
|IAB17-16|Horse Racing|
|IAB17-17|Horses|
|IAB17-18|Hunting/Shooting|
|IAB17-19|Inline Skating|
|IAB17-20|Martial Arts|
|IAB17-21|Mountain Biking|
|IAB17-22|NASCAR Racing|
|IAB17-23|Olympics|
|IAB17-24|Paintball|
|IAB17-25|Power & Motorcycles|
|IAB17-26|Pro Basketball|
|IAB17-27|Pro Ice Hockey|
|IAB17-28|Rodeo|
|IAB17-29|Rugby|
|IAB17-30|Running/Jogging|
|IAB17-31|Sailing|
|IAB17-32|Saltwater Fishing|
|IAB17-33|Scuba Diving|
|IAB17-34|Skateboarding|
|IAB17-35|Skiing|
|IAB17-36|Snowboarding|
|IAB17-37|Surfing/Body-Boarding|
|IAB17-38|Swimming|
|IAB17-39|Table Tennis/Ping-Pong|
|IAB17-40|Tennis|
|IAB17-41|Volleyball|
|IAB17-42|Walking|
|IAB17-43|Waterski/Wakeboard|
|IAB17-44|World Soccer|
|**IAB18**|**Style & Fashion**|
|IAB18-1|Beauty|
|IAB18-2|Body Art|
|IAB18-3|Fashion|
|IAB18-4|Jewelry|
|IAB18-5|Clothing|
|IAB18-6|Accessories|
|**IAB19**|**Technology & Computing**|
|IAB19-1|3-D Graphics|
|IAB19-2|Animation|
|IAB19-3|Antivirus Software|
|IAB19-4|C/C++|
|IAB19-5|Cameras & Camcorders|
|IAB19-6|Cell Phones|
|IAB19-7|Computer Certification|
|IAB19-8|Computer Networking|
|IAB19-9|Computer Peripherals|
|IAB19-10|Computer Reviews|
|IAB19-11|Data Centers|
|IAB19-12|Databases|
|IAB19-13|Desktop Publishing|
|IAB19-14|Desktop Video|
|IAB19-15|Email|
|IAB19-16|Graphics Software|
|IAB19-17|Home Video/DVD|
|IAB19-18|Internet Technology|
|IAB19-19|Java|
|IAB19-20|JavaScript|
|IAB19-21|Mac Support|
|IAB19-22|MP3/MIDI|
|IAB19-23|Net Conferencing|
|IAB19-24|Net for Beginners|
|IAB19-25|Network Security|
|IAB19-26|Palmtops/PDAs|
|IAB19-27|PC Support|
|IAB19-28|Portable|
|IAB19-29|Entertainment|
|IAB19-30|Shareware/Freeware|
|IAB19-31|Unix|
|IAB19-32|Visual Basic|
|IAB19-33|Web Clip Art|
|IAB19-34|Web Design/HTML|
|IAB19-35|Web Search|
|IAB19-36|Windows|
|**IAB20**|**Travel**|
|IAB20-1|Adventure Travel|
|IAB20-2|Africa|
|IAB20-3|Air Travel|
|IAB20-4|Australia & New Zealand|
|IAB20-5|Bed & Breakfasts|
|IAB20-6|Budget Travel|
|IAB20-7|Business Travel|
|IAB20-8|By US Locale|
|IAB20-9|Camping|
|IAB20-10|Canada|
|IAB20-11|Caribbean|
|IAB20-12|Cruises|
|IAB20-13|Eastern Europe|
|IAB20-14|Europe|
|IAB20-15|France|
|IAB20-16|Greece|
|IAB20-17|Honeymoons/Getaways|
|IAB20-18|Hotels|
|IAB20-19|Italy|
|IAB20-20|Japan|
|IAB20-21|Mexico & Central America|
|IAB20-22|National Parks|
|IAB20-23|South America|
|IAB20-24|Spas|
|IAB20-25|Theme Parks|
|IAB20-26|Traveling with Kids|
|IAB20-27|United Kingdom|
|**IAB21**|**Real Estate**|
|IAB21-1|Apartments|
|IAB21-2|Architects|
|IAB21-3|Buying/Selling Homes|
|**IAB22**|**Shopping**|
|IAB22-1|Contests & Freebies|
|IAB22-2|Couponing|
|IAB22-3|Comparison|
|IAB22-4|Engines|
|**IAB23**|**Religion & Spirituality**|
|IAB23-1|Alternative Religions|
|IAB23-2|Atheism/Agnosticism|
|IAB23-3|Buddhism|
|IAB23-4|Catholicism|
|IAB23-5|Christianity|
|IAB23-6|Hinduism|
|IAB23-7|Islam|
|IAB23-8|Judaism|
|IAB23-9|Latter-Day Saints|
|IAB23-10|Pagan/Wiccan|
|**IAB24****IAB25**|**Uncategorized****Non-Standard Content**|
|IAB25-1|Unmoderated UGC|
|IAB25-2|Extreme Graphic/Explicit Violence|
|IAB25-3|Pornography|
|IAB25-4|Profane Content|
|IAB25-5|Hate Content|
|IAB25-6|Under Construction|
|IAB25-7|Incentivized|
|**IAB26**|**Illegal Content**|
|IAB26-1|Illegal Content|
|IAB26-2|Warez|
|IAB26-3|Spyware/Malware|
|IAB26-4|Copyright Infringement|

 

## 5.2 banner广告类型枚举值

|**Value**|**Description**|
|:----|:----|
|**1**|XHTML Text Ad (usually mobile)|
|**2**|XHTML Banner Ad. (usually mobile)|
|**3**|JavaScript Ad; must be valid XHTML (i.e., Script Tags Included)|
|**4**|iframe|

 

##  

## 5.3 creative属性枚举值列表

|值|描述|
|:----|:----|
|1|Audio Ad(Auto-Play)|
|2|Audio Ad(User Initiated)|
|3|Expandable(Automatic)|
|4|Expandable(User Initiated - Click)|
|5|Expandable(User Initiated - Rollover)|
|6|In-Banner Video Ad (Auto-Play)|
|7|In-Banner Video Ad (User Initiated)|
|8|Pop (e.g., Over, Under, or Upon Exit)|
|9|Provocative or Suggestive Imagery|
|10|Shaky, Flashing, Flickering, Extreme Animation, Smileys|
|11|Surveys|
|12|Text Only|
|13|User Interactive (e.g., Embedded Games)|
|14|Windows Dialog or Alert Style|
|15|Has Audio On/Off Button|
|16|Ad Provides Skip Button (e.g. VPAID-rendered skip button on pre-roll video)|
|17|Adobe Flash|

## 5.4 API框架枚举值

|值|描述|
|:----|:----|
|1|VPAID 1.0|
|2|VPAID 2.0|
|3|MRAID-1|
|4|ORMMA|
|5|MRAID-2|
|6|MRAID-3|

 

##  

## 5.5 广告位置属性枚举值列表

|Value|Description|
|:----|:----|
|0|Unknown|
|1|Above the Fold|
|2|DEPRECATED - May or may not be initially visible depending on screen size/resolution.|
|3|Below the Fold|
|4|Header|
|5|Footer|
|6|Sidebar|
|7|Full Screen|

 

## 5.6 视频竞价响应协议枚举值列表

|值|描述|
|:----|:----|
|1|VAST 1.0|
|2|VAST 2.0|
|3|VAST 3.0|
|4|VAST 1.0 Wrapper|
|5|VAST 2.0 Wrapper|
|6|VAST 3.0 Wrapper|
|7|VAST 4.0|
|8|DAAST 1.0|

 

## 5.7 设备类型枚举值列表

|**Value**|**Description Notes**|
|:----|:----|
|1|Mobile/Tablet Version 2.0|
|2|Personal Computer Version 2.0|
|3|Connected TV Version 2.0|
|4|Phone New for Version 2.2|
|5|Tablet New for Version 2.2|
|6|Connected Device New for Version 2.2|
|7|Set Top Box New for Version 2.2|

 

## 5.8 网络连接类型枚举值列表

|**Value**|**Description**|
|:----|:----|
|0|Unknown|
|1|Ethernet|
|2|WIFI|
|3|Cellular Network – Unknown Generation|
|4|Cellular Network – 2G|
|5|Cellular Network – 3G|
|6|Cellular Network – 4G|
|7|Cellular Network – 5G|

 

## 5.9 IQG 媒体评级枚举值列表

|**Value**|**Description**|
|:----|:----|
|1|All Audiences|
|2|Everyone Over 12|
|3|Mature Audiences|

 

## 5.10 Context Type IDs

|**Value**|**Description**|
|:----|:----|
|1|Content-centric context such as newsfeed, article, image gallery, video gallery, or similar|
|2|Social-centric context such as social network feed, email, chat, or similar.|
|3|Product context such as product listings, details, recommendations, reviews, or similar.|

 

## 5.11 Context Sub Type IDs

|**Value**|**Description**|
|:----|:----|
|10|General or mixed content.|
|11|Primarily article content (which of course could include images, etc as part of the article)|
|12|Primarily video content|
|13|Primarily audio content|
|14|Primarily image content|
|15|User-generated content - forums, comments, etc|
|20|General social content such as a general social network|
|21|Primarily email content|
|22|Primarily chat/IM content|
|30|Content focused on selling products, whether digital or physical|
|31|Application store/marketplace|
|32|Product reviews site primarily (which may sell product secondarily)|

 

## 5.12 Data Asset Types

|**Value**|**Description**|
|:----|:----|
|sponsored|Sponsored By message where response should contain the brand name of the sponsor.|
|desc|Descriptive text associated with the product or service being advertised. Longer length of text in response may be truncated or ellipsed by the exchange.|
|rating|Rating of the product being offered to the user. For example an app’s rating in an app store from 0-5.|
|likes|Number of social ratings or “likes” of the product being offered to the user.|
|downloads|Number downloads/installs of this product |
|price|Price for product / app / in-app purchase. Value should include currency symbol in localised format.|
|saleprice|Sale price that can be used together with price to indicate a discounted price compared to a regular price. Value should include currency symbol in localised format.|
|phone|Phone number|
|address|Address|
|desc2|Additional descriptive text associated with the product or service being advertised|
|displayurl|Display URL for the text ad. To be used when sponsoring entity doesn’t own the content. IE sponsored by BRAND on SITE (where SITE is transmitted in this field).|
|ctatext|CTA description - descriptive text describing a ‘call to action’ button for the destination URL.|

 

## 5.13 Location Type

|**Value**|**Description**|
|:----|:----|
|1|GPS/Location Services |
|2|IP Address |
|3|User provided (e.g., registration data)|

 

 

## 5.14 IP Location Services

|**Value**|**Description**|
|:----|:----|
|1|ip2location|
|2|Neustar (Quova)|
|3|MaxMind|
|4|NetAcuity (Digital Element) |

 

 

# **RTB Request示例**

## **1a) native icon image request示例**

{

  "id": "123",

  "imp": [{

    "id": "1",

    "tagid": "2049",

    "bidfloor": 0.01,

    "bidfloorcur": "USD",

    "native": {

      "request": "{\"assets\":[{\"required\":1,\"img\":{\"type\":1,\"wmin\":84,\"hmin\":84}}]，\"ver\": \"1.2\"}"

    },

    "instl": 0

  }],

  "app": {

    "storeurl": "",

    "ver": "8888888",

    "publisher": {

      "id": "de30a0c4-ed1c-42bd-a5ff-af65852ef34a"

    }

  },

  "device": {

    "ip": "1.1.1.1",

    "geo": {

      "country": "IDN"

    }

  }

}

## **1b) native large image request示例**

{

  "id": "123",

  "imp": [{

    "id": "1",

    "tagid": "2049",

    "bidfloor": 0.01,

    "bidfloorcur": "USD",

    "native": {

      "request": "{\"assets\":[{\"required\":1,\"img\":{\"type\":3,\"wmin\":600,\"hmin\":314}}],\"ver\": \"1.2\"}"

    },

    "instl": 0

  }],

  "app": {

    "storeurl": "",

    "ver": "8888888",

    "publisher": {

      "id": "de30a0c4-ed1c-42bd-a5ff-af65852ef34a"

    }

  },

  "device": {

    "ip": "1.1.1.1",

    "geo": {

      "country": "ID"

    }

  }

}


## **1c) native video request示例**

{

  "id": "32f94228-52d3-4bce-be88-5dd1d799ae4f",

  "imp": [{

    "id": "1",

    "tagid": "2061",

    "bidfloor": 1.00,

    "bidfloorcur": "USD",

    "secure": 0,

    "native": {

      "request": "{\"assets\":[{\"id\":1,\"required\":1,\"title\":{\"len \":100}},{\"id\":2,\"required \":0,\"data \":{\"type \":2,\"len \":100}},{\"id\":3,\"required \":0,\"img \":{\"type \":3}},{\"id \":4,\"required \":1,\"img \":{\"type\":1}},{\"id \":5,\"required \":0,\"img\":{\"type \":1}},{\"id\":6,\"required\":0,\"data\":{\"type \":12,\"len \":100}},{\"id\":7,\"required\":1,\"video\":{\"mimes\":[\"video/mp4\"],\"maxduration\":30,\"protocols\":[3],\"w\":1280,\"h\":720,\"linearity\":1,\"skip\":0,\"battr\":[16],\"maxbitrate\":2000,\"delivery\":[2],\"companionad\":[{\"w\":1280,\"h\":720,\"id\":\"1\",\"btype\":[1,2,4],\"battr\":[16],\"pos\":7,\"mimes\":[\"application/javascript\"],\"ext\":{\"orientation\":1}}]}}], \"ver\": \"1.2\"}"

	}

}],

 

  "app": {

    "id": "100461",

    "ver": "4050618",

    "publisher": {

      "id": "bd8b25d0-413e-4886-9adf-6a796ff47740"

    }

  },

  "device": {

    "ip": "1.1.1.1",

    "geo": {

      "country": "IND",

      "type": 2

    },

    "devicetype": 4,

    "make": "WIKO",

    "model": "samsung-sm-g900a",

    "os": "android",

    "osv": "5.1.1",

    "language": "en",

    "carrier": "310410",

    "connectiontype": 7,

    "ifa": "5731b4c8-7cc1-4a09-bb77-24f237d93a66",

    "didsha1": "ecae9042f3e549a4fd1af27a1ed38b5f682fa3ca",

    "didmd5": "c98de3c3706ae57c89f97c0778c8121d",

    "dpidsha1": "a07713adea29cd23fd10a2e81a4fb07e23e00fb7",

    "dpidmd5": "af4935679d392191157f687f48907492"

  },

  "at": 1,

  "tmax": 2000

}

## **1d) banner request example**
 ```json
{
    "id": "123",
    "imp": [{
        "id": "1",
        "tagid": "2051",
        "bidfloor": 0.00001,
        "bidfloorcur": "USD",
        "banner":{
            "w":320,
            "h":50,
            "mimes": ["image/png", "image/jpg", "image/gif"]
        },
        "instl": 0
    }],

    "app": {
        "storeurl": "",
        "ver": "8888888",
        "publisher": {
            "id": "5e33f618-42a0-47f0-b7b1-9ad9866f3b9a"
        }
    },
    "device": {
        "ip": "1.1.1.1",
        "geo": {
            "country": "ID"
        },
        "connectiontype": 7
    }
}
```

# **RTB Response 示例**

## **2a) native icon image response示例**

{

  "id": "123",

  "bidid": "47b61fdb-0c63-46fc-9343-df78cd5b700f",

  "seatbid": [{

    "bid": [{

      "id": "47b61fdb-0c63-46fc-9343-df78cd5b700f",

      "impid": "1",

      "price": 289,

      "nurl": "http://midas-tracker-dev.hellay.net/win.fcg?viewid=e6d8578664e2f10e11afa9c2e16cb37680d4df1e94d245ba2d33780f1edaba4ac6aa12fb51d3bf16e4e45ea2f5d7f6001a07323bea0c8e8feabe0dc8d191ef8ab417be2d03b8f2f42b670a47da2e4cfc471bc12cab9b8b93ea9f6096bc3754a4504199138bfc3de2175b25964e6353ca9390f6b7d21d46562d114b3ce6b79aff194ecdf27e608df0042697e7b0649503d67118df10c3755021e9f0de8f7a9b81239631fcaec5085653cc924139de74a362dd9e496589f008a5fe08421489a154726e484f2a9ffd7974a17970b211becc8a6e0cbeb0da71a2ae9adeb03e38ccd710ae582d51af0d38435548613dd6ccc224e2043f773fb9d19f781248676bd6cfeb60aebed339572462c42954267f6d5674eb418481a076e1e93a80211802c27dbe661bc5261764e6ce987657027a6622954100b11934fd1e5b5034a26c4eef9d554705b9f2ac51c3f0bd4a70dec9eb1e3de6f8fefd4f565a4c11a76f3c0cdf2b039cde4bc985eb65e5f5d73ef36b82b0a67ac3031310b383067f90eb6a5ae7259c9888337e30b69a5c1f0c089e4014766a861d9e6f391a6659687391ba1c3f8b3e4c1335b8a729d305b73b985ebd4e063cf47ca2b38f3312ab92df818eaf56342bea6633b7e9d2ae195fbf5200646ea3b3afaf413a76f1c6785087c65feec214112cbc835df6f6fcab32fdc31d1288e01128279a527a8c9601e290cbf7a138230eee2eaf3023cb15&auction_price=${AUCTION_PRICE}&sid=__SID__",

      "adm":"{\"native\":{\"ver\":\"1.2\",\"assets\":[{\"img\":{\"type\":1,\"url\":\"http://static-dev.rqmob.com/test/sa/20210710/53da2d2e6b4f1482ecc5afd2c15bc824__FILE_CM____FILE_CM480_720____WEBP__.png\",\"w\":84,\"h\":84}}],\"link\":{\"url\":\"https://play.google.com/store/apps/details?id=com.janel.doctor.ear\&hl=en\",\"clicktrackers\":[\"http://midas-tracker-dev.hellay.net/clk.fcg?viewid=e6d8578664e2f10e11afa9c2e16cb37680d4df1e94d245ba2d33780f1edaba4ac6aa12fb51d3bf16e4e45ea2f5d7f6001a07323bea0c8e8feabe0dc8d191ef8ab417be2d03b8f2f42b670a47da2e4cfc471bc12cab9b8b93ea9f6096bc3754a4504199138bfc3de2175b25964e6353ca9390f6b7d21d46562d114b3ce6b79aff194ecdf27e608df0042697e7b0649503d67118df10c3755021e9f0de8f7a9b81239631fcaec5085653cc924139de74a362dd9e496589f008a5fe08421489a154726e484f2a9ffd7974a17970b211becc8a6e0cbeb0da71a2ae9adeb03e38ccd710ae582d51af0d38435548613dd6ccc224e2043f773fb9d19f781248676bd6cfeb60aebed339572462c42954267f6d5674eb418481a076e1e93a80211802c27dbe661bc5261764e6ce987657027a6622954100b11934fd1e5b5034a26c4eef9d554705b9f2ac51c3f0bd4a70dec9eb1e3de6f8fefd4f565a4c11a76f3c0cdf2b039cde4bc985eb65e5f5d73ef36b82b0a67ac3031310b383067f90eb6a5ae7259c9888337e30b69a5c1f0c089e4014766a861d9e6f391a6659687391ba1c3f8b3e4c1335b8a729d305b73b985ebd4e063cf47ca2b38f3312ab92df818eaf56342bea6633b7e9d2ae195fbf5200646ea3b3afaf413a76f1c6785087c65feec214112cbc835df6f6fcab32fdc31d1288e01128279a527a8c9601f19dd0f5a75a3f7f38f1\&auction_price=${AUCTION_PRICE}\&sid=__SID__\",\"http://midas-tracker-dev.hellay.net/cpi_clk.fcg?log=eyJldmVudF9uYW1lIjoiQURfQ3BpQ2xpY2siLCJyZXF1ZXN0X2lkIjoiNDdiNjFmZGItMGM2My00NmZjLTkzNDMtZGY3OGNkNWI3MDBmIiwic3ViX3BsYXRmb3JtIjoiY3BpIiwiY2hhbm5lbCI6Ik9NQ19TREsiLCJjbGllbnRfaXAiOiIxLjEuMS4xIiwic2NyZWVuX3NpemUiOiIweDAiLCJjb3VudHJ5IjoiaWQiLCJQYXJhbXMiOlt7ImNhbXBhaWduX2lkIjoyMjM4LCJwb3NfaWQiOiIyMDQ5IiwiYWRfaWQiOjEwMDc2NiwiY2FtcF9uYW1lIjoicGFubHVoLXNhbiIsImFkX3BhY2thZ2VfbmFtZSI6ImNvbS5qYW5lbC5kb2N0b3IuZWFyIiwiY2lkIjozODgwMywiYmlkX3ByaWNlIjoxMjMwMDAwMDAsImVjcG0iOjI4OTU1MzgwNCwiZHNwX25hbWUiOiJTaGFyZWl0IiwiYXR0cl9wbGF0Zm9ybSI6MiwiYWRzZXRfaWQiOjEzNTAsImFkX25hbWUiOiJTQU4tU0RLIiwiYWRzZXRfbmFtZSI6IlNBTi1TREsiLCJwaHlfcG9zIjoiMjA0OSJ9XSwibWlkYXNfdmVyc2lvbiI6IjIuMCJ9\&sid=__SID__\"]},\"imptrackers\":[\"http://midas-tracker-dev.hellay.net/imp.fcg?viewid=e6d8578664e2f10e11afa9c2e16cb37680d4df1e94d245ba2d33780f1edaba4ac6aa12fb51d3bf16e4e45ea2f5d7f6001a07323bea0c8e8feabe0dc8d191ef8ab417be2d03b8f2f42b670a47da2e4cfc471bc12cab9b8b93ea9f6096bc3754a4504199138bfc3de2175b25964e6353ca9390f6b7d21d46562d114b3ce6b79aff194ecdf27e608df0042697e7b0649503d67118df10c3755021e9f0de8f7a9b81239631fcaec5085653cc924139de74a362dd9e496589f008a5fe08421489a154726e484f2a9ffd7974a17970b211becc8a6e0cbeb0da71a2ae9adeb03e38ccd710ae582d51af0d38435548613dd6ccc224e2043f773fb9d19f781248676bd6cfeb60aebed339572462c42954267f6d5674eb418481a076e1e93a80211802c27dbe661bc5261764e6ce987657027a6622954100b11934fd1e5b5034a26c4eef9d554705b9f2ac51c3f0bd4a70dec9eb1e3de6f8fefd4f565a4c11a76f3c0cdf2b039cde4bc985eb65e5f5d73ef36b82b0a67ac3031310b383067f90eb6a5ae7259c9888337e30b69a5c1f0c089e4014766a861d9e6f391a6659687391ba1c3f8b3e4c1335b8a729d305b73b985ebd4e063cf47ca2b38f3312ab92df818eaf56342bea6633b7e9d2ae195fbf5200646ea3b3afaf413a76f1c6785087c65feec214112cbc835df6f6fcab32fdc31d1288e01128279a527a8c9601f698cae6a06433d213ad0e27e61ecbf359716f\&auction_price=${AUCTION_PRICE}\&sid=__SID__\",\"http://midas-tracker-dev.hellay.net/cpi_imp.fcg?log=eyJldmVudF9uYW1lIjoiQURfQ3BpU2hvdyIsInJlcXVlc3RfaWQiOiI0N2I2MWZkYi0wYzYzLTQ2ZmMtOTM0My1kZjc4Y2Q1YjcwMGYiLCJzdWJfcGxhdGZvcm0iOiJjcGkiLCJjaGFubmVsIjoiT01DX1NESyIsImNsaWVudF9pcCI6IjEuMS4xLjEiLCJzY3JlZW5fc2l6ZSI6IjB4MCIsImNvdW50cnkiOiJpZCIsIlBhcmFtcyI6W3siY2FtcGFpZ25faWQiOjIyMzgsInBvc19pZCI6IjIwNDkiLCJhZF9pZCI6MTAwNzY2LCJjYW1wX25hbWUiOiJwYW5sdWgtc2FuIiwiYWRfcGFja2FnZV9uYW1lIjoiY29tLmphbmVsLmRvY3Rvci5lYXIiLCJjaWQiOjM4ODAzLCJiaWRfcHJpY2UiOjEyMzAwMDAwMCwiZWNwbSI6Mjg5NTUzODA0LCJkc3BfbmFtZSI6IlNoYXJlaXQiLCJhdHRyX3BsYXRmb3JtIjoyLCJhZHNldF9pZCI6MTM1MCwiYWRfbmFtZSI6IlNBTi1TREsiLCJhZHNldF9uYW1lIjoiU0FOLVNESyIsInBoeV9wb3MiOiIyMDQ5In1dLCJtaWRhc192ZXJzaW9uIjoiMi4wIn0\&sid=__SID__\"]}}",

      "adid": "100766"

    }]

  }],

  "cur": "USD"

}

## **2b) native large image response示例**

{

    "id": "123",

    "bidid": "292bb9a0-0782-4d36-b543-437b47d48fef",

    "seatbid": [

        {

            "bid": [

                {

                    "id": "292bb9a0-0782-4d36-b543-437b47d48fef",

                    "impid": "1",

                    "price": 286,

                    "nurl": "http://midas-tracker-dev.hellay.net/win.fcg?viewid=e6d8578664e2f10e11afa9c2e16cb37680d4df1e94d245ba2d33780f1edaba4ac6aa12fb51d3bf16e4e45ea2f5d7f6001a07323bea0c8e8feabe0dc8d191ef8ab417be2d03b8f2f42b650f47da2e4cfc41a095014a039da67c124ea7967efe7813bb7a17263fad53e827b61f999d776ff1cab439eac5ca4a5a90cec45454569c719dcaeb76ecee7917aa2e81aac1b96ee38c0faed998697cef41595637e9d1789ff24157fb37d290cfd4f37c4f87682e950964cada4915e29ade88ee663dcca4ef4080a3b2c18a39c819c8c68d685c27d4fe098fb694eab0fc95fdd54c55909b3783d7556de04d0b9e0a69556da80f1f3d5d7422078be5635fff6209f88e4240728987fcc3b0117f7a87bd743356242cc8518ea5fe9ea9e545354cdeb46fd1912a21fd776145ed8df1b7bb7d185bcf119adeebb6a0004f85202f4edb7a8822be24bfaf0e2372f0fdedc91dd8bc07c63c17b1e6f2c613863e4843124a3f7f2524aa6505bfa88c9dcc8133c8e11a0e4e7b71bc3a16ce3c8cb7025af21f84288355f9f330aae9c3fadf9177b6a2c81669ef00f0da7e95ef1562ebf10da4fa26849d609a370cfc8a6ea6052b1182dd14eeb31870ce4dc064b8be92fa58ba2f33aa7c3ae1b5c2cdd5d13a1a09335730d9bf1dd473e70f0c44eea10769dc831399256b411a96cd8791ca0e978fa858717c041af3cb8b5e96fcfbf4eb9fb1d6df74249cf5a46fc831e4bf&auction_price=${AUCTION_PRICE}&sid=__SID__",

                    "adm":"{\"native\":{\"ver\":\"1.2\",\"assets\":[{\"img\":{\"type\":3,\"url\":\"http://static-dev.rqmob.com/test/sa/20210710/95def64e487533b5415faaf8486491f5__FILE_CM____FILE_CM480_720____WEBP__.png\",\"w\":660,\"h\":346}}],\"link\":{\"url\":\"https://play.google.com/store/apps/details?id=com.janel.doctor.ear\&hl=en\",\"clicktrackers\":[\"http://midas-tracker-dev.hellay.net/clk.fcg?viewid=e6d8578664e2f10e11afa9c2e16cb37680d4df1e94d245ba2d33780f1edaba4ac6aa12fb51d3bf16e4e45ea2f5d7f6001a07323bea0c8e8feabe0dc8d191ef8ab417be2d03b8f2f42b650f47da2e4cfc41a095014a039da67c124ea7967efe7813bb7a17263fad53e827b61f999d776ff1cab439eac5ca4a5a90cec45454569c719dcaeb76ecee7917aa2e81aac1b96ee38c0faed998697cef41595637e9d1789ff24157fb37d290cfd4f37c4f87682e950964cada4915e29ade88ee663dcca4ef4080a3b2c18a39c819c8c68d685c27d4fe098fb694eab0fc95fdd54c55909b3783d7556de04d0b9e0a69556da80f1f3d5d7422078be5635fff6209f88e4240728987fcc3b0117f7a87bd743356242cc8518ea5fe9ea9e545354cdeb46fd1912a21fd776145ed8df1b7bb7d185bcf119adeebb6a0004f85202f4edb7a8822be24bfaf0e2372f0fdedc91dd8bc07c63c17b1e6f2c613863e4843124a3f7f2524aa6505bfa88c9dcc8133c8e11a0e4e7b71bc3a16ce3c8cb7025af21f84288355f9f330aae9c3fadf9177b6a2c81669ef00f0da7e95ef1562ebf10da4fa26849d609a370cfc8a6ea6052b1182dd14eeb31870ce4dc064b8be92fa58ba2f33aa7c3ae1b5c2cdd5d13a1a09335730d9bf1dd473e70f0c44eea10769dc831399256b411a96cd8791ca0e978fa858717c041af3cb8b5e96fcfbf4f892aad4d91638832204\&auction_price=${AUCTION_PRICE}\&sid=__SID__\",\"http://midas-tracker-dev.hellay.net/cpi_clk.fcg?log=eyJldmVudF9uYW1lIjoiQURfQ3BpQ2xpY2siLCJyZXF1ZXN0X2lkIjoiMjkyYmI5YTAtMDc4Mi00ZDM2LWI1NDMtNDM3YjQ3ZDQ4ZmVmIiwic3ViX3BsYXRmb3JtIjoiY3BpIiwiY2hhbm5lbCI6Ik9NQ19TREsiLCJjbGllbnRfaXAiOiIxLjEuMS4xIiwic2NyZWVuX3NpemUiOiIweDAiLCJjb3VudHJ5IjoiaWQiLCJQYXJhbXMiOlt7ImNhbXBhaWduX2lkIjoyMjM4LCJwb3NfaWQiOiIyMDQ5IiwiYWRfaWQiOjEwMDc2NiwiY2FtcF9uYW1lIjoicGFubHVoLXNhbiIsImFkX3BhY2thZ2VfbmFtZSI6ImNvbS5qYW5lbC5kb2N0b3IuZWFyIiwiY2lkIjozODgyNiwiYmlkX3ByaWNlIjoxMjMwMDAwMDAsImVjcG0iOjI4NjI2MjYxNCwiZHNwX25hbWUiOiJTaGFyZWl0IiwiYXR0cl9wbGF0Zm9ybSI6MiwiYWRzZXRfaWQiOjEzNTAsImFkX25hbWUiOiJTQU4tU0RLIiwiYWRzZXRfbmFtZSI6IlNBTi1TREsiLCJwaHlfcG9zIjoiMjA0OSJ9XSwibWlkYXNfdmVyc2lvbiI6IjIuMCJ9\&sid=__SID__\",\"http://ping-test.rqmob.com/click?ad=SAN-SDK\&ad_id=100766\&ad_type={ad_type}\&adpos_id=2049\&adset={adset}\&adset_id=1350\&advid=29\&_app_id=7752\&android_id=\&app_id=\&app_type=3\&beyla_id=\&c={c}\&c_id=38826\&campid=1321943\&channel_pkg=\&channel_pkg_ver=0\&cost_currency={cost_currency}\&cost_model={cost_model}\&cost_value={cost_value}\&country_code=id\&device_id=\&ext_info={ext_info}\&gaid=\&imei={imei}\&imsi={imsi}\&ip=1.1.1.1\&is_offline=1\&is_pre_install={is_pre_install}\&midas_camp_id=2238\&midas_traffic_type=1\&os_version=\&other_category={other_category}\&pkg=com.janel.doctor.ear\&placement=2049\&platform=adshonor\&real_attrplat=2\&remote_ip=1.1.1.1\&requestid=292bb9a0-0782-4d36-b543-437b47d48fef\&sid={sid}\&site_id={site_id}\&uagent=\",\"http://midas-tracker-dev.hellay.net/effect.fcg?viewid=e6d8578664e2f10e11afa9c2e16cb37680d4df1e94d245ba2d33780f1edaba4ac6aa12fb51d3bf16e4e45ea2f5d7f6001a07323bea0c8e8feabe0dc8d191ef8ab417be2d03b8f2f42b650f47da2e4cfc41a095014a039da67c124ea7967efe7813bb7a17263fad53e827b61f999d776ff1cab439eac5ca4a5a90cec45454569c719dcaeb76ecee7917aa2e81aac1b96ee38c0faed998697cef41595637e9d1789ff24157fb37d290cfd4f37c4f87682e950964cada4915e29ade88ee663dcca4ef4080a3b2c18a39c819c8c68d685c27d4fe098fb694eab0fc95fdd54c55909b3783d7556de04d0b9e0a69556da80f1f3d5d7422078be5635fff6209f88e4240728987fcc3b0117f7a87bd743356242cc8518ea5fe9ea9e545354cdeb46fd1912a21fd776145ed8df1b7bb7d185bcf119adeebb6a0004f85202f4edb7a8822be24bfaf0e2372f0fdedc91dd8bc07c63c17b1e6f2c613863e4843124a3f7f2524aa6505bfa88c9dcc8133c8e11a0e4e7b71bc3a16ce3c8cb7025af21f84288355f9f330aae9c3fadf9177b6a2c81669ef00f0da7e95ef1562ebf10da4fa26849d609a370cfc8a6ea6052b1182dd14eeb31870ce4dc064b8be92fa58ba2f33aa7c3ae1b5c2cdd5d13a1a09335730d9bf1dd473e70f0c44eea10769dc831399256b411a96cd8791ca0e978fa858717c041af3cb8b5e96fcfbf4ff97b0c7de2834ae6eb4de3deb240e3b1ff423\&effect_type={EFFECT_TYPE}\&sid=__SID__\"]},\"imptrackers\":[\"http://midas-tracker-dev.hellay.net/imp.fcg?viewid=e6d8578664e2f10e11afa9c2e16cb37680d4df1e94d245ba2d33780f1edaba4ac6aa12fb51d3bf16e4e45ea2f5d7f6001a07323bea0c8e8feabe0dc8d191ef8ab417be2d03b8f2f42b650f47da2e4cfc41a095014a039da67c124ea7967efe7813bb7a17263fad53e827b61f999d776ff1cab439eac5ca4a5a90cec45454569c719dcaeb76ecee7917aa2e81aac1b96ee38c0faed998697cef41595637e9d1789ff24157fb37d290cfd4f37c4f87682e950964cada4915e29ade88ee663dcca4ef4080a3b2c18a39c819c8c68d685c27d4fe098fb694eab0fc95fdd54c55909b3783d7556de04d0b9e0a69556da80f1f3d5d7422078be5635fff6209f88e4240728987fcc3b0117f7a87bd743356242cc8518ea5fe9ea9e545354cdeb46fd1912a21fd776145ed8df1b7bb7d185bcf119adeebb6a0004f85202f4edb7a8822be24bfaf0e2372f0fdedc91dd8bc07c63c17b1e6f2c613863e4843124a3f7f2524aa6505bfa88c9dcc8133c8e11a0e4e7b71bc3a16ce3c8cb7025af21f84288355f9f330aae9c3fadf9177b6a2c81669ef00f0da7e95ef1562ebf10da4fa26849d609a370cfc8a6ea6052b1182dd14eeb31870ce4dc064b8be92fa58ba2f33aa7c3ae1b5c2cdd5d13a1a09335730d9bf1dd473e70f0c44eea10769dc831399256b411a96cd8791ca0e978fa858717c041af3cb8b5e96fcfbf4ff97b0c7de2834ae6eb4de3deb240e3b1ff423\&auction_price=${AUCTION_PRICE}\&sid=__SID__\",\"http://midas-tracker-dev.hellay.net/cpi_imp.fcg?log=eyJldmVudF9uYW1lIjoiQURfQ3BpU2hvdyIsInJlcXVlc3RfaWQiOiIyOTJiYjlhMC0wNzgyLTRkMzYtYjU0My00MzdiNDdkNDhmZWYiLCJzdWJfcGxhdGZvcm0iOiJjcGkiLCJjaGFubmVsIjoiT01DX1NESyIsImNsaWVudF9pcCI6IjEuMS4xLjEiLCJzY3JlZW5fc2l6ZSI6IjB4MCIsImNvdW50cnkiOiJpZCIsIlBhcmFtcyI6W3siY2FtcGFpZ25faWQiOjIyMzgsInBvc19pZCI6IjIwNDkiLCJhZF9pZCI6MTAwNzY2LCJjYW1wX25hbWUiOiJwYW5sdWgtc2FuIiwiYWRfcGFja2FnZV9uYW1lIjoiY29tLmphbmVsLmRvY3Rvci5lYXIiLCJjaWQiOjM4ODI2LCJiaWRfcHJpY2UiOjEyMzAwMDAwMCwiZWNwbSI6Mjg2MjYyNjE0LCJkc3BfbmFtZSI6IlNoYXJlaXQiLCJhdHRyX3BsYXRmb3JtIjoyLCJhZHNldF9pZCI6MTM1MCwiYWRfbmFtZSI6IlNBTi1TREsiLCJhZHNldF9uYW1lIjoiU0FOLVNESyIsInBoeV9wb3MiOiIyMDQ5In1dLCJtaWRhc192ZXJzaW9uIjoiMi4wIn0\&sid=__SID__\"]}}",

                    "adid": "100766",

                    "bundle": "com.janel.doctor.ear"

                }

            ]

        }

    ],

    "cur": "USD"

}

## **2c) native video response示例**

{

  "id": "32f94228-52d3-4bce-be88-5dd1d799ae4f",

  "bidid": "a5254b1b-f95d-42b5-8415-c660712532b2",

  "seatbid": [{

    "bid": [{

      "id": "a5254b1b-f95d-42b5-8415-c660712532b2",

      "impid": "1",

      "price": 50,

      "nurl": "http://midas-tracker-dev.hellay.net/win.fcg?viewid=e6d8578664e2f10e11afa9c0e96cb3762e56ab1614a0eb790bda8dee1f46ddd0ed054d91e419ea8b705890309607448e65f407e9336bc0a90533085ff8f141a42984adca526e1cca8ddf69757aecb73b05a22892d982a78aabbf0a1f6079b696e653900269900c16d45a364f062e705ec9adb3b1d6334b2746ef9f3ed20dc6ef05d95e16aa65e23c7e0b52cb1ce70ee639943f7b95625198ec7ac09435643ed9c4abd37f686f89f4ea123215aa700aa73005e728bcb45d054bb75ba53b2bb105286ce8941b833064268f1fb47f6cb00b0c3030e892027fc3178b7aeda7a0fe9efcb34e32ccd590569643552cae4cb5d6a359acb6b326faf3434c5ddf7574179869e6482ca8a5fdb4b7d344ed3cb022cabf9bd33e1316be063fefb6ac55113ec00eda60f87e50c3019442885a22873312f97ffd6fed0b69871925b9fee5b1923bde7212f824eea9c66deb48a60a8a20bfb895d26a3e1fb1614eefcdd8e16d899ba83e79803d451f3f8664ea3a4263915eadb1ac314f715062365ec0fe9a97e01f3cba17f73142e937034de4aeab9e5c64a445367dbff95805c26dba982d7d97914c5b2670f5528553a93a7aefa85e4875a35d79633e319c6e65f344f47b302ef3a76f9fad88bc3d3b7890b08097a4839a8c58dcb83027b0e80ce12ee7988f8660da46b8723f736122efbee4441724ba697a81160c9014e45ab023847a4f79ad987c1e49da43b7d3a0fb3b6e3ecc6f6cd1d9cccb166e7495fcd7d41bda6ca2cf7cadfd3e8825843f081c8faa098a34121a36ae4164&auction_price=${AUCTION_PRICE}&sid=__SID__",

    "adm":"{\"native\":{\"assets\":[{\"id\":1,\"title\":{\"text\":\"openrtb-this is ad title\",\"len\":14}},{\"id\":7,\"video\":{\"vasttag\":\"\<VAST version=\\\"3.0\\\"\>\<Ad id=\\\"100674\\\"\>\<InLine\>\<AdSystem\>\<![CDATA[midas]]\>\</AdSystem\>\<AdTitle\>\<![CDATA[openrtb-this is ad title]]\>\</AdTitle\>\<Impression\>\<![CDATA[http://midas-tracker-dev.hellay.net/imp.fcg?viewid=e6d8578664e2f10e11afa9c0e96cb3762e56ab1614a0eb790bda8dee1f46ddd0ed054d91e419ea8b705890309607448e65f407e9336bc0a90533085ff8f141a42984adca526e1cca8ddf69757aecb73b05a22892d982a78aabbf0a1f6079b696e653900269900c16d45a364f062e705ec9adb3b1d6334b2746ef9f3ed20dc6ef05d95e16aa65e23c7e0b52cb1ce70ee639943f7b95625198ec7ac09435643ed9c4abd37f686f89f4ea123215aa700aa73005e728bcb45d054bb75ba53b2bb105286ce8941b833064268f1fb47f6cb00b0c3030e892027fc3178b7aeda7a0fe9efcb34e32ccd590569643552cae4cb5d6a359acb6b326faf3434c5ddf7574179869e6482ca8a5fdb4b7d344ed3cb022cabf9bd33e1316be063fefb6ac55113ec00eda60f87e50c3019442885a22873312f97ffd6fed0b69871925b9fee5b1923bde7212f824eea9c66deb48a60a8a20bfb895d26a3e1fb1614eefcdd8e16d899ba83e79803d451f3f8664ea3a4263915eadb1ac314f715062365ec0fe9a97e01f3cba17f73142e937034de4aeab9e5c64a445367dbff95805c26dba982d7d97914c5b2670f5528553a93a7aefa85e4875a35d79633e319c6e65f344f47b302ef3a76f9fad88bc3d3b7890b08097a4839a8c58dcb83027b0e80ce12ee7988f8660da46b8723f736122efbee4441724ba697a81160c9014e45ab023847a4f79ad987c1e49da43b7d3a0fb3b6e3ecc6f6cd1d9cccb166e7495fcd7d41bda6ca2cf7cadfd3e88259037090d8ef619b830020e3eeedd3895de2a52\&auction_price=${AUCTION_PRICE}\&sid=__SID__]]\>\</Impression\>\<Impression\>\<![CDATA[http://midas-tracker-dev.hellay.net/cpi_imp.fcg?log=eyJldmVudF9uYW1lIjoiQURfQ3BpU2hvdyIsInJlcXVlc3RfaWQiOiJhNTI1NGIxYi1mOTVkLTQyYjUtODQxNS1jNjYwNzEyNTMyYjIiLCJzdWJfcGxhdGZvcm0iOiJjcGkiLCJjaGFubmVsIjoiT01DX1NESyIsImNsaWVudF9pcCI6IjEuMS4xLjEiLCJwbGF0Zm9ybSI6MSwib3NfdmVyc2lvbiI6IjUuMS4xIiwiaW1laSI6ImVjYWU5MDQyZjNlNTQ5YTRmZDFhZjI3YTFlZDM4YjVmNjgyZmEzY2EiLCJicmFuZCI6IldJS08iLCJtb2RlbCI6InNhbXN1bmctc20tZzkwMGEiLCJzY3JlZW5fc2l6ZSI6IjB4MCIsImxhbmd1YWdlIjoiZW4iLCJnYWlkIjoiNTczMWI0YzgtN2NjMS00YTA5LWJiNzctMjRmMjM3ZDkzYTY2IiwibmV0d29ya190eXBlIjo3LCJjb3VudHJ5IjoiaW4iLCJQYXJhbXMiOlt7ImNhbXBhaWduX2lkIjoyMTM2LCJwb3NfaWQiOiIyMDYxIiwiYWRfaWQiOjEwMDY3NCwiY2FtcF9uYW1lIjoi5p2o5bCR55GcLXNoYXJlaXQt5rWL6K-VLW9wZW5ydGIiLCJhZF9wYWNrYWdlX25hbWUiOiJjYy5mb3Jlc3RhcHAiLCJjaWQiOjM4Njk4LCJiaWRfcHJpY2UiOjEwMDAwMDAwMDAsImVjcG0iOjY4MjE5MzIwMCwiZHNwX25hbWUiOiJTaGFyZWl0IiwiYXR0cl9wbGF0Zm9ybSI6MSwiYWRzZXRfaWQiOjEyMzQsImFkX25hbWUiOiLmnajlsJHnkZwtb3BlbnJ0Yi0xOOinhOagvC3lsI_nsbMiLCJhZHNldF9uYW1lIjoi5p2o5bCR55GcLW9wZW5ydGIt5bCP57GzIiwicGh5X3BvcyI6IjIwNjEifV0sIm1pZGFzX3ZlcnNpb24iOiIyLjAifQ\&sid=__SID__]]\>\</Impression\>\<Creatives\>\<Creative id=\\\"38698\\\" AdID=\\\"100674\\\"\>\<Linear\>\<Duration\>00:00:15\</Duration\>\<TrackingEvents\>\<Tracking event=\\\"start\\\"\>\<![CDATA[http://midas-tracker-dev.hellay.net/play.fcg?viewid=e6d8578664e2f10e11afa9c0e96cb3762e56ab1614a0eb790bda8dee1f46ddd0ed054d91e419ea8b705890309607448e65f407e9336bc0a90533085ff8f141a42984adca526e1cca8ddf69757aecb73b05a22892d982a78aabbf0a1f6079b696e653900269900c16d45a364f062e705ec9adb3b1d6334b2746ef9f3ed20dc6ef05d95e16aa65e23c7e0b52cb1ce70ee639943f7b95625198ec7ac09435643ed9c4abd37f686f89f4ea123215aa700aa73005e728bcb45d054bb75ba53b2bb105286ce8941b833064268f1fb47f6cb00b0c3030e892027fc3178b7aeda7a0fe9efcb34e32ccd590569643552cae4cb5d6a359acb6b326faf3434c5ddf7574179869e6482ca8a5fdb4b7d344ed3cb022cabf9bd33e1316be063fefb6ac55113ec00eda60f87e50c3019442885a22873312f97ffd6fed0b69871925b9fee5b1923bde7212f824eea9c66deb48a60a8a20bfb895d26a3e1fb1614eefcdd8e16d899ba83e79803d451f3f8664ea3a4263915eadb1ac314f715062365ec0fe9a97e01f3cba17f73142e937034de4aeab9e5c64a445367dbff95805c26dba982d7d97914c5b2670f5528553a93a7aefa85e4875a35d79633e319c6e65f344f47b302ef3a76f9fad88bc3d3b7890b08097a4839a8c58dcb83027b0e80ce12ee7988f8660da46b8723f736122efbee4441724ba697a81160c9014e45ab023847a4f79ad987c1e49da43b7d3a0fb3b6e3ecc6f6cd1d9cccb166e7495fcd7d41bda6ca2cf7cadfd3e882584321b04bde7018421460ddfba3c99\&play_duration={PLAYDURATION}\&sid=__SID__]]\>\</Tracking\>\<Tracking event=\\\"firstQuartile\\\"\>\<![CDATA[http://midas-tracker-dev.hellay.net/play.fcg?viewid=e6d8578664e2f10e11afa9c0e96cb3762e56ab1614a0eb790bda8dee1f46ddd0ed054d91e419ea8b705890309607448e65f407e9336bc0a90533085ff8f141a42984adca526e1cca8ddf69757aecb73b05a22892d982a78aabbf0a1f6079b696e653900269900c16d45a364f062e705ec9adb3b1d6334b2746ef9f3ed20dc6ef05d95e16aa65e23c7e0b52cb1ce70ee639943f7b95625198ec7ac09435643ed9c4abd37f686f89f4ea123215aa700aa73005e728bcb45d054bb75ba53b2bb105286ce8941b833064268f1fb47f6cb00b0c3030e892027fc3178b7aeda7a0fe9efcb34e32ccd590569643552cae4cb5d6a359acb6b326faf3434c5ddf7574179869e6482ca8a5fdb4b7d344ed3cb022cabf9bd33e1316be063fefb6ac55113ec00eda60f87e50c3019442885a22873312f97ffd6fed0b69871925b9fee5b1923bde7212f824eea9c66deb48a60a8a20bfb895d26a3e1fb1614eefcdd8e16d899ba83e79803d451f3f8664ea3a4263915eadb1ac314f715062365ec0fe9a97e01f3cba17f73142e937034de4aeab9e5c64a445367dbff95805c26dba982d7d97914c5b2670f5528553a93a7aefa85e4875a35d79633e319c6e65f344f47b302ef3a76f9fad88bc3d3b7890b08097a4839a8c58dcb83027b0e80ce12ee7988f8660da46b8723f736122efbee4441724ba697a81160c9014e45ab023847a4f79ad987c1e49da43b7d3a0fb3b6e3ecc6f6cd1d9cccb166e7495fcd7d41bda6ca2cf7cadfd3e882584321b04bde7018421460f1d3c35c57b95\&play_duration={PLAYDURATION}\&sid=__SID__]]\>\</Tracking\>\<Tracking event=\\\"midpoint\\\"\>\<![CDATA[http://midas-tracker-dev.hellay.net/play.fcg?viewid=e6d8578664e2f10e11afa9c0e96cb3762e56ab1614a0eb790bda8dee1f46ddd0ed054d91e419ea8b705890309607448e65f407e9336bc0a90533085ff8f141a42984adca526e1cca8ddf69757aecb73b05a22892d982a78aabbf0a1f6079b696e653900269900c16d45a364f062e705ec9adb3b1d6334b2746ef9f3ed20dc6ef05d95e16aa65e23c7e0b52cb1ce70ee639943f7b95625198ec7ac09435643ed9c4abd37f686f89f4ea123215aa700aa73005e728bcb45d054bb75ba53b2bb105286ce8941b833064268f1fb47f6cb00b0c3030e892027fc3178b7aeda7a0fe9efcb34e32ccd590569643552cae4cb5d6a359acb6b326faf3434c5ddf7574179869e6482ca8a5fdb4b7d344ed3cb022cabf9bd33e1316be063fefb6ac55113ec00eda60f87e50c3019442885a22873312f97ffd6fed0b69871925b9fee5b1923bde7212f824eea9c66deb48a60a8a20bfb895d26a3e1fb1614eefcdd8e16d899ba83e79803d451f3f8664ea3a4263915eadb1ac314f715062365ec0fe9a97e01f3cba17f73142e937034de4aeab9e5c64a445367dbff95805c26dba982d7d97914c5b2670f5528553a93a7aefa85e4875a35d79633e319c6e65f344f47b302ef3a76f9fad88bc3d3b7890b08097a4839a8c58dcb83027b0e80ce12ee7988f8660da46b8723f736122efbee4441724ba697a81160c9014e45ab023847a4f79ad987c1e49da43b7d3a0fb3b6e3ecc6f6cd1d9cccb166e7495fcd7d41bda6ca2cf7cadfd3e882584321b04bde70184214616daafdc\&play_duration={PLAYDURATION}\&sid=__SID__]]\>\</Tracking\>\<Tracking event=\\\"thirdQuartile\\\"\>\<![CDATA[http://midas-tracker-dev.hellay.net/play.fcg?viewid=e6d8578664e2f10e11afa9c0e96cb3762e56ab1614a0eb790bda8dee1f46ddd0ed054d91e419ea8b705890309607448e65f407e9336bc0a90533085ff8f141a42984adca526e1cca8ddf69757aecb73b05a22892d982a78aabbf0a1f6079b696e653900269900c16d45a364f062e705ec9adb3b1d6334b2746ef9f3ed20dc6ef05d95e16aa65e23c7e0b52cb1ce70ee639943f7b95625198ec7ac09435643ed9c4abd37f686f89f4ea123215aa700aa73005e728bcb45d054bb75ba53b2bb105286ce8941b833064268f1fb47f6cb00b0c3030e892027fc3178b7aeda7a0fe9efcb34e32ccd590569643552cae4cb5d6a359acb6b326faf3434c5ddf7574179869e6482ca8a5fdb4b7d344ed3cb022cabf9bd33e1316be063fefb6ac55113ec00eda60f87e50c3019442885a22873312f97ffd6fed0b69871925b9fee5b1923bde7212f824eea9c66deb48a60a8a20bfb895d26a3e1fb1614eefcdd8e16d899ba83e79803d451f3f8664ea3a4263915eadb1ac314f715062365ec0fe9a97e01f3cba17f73142e937034de4aeab9e5c64a445367dbff95805c26dba982d7d97914c5b2670f5528553a93a7aefa85e4875a35d79633e319c6e65f344f47b302ef3a76f9fad88bc3d3b7890b08097a4839a8c58dcb83027b0e80ce12ee7988f8660da46b8723f736122efbee4441724ba697a81160c9014e45ab023847a4f79ad987c1e49da43b7d3a0fb3b6e3ecc6f6cd1d9cccb166e7495fcd7d41bda6ca2cf7cadfd3e882584321b04bde7018421460adcb5a4e74a7686688a608288\&play_duration={PLAYDURATION}\&sid=__SID__]]\>\</Tracking\>\<Tracking event=\\\"complete\\\"\>\<![CDATA[http://midas-tracker-dev.hellay.net/play.fcg?viewid=e6d8578664e2f10e11afa9c0e96cb3762e56ab1614a0eb790bda8dee1f46ddd0ed054d91e419ea8b705890309607448e65f407e9336bc0a90533085ff8f141a42984adca526e1cca8ddf69757aecb73b05a22892d982a78aabbf0a1f6079b696e653900269900c16d45a364f062e705ec9adb3b1d6334b2746ef9f3ed20dc6ef05d95e16aa65e23c7e0b52cb1ce70ee639943f7b95625198ec7ac09435643ed9c4abd37f686f89f4ea123215aa700aa73005e728bcb45d054bb75ba53b2bb105286ce8941b833064268f1fb47f6cb00b0c3030e892027fc3178b7aeda7a0fe9efcb34e32ccd590569643552cae4cb5d6a359acb6b326faf3434c5ddf7574179869e6482ca8a5fdb4b7d344ed3cb022cabf9bd33e1316be063fefb6ac55113ec00eda60f87e50c3019442885a22873312f97ffd6fed0b69871925b9fee5b1923bde7212f824eea9c66deb48a60a8a20bfb895d26a3e1fb1614eefcdd8e16d899ba83e79803d451f3f8664ea3a4263915eadb1ac314f715062365ec0fe9a97e01f3cba17f73142e937034de4aeab9e5c64a445367dbff95805c26dba982d7d97914c5b2670f5528553a93a7aefa85e4875a35d79633e319c6e65f344f47b302ef3a76f9fad88bc3d3b7890b08097a4839a8c58dcb83027b0e80ce12ee7988f8660da46b8723f736122efbee4441724ba697a81160c9014e45ab023847a4f79ad987c1e49da43b7d3a0fb3b6e3ecc6f6cd1d9cccb166e7495fcd7d41bda6ca2cf7cadfd3e882584321b04bde7018421461d1c146c45935266\&play_duration={PLAYDURATION}\&sid=__SID__]]\>\</Tracking\>\<Tracking event=\\\"resume\\\"\>\<![CDATA[http://midas-tracker-dev.hellay.net/play.fcg?viewid=e6d8578664e2f10e11afa9c0e96cb3762e56ab1614a0eb790bda8dee1f46ddd0ed054d91e419ea8b705890309607448e65f407e9336bc0a90533085ff8f141a42984adca526e1cca8ddf69757aecb73b05a22892d982a78aabbf0a1f6079b696e653900269900c16d45a364f062e705ec9adb3b1d6334b2746ef9f3ed20dc6ef05d95e16aa65e23c7e0b52cb1ce70ee639943f7b95625198ec7ac09435643ed9c4abd37f686f89f4ea123215aa700aa73005e728bcb45d054bb75ba53b2bb105286ce8941b833064268f1fb47f6cb00b0c3030e892027fc3178b7aeda7a0fe9efcb34e32ccd590569643552cae4cb5d6a359acb6b326faf3434c5ddf7574179869e6482ca8a5fdb4b7d344ed3cb022cabf9bd33e1316be063fefb6ac55113ec00eda60f87e50c3019442885a22873312f97ffd6fed0b69871925b9fee5b1923bde7212f824eea9c66deb48a60a8a20bfb895d26a3e1fb1614eefcdd8e16d899ba83e79803d451f3f8664ea3a4263915eadb1ac314f715062365ec0fe9a97e01f3cba17f73142e937034de4aeab9e5c64a445367dbff95805c26dba982d7d97914c5b2670f5528553a93a7aefa85e4875a35d79633e319c6e65f344f47b302ef3a76f9fad88bc3d3b7890b08097a4839a8c58dcb83027b0e80ce12ee7988f8660da46b8723f736122efbee4441724ba697a81160c9014e45ab023847a4f79ad987c1e49da43b7d3a0fb3b6e3ecc6f6cd1d9cccb166e7495fcd7d41bda6ca2cf7cadfd3e882584321b04bde7018421460c21ba10f3e2\&play_duration={PLAYDURATION}\&sid=__SID__]]\>\</Tracking\>\</TrackingEvents\>\<VideoClicks\>\<ClickThrough\>\</ClickThrough\>\</VideoClicks\>\<MediaFiles\>\<MediaFile delivery=\\\"progressive\\\" type=\\\"video/mp4\\\" width=\\\"720\\\" height=\\\"1280\\\"\>\<![CDATA[http://static-dev.rqmob.com/test/sa/20210518/3cac33a7d34ff7d482b05fc00b59622e.mp4]]\>\</MediaFile\>\</MediaFiles\>\</Linear\>\</Creative\>\<Creative\>\<CompanionAds\>\<Companion width=\\\"720\\\" height=\\\"1280\\\"\>\<CompanionClickThrough\>\</CompanionClickThrough\>\<TrackingEvents\>\</TrackingEvents\>\<StaticResource creativeType=\\\"image/png\\\"\>\<![CDATA[http://static-dev.rqmob.com/test/sa/20201012/e804e830061916a6d502c740eb009a9a__FILE_CM____FILE_CM480_720__.png]]\>\</StaticResource\>\<IFrameResource\>\</IFrameResource\>\</Companion\>\</CompanionAds\>\</Creative\>\</Creatives\>\<Description\>\<![CDATA[openrtb-this is ad description]]\>\</Description\>\<Survey\>\</Survey\>\</InLine\>\</Ad\>\</VAST\>\"}}],\"link\":{\"url\":\"https://play.google.com/store/apps/details?id=cc.forestapp\&hl=en\",\"clicktrackers\":[\"http://midas-tracker-dev.hellay.net/clk.fcg?viewid=e6d8578664e2f10e11afa9c0e96cb3762e56ab1614a0eb790bda8dee1f46ddd0ed054d91e419ea8b705890309607448e65f407e9336bc0a90533085ff8f141a42984adca526e1cca8ddf69757aecb73b05a22892d982a78aabbf0a1f6079b696e653900269900c16d45a364f062e705ec9adb3b1d6334b2746ef9f3ed20dc6ef05d95e16aa65e23c7e0b52cb1ce70ee639943f7b95625198ec7ac09435643ed9c4abd37f686f89f4ea123215aa700aa73005e728bcb45d054bb75ba53b2bb105286ce8941b833064268f1fb47f6cb00b0c3030e892027fc3178b7aeda7a0fe9efcb34e32ccd590569643552cae4cb5d6a359acb6b326faf3434c5ddf7574179869e6482ca8a5fdb4b7d344ed3cb022cabf9bd33e1316be063fefb6ac55113ec00eda60f87e50c3019442885a22873312f97ffd6fed0b69871925b9fee5b1923bde7212f824eea9c66deb48a60a8a20bfb895d26a3e1fb1614eefcdd8e16d899ba83e79803d451f3f8664ea3a4263915eadb1ac314f715062365ec0fe9a97e01f3cba17f73142e937034de4aeab9e5c64a445367dbff95805c26dba982d7d97914c5b2670f5528553a93a7aefa85e4875a35d79633e319c6e65f344f47b302ef3a76f9fad88bc3d3b7890b08097a4839a8c58dcb83027b0e80ce12ee7988f8660da46b8723f736122efbee4441724ba697a81160c9014e45ab023847a4f79ad987c1e49da43b7d3a0fb3b6e3ecc6f6cd1d9cccb166e7495fcd7d41bda6ca2cf7cadfd3e88259732131e89c815952846\&auction_price=${AUCTION_PRICE}\&sid=__SID__\",\"http://midas-tracker-dev.hellay.net/cpi_clk.fcg?log=eyJldmVudF9uYW1lIjoiQURfQ3BpQ2xpY2siLCJyZXF1ZXN0X2lkIjoiYTUyNTRiMWItZjk1ZC00MmI1LTg0MTUtYzY2MDcxMjUzMmIyIiwic3ViX3BsYXRmb3JtIjoiY3BpIiwiY2hhbm5lbCI6Ik9NQ19TREsiLCJjbGllbnRfaXAiOiIxLjEuMS4xIiwicGxhdGZvcm0iOjEsIm9zX3ZlcnNpb24iOiI1LjEuMSIsImltZWkiOiJlY2FlOTA0MmYzZTU0OWE0ZmQxYWYyN2ExZWQzOGI1ZjY4MmZhM2NhIiwiYnJhbmQiOiJXSUtPIiwibW9kZWwiOiJzYW1zdW5nLXNtLWc5MDBhIiwic2NyZWVuX3NpemUiOiIweDAiLCJsYW5ndWFnZSI6ImVuIiwiZ2FpZCI6IjU3MzFiNGM4LTdjYzEtNGEwOS1iYjc3LTI0ZjIzN2Q5M2E2NiIsIm5ldHdvcmtfdHlwZSI6NywiY291bnRyeSI6ImluIiwiUGFyYW1zIjpbeyJjYW1wYWlnbl9pZCI6MjEzNiwicG9zX2lkIjoiMjA2MSIsImFkX2lkIjoxMDA2NzQsImNhbXBfbmFtZSI6IuadqOWwkeeRnC1zaGFyZWl0Lea1i-ivlS1vcGVucnRiIiwiYWRfcGFja2FnZV9uYW1lIjoiY2MuZm9yZXN0YXBwIiwiY2lkIjozODY5OCwiYmlkX3ByaWNlIjoxMDAwMDAwMDAwLCJlY3BtIjo2ODIxOTMyMDAsImRzcF9uYW1lIjoiU2hhcmVpdCIsImF0dHJfcGxhdGZvcm0iOjEsImFkc2V0X2lkIjoxMjM0LCJhZF9uYW1lIjoi5p2o5bCR55GcLW9wZW5ydGItMTjop4TmoLwt5bCP57GzIiwiYWRzZXRfbmFtZSI6IuadqOWwkeeRnC1vcGVucnRiLeWwj-exsyIsInBoeV9wb3MiOiIyMDYxIn1dLCJtaWRhc192ZXJzaW9uIjoiMi4wIn0\&sid=__SID__\"]},\"imptrackers\":[\"http://midas-tracker-dev.hellay.net/imp.fcg?viewid=e6d8578664e2f10e11afa9c0e96cb3762e56ab1614a0eb790bda8dee1f46ddd0ed054d91e419ea8b705890309607448e65f407e9336bc0a90533085ff8f141a42984adca526e1cca8ddf69757aecb73b05a22892d982a78aabbf0a1f6079b696e653900269900c16d45a364f062e705ec9adb3b1d6334b2746ef9f3ed20dc6ef05d95e16aa65e23c7e0b52cb1ce70ee639943f7b95625198ec7ac09435643ed9c4abd37f686f89f4ea123215aa700aa73005e728bcb45d054bb75ba53b2bb105286ce8941b833064268f1fb47f6cb00b0c3030e892027fc3178b7aeda7a0fe9efcb34e32ccd590569643552cae4cb5d6a359acb6b326faf3434c5ddf7574179869e6482ca8a5fdb4b7d344ed3cb022cabf9bd33e1316be063fefb6ac55113ec00eda60f87e50c3019442885a22873312f97ffd6fed0b69871925b9fee5b1923bde7212f824eea9c66deb48a60a8a20bfb895d26a3e1fb1614eefcdd8e16d899ba83e79803d451f3f8664ea3a4263915eadb1ac314f715062365ec0fe9a97e01f3cba17f73142e937034de4aeab9e5c64a445367dbff95805c26dba982d7d97914c5b2670f5528553a93a7aefa85e4875a35d79633e319c6e65f344f47b302ef3a76f9fad88bc3d3b7890b08097a4839a8c58dcb83027b0e80ce12ee7988f8660da46b8723f736122efbee4441724ba697a81160c9014e45ab023847a4f79ad987c1e49da43b7d3a0fb3b6e3ecc6f6cd1d9cccb166e7495fcd7d41bda6ca2cf7cadfd3e88259037090d8ef619b830020e3eeedd3895de2a52\&auction_price=${AUCTION_PRICE}\&sid=__SID__\",\"http://midas-tracker-dev.hellay.net/cpi_imp.fcg?log=eyJldmVudF9uYW1lIjoiQURfQ3BpU2hvdyIsInJlcXVlc3RfaWQiOiJhNTI1NGIxYi1mOTVkLTQyYjUtODQxNS1jNjYwNzEyNTMyYjIiLCJzdWJfcGxhdGZvcm0iOiJjcGkiLCJjaGFubmVsIjoiT01DX1NESyIsImNsaWVudF9pcCI6IjEuMS4xLjEiLCJwbGF0Zm9ybSI6MSwib3NfdmVyc2lvbiI6IjUuMS4xIiwiaW1laSI6ImVjYWU5MDQyZjNlNTQ5YTRmZDFhZjI3YTFlZDM4YjVmNjgyZmEzY2EiLCJicmFuZCI6IldJS08iLCJtb2RlbCI6InNhbXN1bmctc20tZzkwMGEiLCJzY3JlZW5fc2l6ZSI6IjB4MCIsImxhbmd1YWdlIjoiZW4iLCJnYWlkIjoiNTczMWI0YzgtN2NjMS00YTA5LWJiNzctMjRmMjM3ZDkzYTY2IiwibmV0d29ya190eXBlIjo3LCJjb3VudHJ5IjoiaW4iLCJQYXJhbXMiOlt7ImNhbXBhaWduX2lkIjoyMTM2LCJwb3NfaWQiOiIyMDYxIiwiYWRfaWQiOjEwMDY3NCwiY2FtcF9uYW1lIjoi5p2o5bCR55GcLXNoYXJlaXQt5rWL6K-VLW9wZW5ydGIiLCJhZF9wYWNrYWdlX25hbWUiOiJjYy5mb3Jlc3RhcHAiLCJjaWQiOjM4Njk4LCJiaWRfcHJpY2UiOjEwMDAwMDAwMDAsImVjcG0iOjY4MjE5MzIwMCwiZHNwX25hbWUiOiJTaGFyZWl0IiwiYXR0cl9wbGF0Zm9ybSI6MSwiYWRzZXRfaWQiOjEyMzQsImFkX25hbWUiOiLmnajlsJHnkZwtb3BlbnJ0Yi0xOOinhOagvC3lsI_nsbMiLCJhZHNldF9uYW1lIjoi5p2o5bCR55GcLW9wZW5ydGIt5bCP57GzIiwicGh5X3BvcyI6IjIwNjEifV0sIm1pZGFzX3ZlcnNpb24iOiIyLjAifQ\&sid=__SID__\"]}}",

      "adid": "100674"

    }]

  }],

  "cur": "USD"

}

## 2d）banner response example
​```json
{
    "id": "123",
    "bidid": "a1271eca-efa2-4f9f-8904-a853afec1289",
    "seatbid": [
        {
            "bid": [
                {
                    "id": "a1271eca-efa2-4f9f-8904-a853afec1289",
                    "impid": "1",
                    "price": 118.0806,
                    "nurl": "https://midas-tracker-test.hellay.net/win.fcg?viewid=e6d8578664e2f10e11afa8c3eb6cb3763be39af447238100277632423c9c212f3987802acb093fa8f226d2beb00ad9aa5d901a36db98e81c1c0f1639ad363decc476977f6ba1dcb36318671a7a606d34961563c37535b94719f7eb4a8d53d8b3c80d002386ea6227bc2e50cdbcaa0c40a29bee9ef33b63c3f56ae4c59790d02b2be9ce6be9598e84f92c0efb7e9fdcf311fb0326d959dbd306031f78ae6a4a34675ad8bb6e84571c015638463b14fab8eded30cabefcf16c758eac517853709909da5be52230975657723bd0182b8eb7e92423a55066d07ab9c25a2c38cb0df2af9462c41155773054912137f7f508187acdfde64036bbcc7108237648df4168089d1ce5c8bae76768ae1f5a7cbda8f78a3c788123653da1a5f9294f7f410ac5a440063a7a62443efae33253b0e2bb16ed36a35b8720a1a75cd3a053a8f6415a00ecb94de70f89dff8f1e9dd5c1317a91c63f186f6e20d346da0aa4bbb9699eac716e961ac71a8048f0ad68c64b5e6ef9b3fec4e7969facea589f05d3fd1e9db885bcfea115dd393addc945f6de28f8ee1c2706e225370fc925762ef0ecb7317c28478b40b09f7b7611fc975cb19ea99226b086ca9c98895c8b86f12fa79b8086c1a3cff9f768f9b729c65121107709120f269c2ea863bda8797e77a98e3f177128fcbb4f46da9addbb1da7ef21f9aef6d0b9b1f4890d3cf764037f056e83dc7ed96a55cfab626fcf5903476d16ece901558cb0c2edc5a43cc57e4c95218c9d01e2e816d005a4f7e&auction_price=${AUCTION_PRICE}&sid=__SID__",
                    "adm":"<a class=\"_rnd_ad_128263\" style=\"display:inline-block;\"><img src=\"https://static-dev.rqmob.com/test/sa/20210804/42728fc75ca1bdadb5ab20fc3b84900b__FILE_CM____FILE_CM480_720____WEBP__.png\" width=\"320px\" height=\"50px\" /></a><script type=\"text/javascript\">window.MidasHTML = (function() {  this.$el = undefined;  this.args = {};  this.isViewable = false;  var bindClick = function() {    click_trackers = this.args.click_trackers;    target_url = this.args.target_url;    this.$el.onclick = function() {      emitTrackers(click_trackers);      window.open(target_url);    }  };  var emitTrackers = function emitTrackers(urls) {    urls.forEach(function(url) {      let obj = document.createElement(\"img\");              obj.src = url;              obj.style.display=\"none\";              this.$el.appendChild(obj);    });  };  var viewableCheck = function() {    if (this.isViewable) {  return;    }    var viewPortHeight = window.innerHeight || document.documentElement.clientHeight;    var viewPortWidth = window.innerWidth || document.documentElement.clientWidth;    var rec = this.$el.getBoundingClientRect();    this.isViewable = (rec.top >= -rec.height / 2 && rec.left >= -rec.width / 2 && rec.bottom <= viewPortHeight + rec.height / 2 && rec.right <= viewPortWidth + rec.width / 2);    if (this.isViewable) {      emitTrackers(args.impression_trackers);      bindClick();      this.isViewable = true;      clearInterval(this.timer)    }  };  var init = function(args) {    this.args = args;    this.$el = document.querySelector(\".\" + args.scope);    this.img = (this.$el.getElementsByTagName('img') || [])[0];    this.img.onload = () =>viewableCheck();    this.timer = setInterval(() =>viewableCheck(), 300);  };  return function(args) {    init(args);  }})();MidasHTML({\"click_trackers\":[\"https://midas-tracker-test.hellay.net/clk.fcg?viewid=e6d8578664e2f10e11afa8c3eb6cb3763be39af447238100277632423c9c212f3987802acb093fa8f226d2beb00ad9aa5d901a36db98e81c1c0f1639ad363decc476977f6ba1dcb36318671a7a606d34961563c37535b94719f7eb4a8d53d8b3c80d002386ea6227bc2e50cdbcaa0c40a29bee9ef33b63c3f56ae4c59790d02b2be9ce6be9598e84f92c0efb7e9fdcf311fb0326d959dbd306031f78ae6a4a34675ad8bb6e84571c015638463b14fab8eded30cabefcf16c758eac517853709909da5be52230975657723bd0182b8eb7e92423a55066d07ab9c25a2c38cb0df2af9462c41155773054912137f7f508187acdfde64036bbcc7108237648df4168089d1ce5c8bae76768ae1f5a7cbda8f78a3c788123653da1a5f9294f7f410ac5a440063a7a62443efae33253b0e2bb16ed36a35b8720a1a75cd3a053a8f6415a00ecb94de70f89dff8f1e9dd5c1317a91c63f186f6e20d346da0aa4bbb9699eac716e961ac71a8048f0ad68c64b5e6ef9b3fec4e7969facea589f05d3fd1e9db885bcfea115dd393addc945f6de28f8ee1c2706e225370fc925762ef0ecb7317c28478b40b09f7b7611fc975cb19ea99226b086ca9c98895c8b86f12fa79b8086c1a3cff9f768f9b729c65121107709120f269c2ea863bda8797e77a98e3f177128fcbb4f46da9addbb1da7ef21f9aef6d0b9b1f4890d3cf764037f056e83dc7ed96a55cfab626fcf5903476d16ece901558cb0c2edc5a43cc44e9d2501eabccb23ead\&auction_price=${AUCTION_PRICE}\&sid=__SID__\",\"https://midas-tracker-test.hellay.net/cpi_clk.fcg?log=eyJldmVudF9uYW1lIjoiQURfQ3BpQ2xpY2siLCJyZXF1ZXN0X2lkIjoiYTEyNzFlY2EtZWZhMi00ZjlmLTg5MDQtYTg1M2FmZWMxMjg5Iiwic3ViX3BsYXRmb3JtIjoiY3BpIiwiY2hhbm5lbCI6Ik9NQ19TREsiLCJjbGllbnRfaXAiOiIxLjEuMS4xIiwic2NyZWVuX3NpemUiOiIweDAiLCJuZXR3b3JrX3R5cGUiOjcsInBhY2thZ2VfbmFtZSI6InRydWVjYWxsZXIiLCJjb3VudHJ5IjoiaWQiLCJQYXJhbXMiOlt7ImNhbXBhaWduX2lkIjoyODYwLCJwb3NfaWQiOiIyMTUzIiwiYWRfaWQiOjEyODI2MywiYWRfcGFja2FnZV9uYW1lIjoiY29tLm1pZ28ubW9iaWxlIiwiY2lkIjo0NzUyMiwiY3RfY3ZyIjowLjAwMzkzNjAxNjc0OSwiYmlkX3ByaWNlIjoxNTAwMDAwMCwiZWNwbSI6NTkwNDAyNTEsImRzcF9uYW1lIjoiU2hhcmVpdCIsImF0dHJfcGxhdGZvcm0iOjEsImlzX2F1dG9fZG93bmxvYWQiOjEsImFkc2V0X2lkIjoyMzU1LCJhZF9uYW1lIjoi5bm_5ZGKMSIsImFkc2V0X25hbWUiOiJiYW5uZXIyNiIsInBoeV9wb3MiOiIyMTUzIn1dLCJtaWRhc192ZXJzaW9uIjoiMi4wIiwiYXBwX2lkIjoidHJ1ZWNhbGxlciJ9\&sid=__SID__\",\"http://ping-test.rqmob.com/click?ad=广告1\&ad_id=128263\&ad_type={ad_type}\&adpos_id=2153\&adset={adset}\&adset_id=2355\&advid=29\&amp_app_id=10053\&android_id=\&app_id=truecaller\&app_type=3\&beyla_id=\&c={c}\&c_id=47522\&campid=1440793\&channel_pkg=truecaller\&channel_pkg_ver=0\&cost_currency={cost_currency}\&cost_model={cost_model}\&cost_value={cost_value}\&country_code=id\&cut_type={cut_type}\&device_id=\&ext_info={ext_info}\&gaid=\&imei={imei}\&imsi={imsi}\&ip=1.1.1.1\&is_offline=1\&is_pre_install={is_pre_install}\&midas_camp_id=2860\&midas_traffic_type=1\&os_version=\&other_category={other_category}\&package_type={package_type}\&pkg=com.migo.mobile\&placement=2153\&platform=adshonor\&real_attrplat=1\&remote_ip=1.1.1.1\&requestid=a1271eca-efa2-4f9f-8904-a853afec1289\&sid={sid}\&site_id={site_id}\&uagent=\"],\"impression_trackers\":[\"https://midas-tracker-test.hellay.net/imp.fcg?viewid=e6d8578664e2f10e11afa8c3eb6cb3763be39af447238100277632423c9c212f3987802acb093fa8f226d2beb00ad9aa5d901a36db98e81c1c0f1639ad363decc476977f6ba1dcb36318671a7a606d34961563c37535b94719f7eb4a8d53d8b3c80d002386ea6227bc2e50cdbcaa0c40a29bee9ef33b63c3f56ae4c59790d02b2be9ce6be9598e84f92c0efb7e9fdcf311fb0326d959dbd306031f78ae6a4a34675ad8bb6e84571c015638463b14fab8eded30cabefcf16c758eac517853709909da5be52230975657723bd0182b8eb7e92423a55066d07ab9c25a2c38cb0df2af9462c41155773054912137f7f508187acdfde64036bbcc7108237648df4168089d1ce5c8bae76768ae1f5a7cbda8f78a3c788123653da1a5f9294f7f410ac5a440063a7a62443efae33253b0e2bb16ed36a35b8720a1a75cd3a053a8f6415a00ecb94de70f89dff8f1e9dd5c1317a91c63f186f6e20d346da0aa4bbb9699eac716e961ac71a8048f0ad68c64b5e6ef9b3fec4e7969facea589f05d3fd1e9db885bcfea115dd393addc945f6de28f8ee1c2706e225370fc925762ef0ecb7317c28478b40b09f7b7611fc975cb19ea99226b086ca9c98895c8b86f12fa79b8086c1a3cff9f768f9b729c65121107709120f269c2ea863bda8797e77a98e3f177128fcbb4f46da9addbb1da7ef21f9aef6d0b9b1f4890d3cf764037f056e83dc7ed96a55cfab626fcf5903476d16ece901558cb0c2edc5a43cc43ecc8431995c0eaa2ffa3eba822597172e50f\&auction_price=${AUCTION_PRICE}\&sid=__SID__\",\"https://midas-tracker-test.hellay.net/cpi_imp.fcg?log=eyJldmVudF9uYW1lIjoiQURfQ3BpU2hvdyIsInJlcXVlc3RfaWQiOiJhMTI3MWVjYS1lZmEyLTRmOWYtODkwNC1hODUzYWZlYzEyODkiLCJzdWJfcGxhdGZvcm0iOiJjcGkiLCJjaGFubmVsIjoiT01DX1NESyIsImNsaWVudF9pcCI6IjEuMS4xLjEiLCJzY3JlZW5fc2l6ZSI6IjB4MCIsIm5ldHdvcmtfdHlwZSI6NywicGFja2FnZV9uYW1lIjoidHJ1ZWNhbGxlciIsImNvdW50cnkiOiJpZCIsIlBhcmFtcyI6W3siY2FtcGFpZ25faWQiOjI4NjAsInBvc19pZCI6IjIxNTMiLCJhZF9pZCI6MTI4MjYzLCJhZF9wYWNrYWdlX25hbWUiOiJjb20ubWlnby5tb2JpbGUiLCJjaWQiOjQ3NTIyLCJjdF9jdnIiOjAuMDAzOTM2MDE2NzQ5LCJiaWRfcHJpY2UiOjE1MDAwMDAwLCJlY3BtIjo1OTA0MDI1MSwiZHNwX25hbWUiOiJTaGFyZWl0IiwiYXR0cl9wbGF0Zm9ybSI6MSwiaXNfYXV0b19kb3dubG9hZCI6MSwiYWRzZXRfaWQiOjIzNTUsImFkX25hbWUiOiLlub_lkYoxIiwiYWRzZXRfbmFtZSI6ImJhbm5lcjI2IiwicGh5X3BvcyI6IjIxNTMifV0sIm1pZGFzX3ZlcnNpb24iOiIyLjAiLCJhcHBfaWQiOiJ0cnVlY2FsbGVyIn0\&sid=__SID__\"],\"scope\":\"_rnd_ad_128263\",\"target_url\":\"https://play.google.com/store/apps/details?id=com.migo.mobile\&hl=en\"});</script>",
                    "adid": "128263",
                    "bundle": "com.migo.mobile"
                }
            ]
        }
    ],
   "cur": "USD"
}
```

 

