### React + Antd实现动态切换主题功能   
###### 注意事项   
>该功能通过动态修改 CSS Variable 实现，因而在 IE 中页面将无法正常展示。请先确认你的用户环境是否需要支持 IE。
该功能在 antd@4.17.0-alpha.0 版本起支持。   
### 实现原理   
动态切换主题的功能是，通过ConfigProvider全局化配置，设置统一的样式前缀，具体ConfigProvider相关文档可以看这里：https://ant-design.gitee.io/components/config-provider-cn/#API。
举个例子，antd按钮控件，参数type设置为'primary'后，实际渲染出来后，会添加上class="ant-btn ant-btn-primary"的属性，其中ant-btn-primary样式控制按钮的背景色及边框线颜色。如下图：   
![avatar](https://upload-images.jianshu.io/upload_images/8575601-45c330c1b357fa28?imageMogr2/auto-orient/strip|imageView2/2/w/1031/format/webp)   
对于类名ant-btn以及ant-btn-primary，ant实际上作为样式的前缀。而我们如果动态的将样式类名中的前缀统一替换成其他的字符串，就实现了主题切换的效果。   
### 具体实现   
1.依据官方文档，通过ConfigProfider在顶层修改prefixCls：   
```javascript   
import { ConfigProvider } from 'antd';

export default () => (
  <ConfigProvider prefixCls="custom">
    <MyApp />
  </ConfigProvider>
);   
```   
2.由于步骤1中，通过ConfigProfider将样式类名前缀调整为custom，我们需要重新通过lessc命令，将antd的默认样式less文件，重新编译生成一份css文件。※注意：如果未安装less，则需要安装less：npm install -g less   
```     
lessc --js --modify-var="ant-prefix=custom" node_modules/antd/dist/antd.variable.less custom.css       

```       
###### 按照步骤2中的命令，会生成一个custom.css，由于文件过于庞大，截取几段内容如下   
```   
html {
  --custom-primary-color: #1890ff;
  --custom-primary-color-hover: #40a9ff;
  --custom-primary-color-active: #096dd9;
  --custom-primary-color-outline: rgba(24, 144, 255, 0.2);
  --custom-primary-1: #e6f7ff;
  --custom-primary-2: #bae7ff;
  --custom-primary-3: #91d5ff;
  --custom-primary-4: #69c0ff;
  --custom-primary-5: #40a9ff;
  --custom-primary-6: #1890ff;
  --custom-primary-7: #096dd9;
  --custom-primary-color-deprecated-pure: ;
  --custom-primary-color-deprecated-l-35: #cbe6ff;
  --custom-primary-color-deprecated-l-20: #7ec1ff;
  --custom-primary-color-deprecated-t-20: #46a6ff;
  --custom-primary-color-deprecated-t-50: #8cc8ff;
  --custom-primary-color-deprecated-f-12: rgba(24, 144, 255, 0.12);
  --custom-primary-color-active-deprecated-f-30: rgba(230, 247, 255, 0.3);
  --custom-primary-color-active-deprecated-d-02: #dcf4ff;
  /* 以下内容略去 */   
  ```   
  其中第二行中的--custom-primary-color即为primary的样色，默认为antd默认的蓝色，用于测试效果，我们可以将该改色修改为红色：   
  ```   
    html {
    --custom-primary-color: #ff0000;/* 修改为红色 */
    --custom-primary-color-hover: #40a9ff;   
    ```   
    ###### index.js文件引入上述步骤中的custom.css文件。※注意：默认的antd样式文件antd/dist/antd.css也需要引入。index.js文件完整代码如下：   
    ```javascript   
    import React, { useState } from "react";
import ReactDOM from "react-dom";
import { Button, DatePicker, Radio, Space, version } from "antd";
import { ConfigProvider } from "antd";

import "antd/dist/antd.css";  // antd默认样式文件
import "./custom.css";  // 修改后的样式文件
import "./index.css";

const TestComponent = () => {
  const [prefix, setPrefix] = useState("ant");

  const handlePrefixChange = (e) => {
    setPrefix(e.target.value);
  };

  return (
    // ConfigProvider修改prefixCls
    <ConfigProvider prefixCls={prefix}>
      <div className="App">
        <h1>
          <Space>
            Change Theme:
            {/* radio动态修改prefix */}
            <Radio.Group onChange={handlePrefixChange} value={prefix}>
              <Radio value="ant">Ant Style</Radio>
              <Radio value="custom">Custom Style</Radio>
            </Radio.Group>
          </Space>
        </h1>

        <h1>antd version: {version}</h1>
        <DatePicker />
        <Button type="primary" style={{ marginLeft: 8 }}>
          Primary Button
        </Button>
      </div>
    </ConfigProvider>
  );
};

ReactDOM.render(<TestComponent />, document.getElementById("root"));
```   
### 最终效果   
![avater](https://upload-images.jianshu.io/upload_images/8575601-452f7d6fc7f70c3c.gif?imageMogr2/auto-orient/strip|imageView2/2/w/718/format/webp)
