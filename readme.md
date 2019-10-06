# 概览

本文档讲解Auth2的概念

## 1 什么是OAuth 2.0

- 快递员问题

```txt
小区有个门禁系统，进入的时候需要用户名密码。
现在我点了外卖，外卖小哥要进入小区，
现在我可以将用户名和密码告诉外卖小哥，他就有了我的权限。这样好像不太合适。万一我想取消他进入小区的权力，也很麻烦，我自己的密码也得跟着改了，还得通知其他的快递员。

有没有一种办法，让快递员能够自由进入小区，又不必知道小区居民的密码，而且他的唯一权限就是送货，其他需要密码的场合，他都没有权限？

于是我设计了一套用户授权机制

第一步，门禁系统的密码输入器下面，增加一个按钮，叫做"获取授权"。快递员需要首先按这个按钮，去申请授权。
第二步，他按下按钮以后，屋主（也就是我）的手机就会跳出对话框：有人正在要求授权。系统还会显示该快递员的姓名、工号和所属的快递公司。
我确认请求属实，就点击按钮，告诉门禁系统，我同意给予他进入小区的授权。
第三步，门禁系统得到我的确认以后，向快递员显示一个进入小区的令牌（access token）。令牌就是类似密码的一串数字，只在短期内（比如七天）有效。
第四步，快递员向门禁系统输入令牌，进入小区。

有人可能会问，为什么不是远程为快递员开门，而要为他单独生成一个令牌？这是因为快递员可能每天都会来送货，第二天他还可以复用这个令牌。另外，有的小区有多重门禁，快递员可以使用同一个令牌通过它们。
```

- 互联网场景

```doc
上述解决方案其实就已经是OAuth的设计了。
首先，居民小区就是储存用户数据的网络服务。比如，微信储存了我的好友信息，获取这些信息，就必须经过微信的"门禁系统"。
其次，快递员（或者说快递公司）就是第三方应用，想要穿过门禁系统，进入小区。
最后，我就是用户本人，同意授权第三方应用进入小区，获取我的数据。
```

**OAuth 就是一种授权机制。数据的所有者告诉系统，同意授权第三方应用进入系统，获取这些数据。系统从而产生一个短期的进入令牌（token），用来代替密码，供第三方应用使用。**

- 令牌与密码

```txt
令牌（token）与密码（password）的作用是一样的，都可以进入系统，但是有三点差异。
```

- 令牌是短期的，到期后会失效，用户自己无法修改。密码是长期的，用户不修改就不会发生变化
- 令牌可以被数据所有者销毁，会立即失效。以上例而言，屋主可以随时取消快递员的令牌。密码一般不允许被他人撤销。
- 令牌有权限范围(scope)，比如只能进小区的二号门。对于网络服务来说，只读令牌就比读写令牌更安全。密码一般是完整权限。

OAuth 2.0 对于如何颁发令牌的细节，规定得非常详细。具体来说，一共分成四种授权类型（authorization grant），即四种颁发令牌的方式，适用于不同的互联网场景。

## 参考文档

```doc
http://www.ruanyifeng.com/blog/2019/04/oauth_design.html

```