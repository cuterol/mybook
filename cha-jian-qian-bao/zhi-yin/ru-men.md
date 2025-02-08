# 入门

你可以选择引用第三方Web3登录库来快速支持 DOMWALLET钱包，比如：web3-onboard ([https://github.com/blocknative/web3-onboard](https://github.com/blocknative/web3-onboard))

或者根据以下的步骤完成DOMWALLET Extension的接入

要用 DOMWALLET Extension 进行开发，请在您的开发设备上选择的浏览器中安装 DOMWALLET Extension。[在这里下载](https://extension.tokenpocket.pro/#/)

一旦 DOMWALLET Extension 安装并运行（确保备份您的助记词或私钥），您应该会发现新的浏览器选项卡`window.ethereum`在开发者控制台中有一个可用的对象。这就是您的网站与 DOMWALLET Extension 交互的方式。

您可以在此处查看该对象的完整 API 。

#### 检测浏览器运行 DOMWALLET Extension <a href="#jian-ce-liu-lan-qi-yun-xing-tokenpocket-extension" id="jian-ce-liu-lan-qi-yun-xing-tokenpocket-extension"></a>

您可以在浏览器的控制台使用以下代码来验证你的浏览器是否正在运行 DOMWALLET Extension。

Copy

```
if (typeof window.ethereum.isDOMWALLET !== 'undefined') {
  console.log('DOMWALLET Extension is installed!');
}
```

#### 检测 DOMWALLET Extension <a href="#detecting-metamask" id="detecting-metamask"></a>

您可以使用 `ethereum.is`DOMWALLET 来区分其他兼容以太坊的浏览器。

#### 连接到 DOMWALLET Extension <a href="#lian-jie-dao-tokenpocket-extension" id="lian-jie-dao-tokenpocket-extension"></a>

“连接”或“登录”到 DOMWALLET Extension 实际上意味着“访问用户的以太坊帐户”。

您可以通过单击按钮来发起请求响应用户的操作，在请求还未有响应的时候禁用此按钮。不要在页面加载的时候发起连接请求。单击按钮调用以下方法：

Copy

```
ethereum.request({ method: 'eth_requestAccounts' });
```

**例子**

HTMLJavaScriptCopy

```
<button class="enableEthereumButton">Enable Ethereum</button>
```

此`Promise`返回的是一个数组，里面包含的是以十六进制为前缀的以太坊地址。

该方法后续会包含各种附加参数，以帮助您的站点在设置期间向用户请求所需的一切。

由于它返回的是一个`Promise`，所以如果你在一个`async`函数中，你可以像下面的代码那样写：

Copy

```
const accounts = await ethereum.request({ method: 'eth_requestAccounts' });
const account = accounts[0];
// We currently only ever provide a single account,
// but the array gives us some room to grow.
```

**例子**

HTMLJavaScriptCopy

```
<button class="enableEthereumButton">Enable Ethereum</button>
<h2>Account: <span class="showAccount"></span></h2>
```
