# 别名

  早上好, 这里是 ljzn.

  今天是周末, 隔离期还有两天, 也不能出去玩, 就在家里做点好吃的.

  闲着没事翻了翻微博, 看到一篇知乎问答"支付宝错打了 5000 块钱,
  对方不接电话, 怎么办?", 看了一下回答, 目前最好的方法还是要到法院提起诉讼.
  虽然回答里面说诉讼流程并不麻烦, 但是看完还是希望自己永远不要遇到这种情况.

  支付宝的转账其实已经做得比较安全了, 输入对方账号之后,
  可以看到对方的姓名(会隐藏第一个字), 认真确认一下就好.
  想一想如果是在转账比特币的时候输错了地址, 大概率永远都找不回来了,
  很可能这个地址是不属于任何人的. 比特币已经出现 10 年了,
  也有很多钱包产品尝试了各种方案让转账变得更不容易出错,
  比如添加通讯录等等. 不过始终没有一个被大量采用的方案,
  用得最多的还是地址.
  有没有一个开放的协议可以让各个钱包之间互相兼容通讯录?
  还真有.

  这套协议简单来说就是用类似 `linjie@handcash.io` 这样的看起来像邮箱一样的账户来替代地址.
  如何通过这个用户名获取到它对应的比特币地址呢?

  首先要通过 DNS 服务器:

  ```sh
  dig srv _bsvalias._tcp.handcash.io
  ```

  会得到类似这样的结果:

  ```
  handcash-cloud-production.herokuapp.com
  ```

  这个就是钱包服务商 `handcash.io` 的 api 服务地址,
  然后可以通过以下命令询问它支持哪些服务:

  ```
  curl handcash-cloud-production.herokuapp.com/.well-known/bsvalias
  ```

  然后会得到一个 json 结构字符串, 里面包含了其支持的服务的信息. 这一套协议叫做
  paymail 协议. 目前已经有 handcash, simplycash, 和 moneybutton 等钱包支持这一协议.
  虽然使用的人数不多, 但是用起来比地址方便多了.

  顺便说一下, 在 github 上搜 `paymail-inspector` 可以找到一个很棒的调试工具, 支持 paymail
  的很多 capabilities.

  写了点代码测试了一下, 还真的能获取到我在 handcash 钱包里面设置的昵称和头像.

  ![paymail profile](./images/paymail_profile.gif)

  要获取地址的话还有点麻烦, 需要请求的时候附带签名. 类似于说明自己的意图, 才能获取到对方的地址, 确切说是锁定脚本.

  就先写到这吧, 希望更多的钱包能支持 paymail.