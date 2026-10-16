# HANDOFF-001 交付报告（测试）

> 仅为 Codex → Muse → Codex 仓库交接流程测试，不计入真实项目进度，不评审视频。
> Codex 独立审核与用户验收均待确认（user_acceptance=pending）。

- task_id: HANDOFF-001（revision 1，publisher: Codex）
- source_commit: 793fe0a05ca3a30c3a538bc55fb6ab53d8f1483c（远端分支 test/handoff-001 在启动时核对一致）
- status: SUBMITTED

## 输入与去重

- 输入 input.csv：UTF-8，6 列（id,title,status,tool_verification,user_acceptance,updated_at），除表头 4 行。
- 输入哈希：SHA-256 = 6f5aad849d20d0f5f963e3df7c869bc96d3f6eb9eeca58890cdb36fe6affb21e，与 input-manifest.json 声明一致。
- 去重规则：按 id 去重，取带时区 updated_at 较新行；同时间冲突则报告 BLOCKED（本次未发生）。
- SIM-01 有两行：2026-10-15T09:00:00+08:00（进行中）与 2026-10-16T11:00:00+08:00（已提交），保留后者。
- 最终 3 个唯一事项，按 id 升序：SIM-01（已提交）、SIM-02（执行者声称完成）、SIM-03（待开始）。

## 历史字段与本轮验证的区别

- `tool_verification` 列是输入数据自带的虚构记录字段（如"静态格式检查通过"/"未运行"），不是本轮 Muse 亲自验证业务的证据。
- `status` 列同样来自输入原样保留：SIM-02 的"执行者声称完成"未被改写为"测试通过"。
- `user_acceptance` 全程保持"未确认"，summary.json 中记为 pending，未写 APPROVED。
- 本轮实际执行的验证见 validation.json 的 checks 数组；未运行的检查未填 PASS，列在 not_verified。

## 实际执行方法

- 云端临时工作目录 /tmp/handoff-001-work（唯一新建）；仅下载 task.md、input.csv、input-manifest.json 三个任务文本。
- 标准库处理：Python csv/json/hashlib 解析、时区感知时间比较去重、SHA-256 计算；未安装依赖、未运行仓库代码、未生成图/视频、未调用付费 API。
- 含时区时间：所有 updated_at 均为 +08:00，去重比较使用时区感知解析。
- 实际改动路径（本次提交唯一新增）：test-handoff/HANDOFF-001/deliver/ 下 summary.json、report.md、validation.json、manifest.sha256。未修改任务书、输入、manifest、README、collab.md 及任何原有路径；未动 main。

## 未验证事项

- 推送后远端验证（推送前编写本报告；推送后将只读核对提交 SHA 与文件哈希）。
- Codex 独立审核、用户验收：均待确认。
- main 分支保护规则：查询需认证，本轮未动用凭证查询。
- 视频素材完整性：按任务书要求未下载 A/、R/、test/。
