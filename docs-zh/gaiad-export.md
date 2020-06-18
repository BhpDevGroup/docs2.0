## 导出区块状态
### 用法
```shell script
gaiad export <flags>
```
### 标识
|  名称   | 类型  | 必须  | 默认值  | 描述  |
|  ----  | ----  | ----  |----  | ----  |
| --for-zero-height  | bool |  | false | 导出数据之前做一些清理性的工作，如果不想以导出的数据启动一条新链，可以不加这个标识 |
| --height  | uint |    | -1 | 从指定的高度导出，默认值为`-1`表示导出当前高度状态 |
|--jail-whitelist | strings |    |  | 列出见证人列表 |
### 示例
- 导出当前的区块链状态
```shell script
gaiad export --home=<path-to-your-home>
```
