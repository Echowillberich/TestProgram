# 接口自动化测试框架使用说明

## 1. 框架结构
- **base**：基础类封装，测试用例工具
- **common**：公共方法封装
- **conf**：存放全局配置文件目录
- **data**：存放测试数据路径（仅该目录文件可自由删除，其他目录文件不可删）
- **logs**：存放测试日志目录
- **report**：测试报告生成目录，支持两种形式报告输出
- **testcase**：存放测试用例文件目录
- **venv**：框架专用虚拟环境
- **conftest.py**：全局操作配置，名称为固定写法，不可更改
- **environment.xml**：Allure测试报告环境配置文件（推荐使用，支持中文显示）
- **extract.yaml**：接口依赖参数存储文件
- **pytest.ini**：pytest框架规范约束，名称为固定写法，不可更改（内容禁止添加`#`中文注释）
- **requirements.txt**：框架依赖第三方库清单
- **run.py**：框架主程序入口


## 2. 参数说明
- **url配置**：仅填写接口路径（如`/dar/user/login`），IP和端口需在`conf/config.ini`的`[api_envi]`节点中配置
- **参数传递格式**：`${函数名(*args)}`，其中`args`为可选参数；函数逻辑需在`common/debugtalk.py`中实现（示例：`${get_extract_data(token)}`，用于提取之前接口的token参数）


## 3. 接口参数类型
参数类型仅支持 **params、data、json** 三者之一，需根据接口请求方式和传参类型选择，且对应请求头（Content-Type）需同步匹配：

| 接口场景                | 参数类型 | 对应请求头（Content-Type）                  |
|-------------------------|----------|---------------------------------------------|
| POST请求-表单提交       | data     | application/x-www-form-urlencoded;charset=UTF-8 |
| POST请求-JSON提交       | json     | application/json;charset=UTF-8              |
| GET请求-URL传参         | params   | 无需特殊设置（默认）                        |
| 文件提交（如上传接口）  | files    | multipart/form-data; charset=utf-8          |

> 注：参数类型与请求头必须匹配，否则会导致接口请求异常。


## 4. 注意事项
1. **依赖安装**：执行以下命令安装依赖（需替换为自身项目路径）：  
   ```bash
   pip install -i https://pypi.doubanio.com/simple -r F:\automaticAPI\pythonproject\requirements.txt
   ```
2. **环境选择**：  
   - 优先使用本地Python环境；若用`venv`虚拟环境，出现库版本不兼容时，需卸载报错库后重新安装。  
   - 环境配置参考：[环境配置指南](https://app.yinxiang.com/fx/276ba8ce-63a6-410b-ac1f-509da376181c)
3. **报错处理**：运行报错若为第三方库版本冲突，卸载对应库后重新安装即可。
4. **文件保护**：除`data`目录外，其他目录/文件不可删除，否则框架无法运行。
5. **pytest.ini规则**：名称不可改，内容不可加`#`中文注释；如需加注释，参考：[pytest.ini中文注释指南](https://blog.51cto.com/u_15688254/5391563)
6. **镜像源安装**：第三方库推荐用清华镜像源，示例（安装pandas）：  
   ```bash
   pip install pandas -i https://pypi.tuna.tsinghua.edu.cn/simple/
   ```
7. **部署问题**：环境部署常见错误参考：[部署错误解决指南](https://app.yinxiang.com/fx/a74c2da8-1f76-4757-9467-5392889d0ad7)


## 5. 接口YAML文件自动生成工具
无需手动编写接口YAML文件时，可通过工具生成：  
1. 运行`base/new_testcase_tools.py`文件；  
2. 填入接口参数（如URL、请求方法、入参等）；  
3. 先点击「接口调试」验证接口可用性，再点击「生成yaml文件」。


## 6. 报告环境配置（environment文件）
`environment.xml`和`environment.properties`均为Allure报告环境配置文件，使用规则如下：
1. 格式选择：推荐`environment.xml`（报告支持中文），`environment.properties`易出现中文乱码；  
2. 内容自定义：可按需编写配置，保持格式正确即可；  
3. 防止文件被删：  
   - 执行`pytest --clean-alluredir`（清除历史报告数据）时，会删除报告目录下的`environment`文件；  
   - 解决方案：  
     ① 先将`environment.xml`/`environment.properties`备份到项目根目录；  
     ② 执行`run.py`第一行代码后；  
     ③ 将备份文件复制到Allure报告原始数据目录（`report/temp`）。


## 7. Allure测试报告生成问题
若执行后未生成Allure报告，配置方法参考：[Allure报告配置指南](https://app.yinxiang.com/fx/fd13ff11-369f-4b3b-bac9-c7ea18bd2f47)
