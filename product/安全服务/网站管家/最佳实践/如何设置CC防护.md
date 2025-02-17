本文将为您介绍如何在 Web 应用防火墙控制台设置 CC 防护。
## 背景信息
CC 防护可以对网站特定的 URL 进行访问保护，CC 防护支持紧急模式 CC 防护和自定义 CC 防护策略。
>!紧急模式 CC 防护策略和自定义 CC 规则防护策略，不能同时开启。


## 操作步骤
### 示例一：紧急模式 CC 防护配置
>!紧急模式 CC 防护默认关闭，开启前请确认自定义 CC 防护规则处于未启用状态。
>
1.  登录 [Web 应用防火墙控制台](https://console.cloud.tencent.com/guanjia/waf/overview)，在左侧导航栏选择**基础安全**。
2. 在基础安全页面，左上角选择需要防护的域名，单击 **CC 防护**，进入 CC 防护页面。
![](https://qcloudimg.tencent-cloud.cn/raw/5e4564d35c7388711034878864f189eb.png)
3. 在紧急模式 CC 防护模块中，单击状态右侧的![](https://qcloudimg.tencent-cloud.cn/raw/b7881ceb022cdf653b379e111af23c1e.png)，经过二次确认后，开启紧急模式 CC 防护。
>?
>- 当开启紧急模式 CC 防护时，若网站遭大流量 CC 攻击会自动触发防护（网站 QPS 不低于1000QPS），无需人工参与。若无明确的防护路径，建议启用紧急模式 CC 防护，可能会存在一定误报。可以在控制台进入黑白名单，对拦截 IP 信息，进行加白处理。
>- 如果知晓明确的防护路径，建议使用自定义 CC 规则进行防护。
>
![](https://qcloudimg.tencent-cloud.cn/raw/264e6201a097e7a9f243d05c52a84b9c.png)



[](id:two)
### 示例二： 基于访问源 IP 的 CC 防护设置
基于 IP 的 CC 防护策略，不需要对 SESSION 维度进行设置，直接配置即可。
1.  登录 [Web 应用防火墙控制台](https://console.cloud.tencent.com/guanjia/waf/overview)，在左侧导航栏选择**基础安全**。
2. 在基础安全页面，左上角选择需要防护的域名，单击 **CC 防护**，进入 CC 防护页面。
![](https://qcloudimg.tencent-cloud.cn/raw/5e4564d35c7388711034878864f189eb.png)
3. 在 CC 防护页面，单击**添加规则**，弹出添加 CC 防护规则弹窗。
![](https://qcloudimg.tencent-cloud.cn/raw/099500e8c9cd0edf14a12a16bf13c89d.png)
4. 在添加 CC 防护规则弹窗中，填写相应信息。
>!IP 为识别方式时，规则被触发拦截时会全站封锁（该 IP 访问其他 URL 也拦截），SESSION 则不会全站拦截。
>
![](https://qcloudimg.tencent-cloud.cn/raw/875cc3ff4ab8ed154a9539da8aed2b45.png)
 **参数说明：**
 - 规则名称：自定义名称，50个字符以内。
 - 识别模式： IP、SESSION。
 - 匹配条件：包括相等、前缀匹配和包含。
 - 高级匹配：通过进行 GET 表单和 POST 表单参数过滤，支持更加精细化频率控制，提高命中率。
	 - 匹配字段：指定请求方法，支持 GET 或 POST。
	 - 参数名：请求字段中的参数名，最多512字符。
	 - 参数值：请求字段中的参数值，最多512字符。
	 示例说明：如下三条 GET 请求测试数据：a=1&b=11、a=2&b=12、 a=&b=13。
		- 如果 GET 配置参数名为 a，参数值为1，则1命中。
		- 如果 GET 配置参数名为 a，参数值为\*，则1、2、3均命中。
 - 访问频次：根据业务情况设置访问频次。建议输入正常访问次数的3倍 - 10倍，例如，网站人平均访问20次/分钟，可配置为60次/分钟 - 200次/分钟，可依据被攻击严重程度调整。
 - 执行动作：观察、人机识别和阻断。
 - 惩罚时长：最短为1分钟，最长为一周。
 - 优先级：请输入1 - 100的整数，数字越小，代表这条规则的执行优先级越高，相同优先级下，创建时间越晚，优先级越高。


### 示例三： 基于 SESSION 的 CC 防护设置
基于 SESSION 访问速率的 CC 防护，能够有效解决在办公网、商超和公共 Wi-Fi 场合，用户因使用相同 IP 出口而导致的误拦截问题。
>!使用基于 SESSION 的 CC 防护策略，需要先进行 SESSION 设置，才能设置基于 SESSION 的 CC 防护策略，下文步骤1 - 步骤4为 SESSION 设置操作。

1.  登录 [Web 应用防火墙控制台](https://console.cloud.tencent.com/guanjia/waf/overview)，在左侧导航栏选择**基础安全**。
2. 在基础安全页面，左上角选择需要防护的域名，单击 **CC 防护**，进入 CC 防护页面。
![](https://qcloudimg.tencent-cloud.cn/raw/5e4564d35c7388711034878864f189eb.png)
2. 在 SESSION 设置模块中，单击**设置**，设置 SESSION 维度信息。
![](https://qcloudimg.tencent-cloud.cn/raw/154dd23236f82de255052f8bfba0285b.png)
3. 在 SESSION 设置弹窗，此示例选择 COOKIE 作为测试内容，标识为 security，开始位置为0，结束位置为9，配置完成后单击**确定**即可。
![](https://qcloudimg.tencent-cloud.cn/raw/d12612ac943dceba856cfa033dcdf161.png)
 **参数说明：**
 - SESSION 位置 ：可选择 COOKIE、GET 或 POST，其中 GET 或 POST 是指 HTTP 请求内容参数，非 HTTP 头部信息。
 - 匹配说明 ：位置匹配或者字符串匹配。
 - SESSION 标识 ：取值标识。
 - 开始位置：字符串或者位置匹配的开始位置。
 - 结束位置：字符串或位置匹配的结束位置。
 - GET/POST 示例 ：如果一条请求的完整参数内容为：key_a = 124&key_b = 456&key_c = 789。
	 - 字符串匹配模式下，SESSION 标识为 key_b = ，结束字符为&，则匹配内容为456。
	 - 位置匹配模式下，SESSION 标识为 key_b，开始位置为0，结束位置2，则匹配内容为456。
 - COOKIE 示例 ：如果一条请求的完整 COOKIE 内容为：cookie_1 = 123;cookie_2 = 456;cookie_3 = 789。
	 - 字符串匹配模式下，SESSION 标识为 cookie_2 =，结束字符为“;”，则匹配内容为456。
	 - 位置匹配模式下，SESSION 标识为 cookie_2，开始位置为0，结束位置2，则匹配内容为456。
4. SESSION 维度信息测试。添加完成后，单击**测试**将填写内容进行测试。
![](https://qcloudimg.tencent-cloud.cn/raw/7b871daf7954e72f1c73cc138ccd2d8f.png)
5. 进入 SESSION 设置页面，设置内容为 security = 0123456789……，后继 Web 应用防火墙将把 security 后面10位字符串作为 SESSION 标识，SESSION 信息也可以删除重新配置。
![](https://main.qcloudimg.com/raw/bf87f5f7037e7758d8c281151852ad70.png)
6. 设置基于 SESSION 的 CC 防护策略，配置过程和 [示例二](#two) 保持一致，识别模式选择 SESSION 即可。
>?以 GET 位置为 SESSION 标识设置 CC 规则，当 CC 规则启用后，会把相同的 SESSION 标识作为维度拦截，而不是将 IP 作为维度。
>
![](https://qcloudimg.tencent-cloud.cn/raw/cb1d660ad4edb56ba17edc94115ce831.png)
7. 配置完成，基于 SESSION 的 CC 防护策略生效。
>!使用基于 SESSION 的 CC 防护机制，无法在 IP 封堵状态中查看封堵信息。
