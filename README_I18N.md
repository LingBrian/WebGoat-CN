# WebGoat 多语言支持

WebGoat 默认使用英语，并支持多语言界面。课程内容的翻译覆盖情况因语言和课程而异；未提供译文时会回退到英文原文。

## 语言选择

1. 当前界面支持英语（en）、法语（fr）、德语（de）、荷兰语（nl）、西班牙语（es）和简体中文（zh）。
2. 初次访问时，应用会参考浏览器语言；不支持的语言回退为英语。
3. 登录后可通过主页面的语言下拉菜单切换语言；切换后会回到课程介绍页。
4. 简体中文界面仅表示已有中文翻译的通用界面和课程内容，不代表所有课程均已翻译。

## 添加语言或补充翻译

新增语言时需要：

1. 在 `src/main/resources/webgoat/templates/main_new.html` 的语言菜单中添加入口，并确保 `lang` 参数使用对应的语言代码。
2. 在 `src/main/resources/webgoat/static/css/img` 中添加相应的国旗图标。
3. 为介绍课程添加语言版本，例如 `Introduction_zh.adoc`。建议说明课程中仍有部分内容为英文，并邀请社区协助翻译。
4. 复制 `src/main/resources/i18n/messages.properties` 为 `messages_<语言代码>.properties`，翻译所有面向用户的值，保留属性键不变。
5. 可为课程新增对应语言的 `WebGoatLabels_<语言代码>.properties`，并在该课程的 `documentation` 目录中添加 `<原文件名>_<语言代码>.adoc`。
6. 运行 `./mvnw clean test`，并启动 WebGoat 检查新语言及已有语言是否正常。

课程内容翻译需保留练习的安全目标、漏洞利用路径、提示及完成条件，不应改变课程行为。
