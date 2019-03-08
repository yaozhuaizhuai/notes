# 1. 浏览器中输入相应的网站
# 2. F12查看网页，切换至Security
![](https://agam-blog-image.oss-cn-hangzhou.aliyuncs.com/a75d98c9f7f08bd18f543479208abc56f49.jpg)
# 3. 在上步骤的基础上View certificate，按步骤导出cer文件
![](https://agam-blog-image.oss-cn-hangzhou.aliyuncs.com/95874003db924373505937271facbfeb709.jpg)
# 4. 用JAVA工具将导出的证书转换成.store格式文件
```
keytool -import -alias "mgr.store" -flie "D:\mgr.cer" -keystore mgr.store
```
# 5.jmeter界面Options选择卡SSL Manager选项添加上述证书