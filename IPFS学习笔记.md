# IPFS与区块链

[TOC]



## 第一章 IPFS环境配置

### 1.1 IPFS简介

`IPFS`是一个点对点的分布式超媒体分发协议，它为所有人提供全球统一的可寻址空间，包括`git`、自证明文件系统`SFS`、`BitTorrent`和`DHT`，同时它被认为是最有可能取代HTTP的新一代互联网协议。

`IPFS`与传统的HTTP协议不同，HTTP协议是基于域名寻址，而`IPFS`是基于内容寻址。将一个文件放到`IPFS`节点中，它会产生唯一的加密哈希值。当`IPFS`被请求一个文件哈希时，它会使用一个分布式哈希表找到文件所在的节点，取回文件并验证文件数据。`IPFS`在存储文件时将大文件切分成小块，下载时可以从多个服务器同时获取。

### 1.2 IPFS本地环境安装

- 打开IPFS官网https://ipfs.io/(该网页需要翻墙)，首页如图所示：

![](/Users/joten/区块链技术/相关截图/ipfs1.jpg)

- 进入下载页面下载对应自己操作系统的安装包(实际上进入时会自动识别你的平台)，下载完后对压缩包进行解压即可。为了方面操作，我们将压缩之后文件夹中的ipfs执行文件移动到`/usr/local/bin/`目录下，那我们可以很方便使用ipfs命令：

![](/Users/joten/区块链技术/相关截图/ipfs2.jpeg)

### 1.3 配置IPFS

- 初始化IPFS节点

```
$ ipfs init
```

> 执行完该命令后，会在当前目标生成一个`.ipfs`的文件夹存储节点数据。
>
> `.ipfs`节点默认存储空间为`10G`

- 修改节点默认存储空间

  - 先执行以下两条命令

    ![](/Users/joten/区块链技术/相关截图/ipfs5.jpg)

  - 修改`StorageMax`

    ![](/Users/joten/区块链技术/相关截图/ipfs4.jpg)

- 查看节点ID

![](/Users/joten/区块链技术/相关截图/ipfs6.jpg)

- 在IPFS节点添加文件

![](/Users/joten/区块链技术/相关截图/ipfs7.jpg)

> 可以看出，当我们向IPFS节点添加文件，它会生成一个与文件对应的加密Hash值，我们可以通过Hash才查看其对应文件的内容。

- 对原有文件进行修改

![](/Users/joten/区块链技术/相关截图/ipfs8.jpg)

> 当我们对原有文件进行修改后，我们再用原来的Hash值查看，发现得到的结果依然是之前的内容。只有我们重新上传，再通过新的Hash才能得到修改后的内容。这是因为IPFS生产的Hash值是对文件内容进行存储，因此IPFS生成的是文件内容的Hash值。

- 进行数据同步，启动节点服务器

此时，如果我们通过https://ipfs.io/ipfs/QmZMPjytaVckhUviADT1uwx1VBt64D428mHE1t6cQfa9CT查看文件内容，是无法查看的。因为我们现在只是将数据存储在本地的IPFS节点中，并没有同步到整个网络中。下面是进行数据同步：

![](/Users/joten/区块链技术/相关截图/ipfs9.jpeg)

等待一段时间(可能时间会很长)，我们就可以通过http://ipfs.io/ipfs/$hash的方式在浏览器中访问我们上传的内容：

![](/Users/joten/区块链技术/相关截图/ipfs11.jpg)

- 跨越资源共享CORS配置

为了后面开发调用相关API，我们需要对跨域资源共享进行配置。

```
$ ipfs config --json API.HTTPHeaders.Access-Control-Allow-Methods '["PUT", "GET", "POST", "OPTIONS"]'
$ ipfs config --json API.HTTPHeaders.Access-Control-Allow-Origin '["*"]'
$ ipfs config --json API.HTTPHeaders.Access-Control-Allow-Credential '["true"]'
$ ipfs config --json API.HTTPHeaders.Access-Control-Allow-Headers '["Authorization"]'
$ ipfs config --json API.HTTPHeaders.Access-Control-Allow-Expose-Headers '["Location"]'
```

- 对配置进行验证

先启动我们的IPFS，即在终端执行`ipfs daemon`，然后执行如下命令，得到下面的画面就说明配置好了。

![](/Users/joten/区块链技术/相关截图/ipfs12.jpg)

- 在IPFS上查看一个已存在的web项目

![](/Users/joten/区块链技术/相关截图/ipfs13.jpg)

## 第二章 基于IPFS搭建个人博客

### 2.1 在IPFS对图片文件处理

#### 2.1.1 上传图片

跟上传文件一样通过执行`ipfs add 图片路径`即可。然后执行`ipfs daemon`进行数据同步，最后我们再浏览器中输入`https://ipfs.io/ipfs/上传图片hash值`就能看到我们上传的图片。

![](/Users/joten/区块链技术/相关截图/ipfs14.jpg)

#### 2.1.2 下载图片

我们再终端中执行`ipfs get`命令就可以将图片下载下来，若用`ipfs cat`来查看图片，显示的是一堆乱码。

![](/Users/joten/区块链技术/相关截图/ipfs15.jpg)

### 2.2 在IPFS创建目录存储文件

- 创建文本文件，上传到IPFS

![](/Users/joten/区块链技术/相关截图/ipfs16.jpg)

- 在IPFS节点创建目录

![](/Users/joten/区块链技术/相关截图/ipfs17.jpg)

- 将已上传的文本文件存储到目录

![](/Users/joten/区块链技术/相关截图/ipfs18.jpg)

### 2.3 在IPFS添加一个现有目录

- 在本地创建一个目录wjt
- 将目录上传到IPFS

![](/Users/joten/区块链技术/相关截图/ipfs19.jpg)

- 查看上传目录里的文件

我们可以有三种方式查看目录里的文件，如下图所示：

![](/Users/joten/区块链技术/相关截图/ipfs20.jpg)

> 第一种直接以文件Hash值为参数。
>
> 第二种以路径加Hash值为参数。
>
> 第三种以路径加Hash值先找到目录，再访问目录里的文件。

- 通过浏览器查看目录

先执行`ipfs daemon`同步数据，再在浏览器中输入`https://ipfs.io/ipfs/目录路径`进行访问。

![](/Users/joten/区块链技术/相关截图/ipfs21.jpg)

### 2.4 创建简单的网页上传到IPFS

#### 2.4.1 编写一个简单的网页

创建`index.html`文件。

```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Hello IPFS!</title>
	<link rel="stylesheet" href="./style.css" />
</head>
<body>
	<h1>Hello IPFS!</h1>
</body>
</html>
```

#### 2.4.2 编写一个简单的css文件

创建`style.css`文件。 

```css
h1 {
    color: black;
}
```

#### 2.4.3 上传我们的网页相关文件

- 通过`ipfs add site/`来上传我们的网页相关文件。

![](/Users/joten/区块链技术/相关截图/ipfs22.jpg)

- 在浏览器中输入`https://ipfs.io/ipfs/网页目录Hash`，可以显示出我们的网页(记得同步数据)。

![](/Users/joten/区块链技术/相关截图/ipfs25.jpg)

> 若想要上传博客，我们必定会不断修改博客内容，IPFS是基于内容的存储，因此内容改变，Hash值也会改变，那就会影响我们的正常的访问。我们需要将我们的网页跟我们的IPFS节点绑定在一起。

- 显示IPFS节点ID

![](/Users/joten/区块链技术/相关截图/ipfs23.jpg)

- 将目录Hash绑定到我们的IPFS节点

![](/Users/joten/区块链技术/相关截图/ipfs24.jpg)

> 以上命令参数为需要绑定的目录Hash，输出的结果显示绑定的目标就是我们的IPFS节点ID。

- 用IPNS访问我们的网页

绑定结束后，我们就可以在浏览器中输入`https://ipfs.io/ipns/节点ID`来访问我们的网页。

> 当我们修改页面内容后，需要重新将新的网页上传，并将其目录Hash值与我们IPFS节点绑定。这样的好处是，访问不需要修改访问地址。

