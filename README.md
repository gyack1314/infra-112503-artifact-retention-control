# 短片衍生素材的共享保留与回收协调

媒体内容以实际摘要标记，读取和删除都由本地节点提供事实。模型以 TTL 秒点表达许可边界。

## 输入消息

每个 JSONL 文件是一段独立输入记录。每行包括整数秒点 at、消息名称 kind 与对象 record。at 代表中间件收到消息的时刻，同一秒内按行序提交。业务时间和对应回执引用写在 record 中。

- **object**　asset、digest、revision 与 deps。deps 引用其他 asset。
- **pin**　节目 program 发布 rev 对 roots 的引用，region 是该发布依赖可读副本的区域。
- **copy**　节点报告某 region、asset、node 的状态 copying、readable 或 deleted。
- **read**　播放请求引用 program/rev，申请区域内根素材的阅读许可证。
- **open_read**　源 node 证明读取许可 lease 已开始。
- **read_done**　源 node 证明指定 lease 已结束。
- **reclaim**　值班台请求删除指定 node/asset。
- **delete_ack**　源 node 回报删除 locked 或 deleted。
- **restart**　协调器重启。
- **inspect**　查看 asset 或 program。
- **unpin**　撤销明确的节目引用。

## 示例

- shared-dependency-and-reader.jsonl
- reader-meets-delete-lock.jsonl

## 读取演练资料

运行环境为 Python 3.11，只使用标准库。在根目录运行以下命令可核对文件格式及读取接口。

    python3 scenario_reader.py examples
    python3 -m unittest discover -v

资料读取器会按文件统计各类参与方消息的数量。演练记录只描述各参与方交给系统的输入。
