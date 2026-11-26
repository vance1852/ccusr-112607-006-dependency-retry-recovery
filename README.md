# 共享依赖的重试隔离与受控恢复

这里的设备、合作方及业务记录均为合成资料。两份记录分别从同一清册开始，可独立读取。记录包含申请、通知及实际观察，申请可能未获准。

## 资料字段

`records/inventory.json` 是初始清册。两份 `.jsonl` 文件均以每行一个 JSON 对象保存观察。`at` 是记录进入值班侧的演练秒，同一秒按原文件顺序。`kind` 标识记录类别。另有字段时，业务发生时间由记录自身给出。

routes 按调用顺序列依赖。retry_pool 和 probe_slots 是同一 scope 所有入口的额度，inflight 包括结果未明的已发请求。attempt_received 为依赖端观测，result 为完成或取消。failure_window 及恢复覆盖以 scope 中的版本和类别区分。fallback_routes 只提供声明的业务保证。

## 运行

在仓库根目录使用 Python 3.11 标准库。

```sh
python3 source_reader.py records/shift.jsonl
python3 source_reader.py records/comparison.jsonl
python3 -m unittest discover -s tests -v
```

读取工具按记录到达顺序显示原始资料，`--kind` 可按类别筛选。
