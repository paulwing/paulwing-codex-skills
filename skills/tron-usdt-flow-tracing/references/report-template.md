# Report Template

Match the user's language. Use this as structure, not fixed wording.

## English

```text
Summary
The initial transaction is confirmed on TRON. The reported funds moved from <source> to <recipient>, then through <intermediate addresses>. The most actionable endpoint identified is <exchange/service label> at <address>. Because the funds entered active account-model addresses, downstream attribution after <hop> should be treated as highly related rather than unit-level proof.

Key Flow
| Step | Time | Amount | From | To | Tx | Confidence |
|---|---:|---:|---|---|---|---|

Mermaid
<flowchart>

Recommended Actions
1. Preserve the original exchange withdrawal or wallet transfer record.
2. File a report with the listed transaction hashes and addresses.
3. Contact the labeled exchange/service compliance team with the evidence bundle.
4. Do not share seed phrases, private keys, 2FA codes, or pay recovery scammers.
```

## Chinese

```text
摘要
初始交易已在 TRON 链上确认。用户报告的资金从 <source> 转入 <recipient>，随后经过 <intermediate addresses>。当前最有操作价值的终点是 <exchange/service label>：<address>。由于资金进入了活跃账户模型地址，<hop> 之后的下游流向应表述为“高度相关”，不能表述为逐枚 USDT 的确定归属。

关键流向
| 步骤 | 时间 | 金额 | 转出 | 转入 | 交易哈希 | 证据强度 |
|---|---:|---:|---|---|---|---|

Mermaid 图
<flowchart>

建议动作
1. 保留原交易所提现记录或钱包转账记录。
2. 报警时提交上述交易哈希、地址和链上截图/链接。
3. 如果流向交易所或桥，向对应平台风控/合规渠道提交证据包。
4. 不要提供助记词、私钥、验证码，也不要相信付费追回资金的人。
```

## Mermaid Style

Use short node labels and full hashes in the table. Put shortened hashes in the diagram.

Use:

- `-->` for confirmed direct transfers
- `-.->` for mixed or confidence-limited downstream flows
- exchange class for public exchange labels
- suspect class for the user-reported scam recipient
- mixer class for active hubs or aggregation addresses
