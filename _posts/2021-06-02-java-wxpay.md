---
title: Java项目实现微信支付
date: 2021/06/02
---

最近在整理项目的过程中，发现最早写的微信支付是纯原生的，代码量比较大，刚好近来无事，准备采用WxJava提供的SDK重构一下代码，此文仅将微信支付涉及到的代码抽取出来，仅供参考。

<!-- more -->

## 引入依赖

首先在项目pom文件中使用maven引入依赖(版本可自行选择进行替换),此处采用的是Java SDK是 [WxJava](https://github.com/Wechat-Group/WxJava) 

```
  <dependency>
    <groupId>com.github.binarywang</groupId>
    <artifactId>weixin-java-pay</artifactId>
    <version>4.0.9.B</version>
  </dependency>
```

## 微信支付服务类配置

```java
@Data
@Component
@ConfigurationProperties(prefix = "pay.wechat")
public class WxPayProperties {

    /**
     * 设置微信公众号或者小程序等的appid
     */
    private String appId;

    /**
     * 微信支付商户号
     */
    private String mchId;

    /**
     * 微信支付商户密钥
     */
    private String mchKey;

    /**
     * apiclient_cert.p12文件的绝对路径，或者如果放在项目中，请以classpath:开头指定
     */
    private String keyPath;

}

```

```java
@Data
@Configuration
@ConditionalOnClass(WxPayService.class)
public class WxPayConfiguration {

    @Autowired
    private WxPayProperties properties;

    @Bean
    @ConditionalOnMissingBean
    public WxPayService service() {
        WxPayConfig config = new WxPayConfig();
        config.setAppId(properties.getAppId());
        config.setMchId(properties.getMchId());
        config.setMchKey(properties.getMchKey());
        config.setKeyPath(properties.getKeyPath());

        WxPayService service = new WxPayServiceImpl();
        service.setConfig(config);
        return service;
    }

}

```

## 配置文件(yml)

```yml
pay:
  wechat:
    appId: #微信公众号或者小程序等的appid
    mchId: #微信支付商户号
    mchKey: #微信支付商户密钥
    keyPath: # p12证书的位置，可以指定绝对路径，也可以指定类路径（以classpath:开头）
```

## 微信统一下单接口开发

[统一下单](https://pay.weixin.qq.com/wiki/doc/api/jsapi.php?chapter=9_1) 的作用是在微信支付服务后台生成预支付交易单,如果预支付交易返回的会话标识是正确的话,APP就可以拉起微信支付

```java
@Slf4j
@RestController
@RequestMapping("/pay")
public class PayController {

    @Autowired
    private WxPayService payWxService;

    /**
     * 调用统一下单接口，并组装生成支付所需参数对象
     *
     * @param request
     * @return
     */
    @GetMapping("/wx/createOrder")
    public Object wxPayCreateOrder(HttpServletRequest request) {
        String openId = request.getParameter("openId");
        String orderNo = UUID.randomUUID().toString().replace("-", "");
        try {
            // 构建预下订单请求
            WxPayUnifiedOrderRequest orderRequest = new WxPayUnifiedOrderRequest();
            orderRequest.setBody("话费充值");
            orderRequest.setOutTradeNo(orderNo);
            // 元转为分
            orderRequest.setTotalFee(BaseWxPayRequest.yuanToFen("99.99"));
            orderRequest.setSpbillCreateIp("192.168.0.99");
            orderRequest.setNotifyUrl("https://lyusantu.github.io/pay/wx/notify");
            orderRequest.setTradeType(WxPayConstants.TradeType.APP);
            // 执行预下订单
            return payWxService.createOrder(orderRequest);
        } catch (Exception e) {
            log.error("预下订单失败！订单号：{}, 原因: {}", orderNo, e.getMessage());
            e.printStackTrace();
            return e.getMessage();
        }

    }

}
```

## 统一下单接口调用

执行日志如下(此处因数据涉密,对真实数据进行了处理,但是与真实数据的格式完全一致)

请求数据

```xml
<xml>
  <appid>wx5232bc0bf666666b</appid>
  <mch_id>1111111111</mch_id>
  <nonce_str>1111111111111</nonce_str>
  <sign>7AABB76ACC108300E289F4434D77D7C0</sign>
  <body>话费充值</body>
  <out_trade_no>34adc69d5d3848ae8a924368717aad42</out_trade_no>
  <total_fee>9999</total_fee>
  <spbill_create_ip>192.168.0.99</spbill_create_ip>
  <notify_url>https://lyusantu.github.io/pay/wx/notify</notify_url>
  <trade_type>APP</trade_type>
</xml>
```

响应数据

```xml
<xml>
  <return_code><![CDATA[SUCCESS]]></return_code>
  <return_msg><![CDATA[OK]]></return_msg>
  <result_code><![CDATA[SUCCESS]]></result_code>
  <mch_id><![CDATA[1111111111]]></mch_id>
  <appid><![CDATA[wx5232bc0bf666666b]]></appid>
  <nonce_str><![CDATA[0yPYJCvl5MdkAbnS]]></nonce_str>
  <sign><![CDATA[ABAB14611803DSFBV39DF345F4E17DAB]]></sign>
  <prepay_id><![CDATA[wx02155756166818b065abc6tr60a5fd0000]]></prepay_id>
  <trade_type><![CDATA[APP]]></trade_type>
</xml>
```

最终返回JSON
```json
{
  sign: "ABAB14611803DSFBV39DF345F4E17DAB",
  prepayId: "wx02155756166818b065abc6tr60a5fd0000",
  partnerId: "1111111111",
  appId: "wx5232bc0bf666666b",
  packageValue: "Sign=WXPay",
  timeStamp: "1622620676",
  nonceStr: "0yPYJCvl5MdkAbnS"
}
```

IOS真机调用统一下单接口后的页面

![ios](https://lyusantu.github.io/images/wxpay.png)

到此,预下订单接口已经测试成功,APP在调用接口后可成功拉起微信支付,在用户支付成功后微信会访问我们在预下订单接口内配置的notify_url

## 结束语

:)

