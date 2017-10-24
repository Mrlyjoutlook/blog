# react-native ~ development
记录react native开发过程的种姿势和bug收集

欢迎反馈和bug提供

## tools
- react-native-cli，官方推出创建项目的工具（自带Watchman依赖，不行另行安装）。
- create-react-native-app，是由Facebook和Expo联合开发用于快速创建项目的工具，特点无需用户再按装adroid sdk和xcode。需要搭配Expo开发的工具xde客户端使用。
- Node。
- Yarn，包管理器，如同Npm。
- Nuclide，集成集成环境，可用于编写，运行和测试应用。
- Watchman，监视文件系统变更工具。

> 开发搭配

- 官方：react-native-cli（创建项目），Yarn（包管理器），Watchman（文件变更工具），flow，Node（环境）。
- expo：create-react-native-app（创建项目），Expo XDE（命令行工具与发布工具，同时支持使用内部模拟器），Expo（手机预览app），Exp（node，编译，可选择）。
- 开发and打包工具：ios（Xcode,simulation），android（andorid-sdk,android-ndk,java,geymotion）。
- 编辑器：vscode（推荐），vscode-react-native-tool（集成插件）。

## vscode

- vscode-react-native-tool，提供了一个开发环境，调试，运行，react-native相关操作命令。
- path Intellisens 文件路径提示补全。
- Auto Close Tag 自动闭合标签。
- Auto Rename Tag 自动重命名标签。
- react-native-react-redux-snippets-for-es6-es7 代码片段。
- vscode-language-babel 语法高亮。
- vscode-react-native-tool的salsa代码提示功能未能启用（估计操作姿势有问题），使用typescript，typings代替使用。

```
npm install typescript@next -g
// 先全局安装typescript，如何修改vscode配置，手动指定路径。
// --> "typescript.tsdk"：typescirpt安装的路径
npm install typings --global
// 全局安装typings, 智能提示功能
typings install dt~react-native --save
// 在项目的根目录运行
// end 重启
```

## debug

### run ios

- 预先编译要对其开发的平台，如ios就需要xcode上编译并在设备或者模拟器运行。后续开发不需要再次编译，可以直接类似开发运行开发，除了对ios相关有代码功能才需要编译。
- 使用vscode reactnative-tools插件run ios运行模拟器上，前提电脑需要安装xcode。

### debugger

- npm start运行下在模拟器command + d，选择Debug JS Remotely开启debug功能，在Chrome上debugger调试。
- 使用vscode reactnative-tools插件start packager运行下，在vscode debugger功能选项下添加配置react native: debug ios 或者 react native: debug android，选择对应的平台，点击进行调试。如何在模拟器command + d，选择Debug JS Remotely开启debug功能。（推荐，比较方便直接在代码上打断点）。

### redux tools

使用redux-devTools工具调试redux。
app开发和web开发在使用redux-devTools工具上是不一样，需要远程支持的。安装`remote-edux-devTools`，`remotedev-server`，建议在package.json上添加“remotedev --hostname=localhost --port=5678”次命令，方便快速启动，另外代码添加  
```
...
import { composeWithDevTools } from 'remote-redux-devtools';
...

...
const composeEnhancers = composeWithDevTools({ realtime: true, port: 5678, hostname: 'localhost' });
...

...
createStore(reducers,initialState, composeEnhancers());
...
```
其中代码中的5678端口必须和添加script命令中的端口配置一致。
启动redux-devTools的remote功能，把端口等配置输入即可。

## components

- [react-navigation
]()
- [react-native-scrollable-tab-view
]()
- [react-native-vector-icons]()
- [react-native-splash-screen]()

## bug
1. react-native 0.47.1，使用redux-saga，需要修改babel-preset-react-native文件/config/main.js中'babel-preset-transform-regenerator'，建议注释掉。