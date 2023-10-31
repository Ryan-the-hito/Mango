# 🥭Mango: Emergent Email Alarmer

![Y5Xt1FR](https://i.imgur.com/UoJvo2c.png)

<a href="https://twitter.com/ryanswindows">Twitter</a> | <a href="https://weibo.com/ryanthehitos">Weibo</a></p>

Mango is a macOS menu bar app to check the unread emails periodically and set a reminder and an alarm for those coming from target addresses. Mango 是一个任务栏软件，可以帮你给原生的 Mail 软件增加特定未读邮件强提醒的软件。

## 解决问题

### 他人需求

写 Mango 的起因是一天在 V2EX 上看到了[这个帖子](https://www.v2ex.com/t/985824#reply7)，有朋友问有没有方法让邮箱收到一些特定的邮件之后给他打电话，这样就不会错过重要信息了。毕竟有时候沉浸在眼前的事情里，确实很难注意到有未读邮件。

![1](https://i.imgur.com/QibQWV1.png)

况且，有时候你的工作非常 HARD，来件量非常大，而你只需要关注其中一部分最重要的软件。怎么办呢？总不能一直盯着收件箱吧？

![2](https://github.com/Ryan-the-hito/Mango/blob/main/pics/CleanShot%202023-10-31%20at%2009.59.35-trimmed%20-%2001.gif?raw=true)

但是 macOS 自带的邮件和 Spark 好像都没有强提醒的功能，也没有针对某一个邮件地址的强提醒功能，所以做一个这样的软件很合理。

### 我也有需求

我自己也想要一个，感觉可以解放一些我的注意力，让我更集中在手头的事情上。另外，写这种小工具也确实是我喜欢和擅长的事情。

### 更广泛的用途

除了这些生活和工作上的需求，我想这也可以适用于正在找工作和关注目标单位邮件的朋友。把目标邮件地址（甚至不需要全部写完，只需要其中部分）填进去，当有未读邮件时自动提醒，岂不美哉。

## 功能亮点

![3](https://i.imgur.com/N14lIer.png)

Mango 没有主界面，出常规项之外只有设置界面。

### 定时检查未读邮件

首先，用户可以在 Mango 里设置多久检查一次 Mail 里的未读邮件，默认是 15 分钟一次。

### 加入需要提醒的邮件

下面的文本框可供用户输入关注的邮件地址，一行一个。例如我关注苹果的新闻，我就写上 newsdigest@insideapple.apple.com。当然，如果我不知道苹果具体用什么邮件地址给我发新闻，怎么办？我也可以只写一个“apple”，只要这个词语包含在对象邮件地址中，Mango 就能命中所需的邮件。（注意大小写哦）

### 多平台命令适配

由于 Mango 使用 Applescript，在不同的语言界面中，对应软件的名称可能不一样。但是这一点我没有亲自测试过，所以我增加了这个自定义项，如果有用户发现在非英语界面无法正确触发操作，可以考虑一下，是不是自己平台上软件名称是用其他语言写的，因此在代码中无法被识别？如果真的遇到这种情况，只需要把对应的名称用对象语言写在接下来的两个文本框中保存即可。Mango 只需要调用 Shortcuts 和 Mail 两个软件。

### 每次检查前重启 Mail 并强制获取最新邮件

在软件测试的前期，我发现 Mail 存在一个奇怪的现象：当 Mail 在后台长时间开启时，它会无法因无法发起网络请求而陷入无穷循环，导致电脑高负载和耗电。我向几位朋友询问了这个情况，有的朋友也有类似的经历。我反思了一下，大概有三个猜想：

1. 网络不好；
2. 网络运营商切断了 IMTP/SMTP 的端口，因此邮件连不上服务器；
3. 软件自身的 bug；
4. macOS 系统与特定邮箱服务商匹配问题。

![4](https://i.imgur.com/CTl15Dm.png)

![5](https://i.imgur.com/Aq3BAnQ.png)

其中，我换了数个网络线路，包括声明不限制端口的，也存在该问题。因此可能不是 1 和 2 的原因。至于 4，有知乎上的帖子说去掉网易的邮箱就能好转，我试了，似乎一开始是这样，但是很快发现问题没有解决。在使用三四个小时后问题依旧。因此我猜测可能是软件的问题，也有可能是 Mail 与其他软件发生冲突导致。

那么怎么办呢？如果 Mail 不能常驻后台，Mango 就无法稳定读取信息了。为此，我想出了一个解决方案，就是在每次检查邮件前都退出邮箱 app，然后重启软件，强制获取最新邮件。因为我发现，虽然连续用几个小时后邮箱软件容易出问题，但是每次重启之后都会恢复如初。如此，假设我每隔 15 分钟获取一次未读邮件，那么我的 Mail 软件就会每隔 15 分钟重启并获取一次最新数据。这样不仅不耗电，反而大大减少了后台耗电量。Mango 自己也不耗电。

![6](https://i.imgur.com/0Fxv9pT.png)

![9](https://i.imgur.com/siHbfid.png)

另外一点值得一提的是，Mango 在几轮试验下来，找到了完全后台启动 Mail 的方案，用户最多只会看到 Dock 里 Mail 弹动一下，并不会有其他任何感知。这样就既解决了后台常驻问题，也解决了异常耗电问题。Mango 默认开启这一选项。如果你的电脑上没有这个问题，可以自行关闭。

### 当特定邮件来的时候弹出提醒、设置闹钟、跨平台设置提醒

这是 Mango 最核心的功能。首先，当一个特定邮件地址发来邮件且被 Mango 发现后，Mango 会弹出 Notification（会在右上角显示）。接着，Mango 会使用 Shortcuts 来设置两个提醒项，一个是闹钟，另一个是提醒。闹钟是 macOS13 上才有的功能。Mango 会设置一个 5 分钟后的闹钟，提醒用户查看。同样地，此时 Mango 还会同时设置一个 5 分钟后的提醒，届时用户不仅可以在 Mac 上看到提醒，还可以在 iPhone、iPad 以及 AppleWatch 上同步收到提示。因此，如果用户此刻并没有在电脑前，Mango 也能通过跨平台同步消息提醒到用户。当然，这一切都有赖于 Shortcuts 脚本，用户也可以在给定脚本中自定义闹钟提醒的间隔时长，以及有何种方式呈现提醒。如果修改了脚本的名称，记得在设置的最后一项填入修改后的名称，保存即可。

<p align="center">
  <img src="https://i.imgur.com/2BSv0nr.png" width=300 />
  <img src="https://i.imgur.com/xzkFFaY.png" width=300 />
</p>

### 隐私保护，开源软件，本地运行

一般来说，这样的工具是第三方写的，会有安全顾虑，如果软件获取我的邮件内容怎么办？不过 Mango 是一个纯本地软件，除了检查更新需要联网，核心功能断网也能用。因为 Mango 不需要联通任何 api，而是通过 Applescript 去获取信息的。这些信息也完全储存在本地，没有任何上传。Mango 是一个开源软件，如果有需要，用户也可以查看其中的代码。

## 环境要求

- MacOS 12 以上（测试环境为 MacOS 13.6.1），需要使用时钟功能需 13 以上；
- M1、M2、M3 芯片；
- 需要与原生 Mail 联动使用。其实 Mango 可以在每次调用 Mail 之前启动 Mail，但是它不会自动退出，因此如果不想 Mail 常驻后台的话，在 Mango 检查完可自行手动关闭 Mail，Mango 会在下次检测时启动。如果嫌麻烦，可以一直挂在后台，并不耗电。

## 类型价目

免费。

## 下载安装

在 Release 里面找到最新版下载，解压后把软件拖入 Application 文件夹。

## 使用说明

### 打开时

1. 如果出现打不开的情况，说软件损坏，请查一下搜索引擎，这是一个非常容易解决的问题。因为没有苹果的证书，所以它会显示这样的提示。要绕过检查只需在终端里输入“sudo xattr -r -d com.apple.quarantine /Applications/Mango.app”，按提示输入开机密码即可。
2. 确保软件已经可以打开，去 System Settings 里面，在 Privacy & Security 下面的 Accessibility，把 Mango 添加进来。
3. 打开会跳出这样的提示，请允许。
   ![8](https://i.imgur.com/CaiJ0Zd.png)
4. 先在 Menu Bar 里找到 Mango 的图标，点击，进入 Settings，按照上面的内容自定义设置、保存。然后再回到第一项，点击“Switch on Mango!”，即可开始使用。

### 更新时

Mango 在用户打开 Settings 界面的时候会自动检测更新，检测更新需要用户的网络能够连接上 GitHub。如果存在新版本，会自动跳出更新提示。用户也可以手动地点击更新栏，效果是一样的。更新需要用户从 GitHub 或者百度网盘下载，更新后请务必在 Accessibility 里面重新给 Mango 权限（从列表里去掉重新添加一次）。

## 注意事项

macOS 可能会有杀后台的行为，即一个循环程序在后台达到一定次数之后，可能被系统强制终止。因此用户可在前几次关注一下，自己电脑上有没有类似的事情发生。这个问题我现在不知道有什么办法能根治，因为似乎是系统层面的设定。如果有，那么不要把 Mango 挂载太久，隔一天可以重新启动一次。没有任何一个软件能保证万无一失地获取信息，最重要的还是得自己上心。Mango 也许能省下 80% 的精力，但依然还有 20% 的精力需要花在关注它是否正常工作。有提醒终究是比没提醒好一些。

## 证书信息

GPL-3.0 license

## 特别致谢

1. [Qt](https://github.com/qt)：本软件遵循 Qt 的开源协议。

## 支持作者

[Buy Me a Cup of Coffee](https://www.buymeacoffee.com/ryanthehito)

<p align="center">
  <img src="https://i.imgur.com/OHHJD4y.png" width=240 />
  <img src="https://i.imgur.com/6XiKMAK.png" width=240 />
</p>
