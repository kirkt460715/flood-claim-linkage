# 暴雨损失合并认定与跨片区共担

这些演练台账是本地合成资料，使用虚构编号。CSV 采用 UTF-8 和带表头的逗号分隔格式。同一单元内用竖线列出的值表示多项或候选集合，含义见各表说明。空值通常表示未记录或不适用，明确的零会保留为 0。

## 资料

- `data/rain.csv`  项目认定参考的独立降雨过程。第一个字段的组表示同一份联合保障范围的报告批次。时点为该批次起算日。
  字段为 `group / event / from_day / to_day / segments / known_day`。
- `data/works.csv`  工作单和申报事实。possible_rain 是可相容的过程范围；amount 是承包队原报数量，不是独立鉴定。previous 表示前次施工的已知关系。
  字段为 `work / segment / district / possible_rain / invoice / kind / amount / previous`。
- `data/inspections.csv`  勘损的确证和未决范围。ends 表示可确证的受损点位，未独立确证的金额允许在 low 与 high 之间。
  字段为 `check / work / day / ends / low / high / event_candidates / source`。
- `data/rules.csv`  联合保障协议，各过程分别应用免赔和单过程上限，pool_limit 是同一组共享额度。金额为演练用同一单位。保留额用于公共工程。
  字段为 `group / covered / deductible / event_cap / pool_limit / reserved`。
- `data/vouchers.csv`  正式支付和申报记录。paid 当时占用金额不能自动改写。source_scope 可供核对旧承诺适用的费用与过程。
  字段为 `voucher / invoice / group / district / amount / state / day / source_scope`。

## 核对台账

环境为 Python 3.11.2，只使用标准库。在仓库根目录运行下列命令检查文件格式、表头和基础外键。输出为各张源表可读行数。

```sh
python3 input_audit.py
python3 -m unittest discover -s tests -v
```

可在第一条命令后指定另一个输入目录，目录中的同名 CSV 沿用上述格式。列名和基础引用清单见 `input_contract.json`。
