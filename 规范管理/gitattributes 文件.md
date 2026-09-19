# gitattributes 文件

`.gitattributes` 文件用于统一 Git 对仓库中文件的处理规则

# 配置说明

| 配置项 | 值   | 说明                  |
|--------|------|-----------------------|
| *      |      | 匹配仓库中的所有文件  |
| text   | auto | 自动识别文本文件      |
| eol    | lf   | 设置文件的换行符为 LF |

# 配置规则

编辑项目根目录中的 `.gitattributes` 文件：

```text
* text=auto eol=lf
```


