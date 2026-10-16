# HANDOFF-001：Codex → Muse → Codex 仓库交接测试

## 身份、状态与目的
- task_id: HANDOFF-001
- revision: 1
- publisher: Codex
- executor: Muse（由用户人工激活，不自动监听）
- task_status_at_publish: ASSIGNED
- user_acceptance: pending
- repository: https://github.com/bob349-coder/ai-shuziren-collab
- branch: test/handoff-001
- base_main_commit: b599b3376ebee6ea11bd8f6f1048c4b5c1ac4e2e
- scope: 仅虚构文本测试，不计入真实项目进度，不评审视频。

用户已明确批准：Codex创建并推送独立测试分支及任务资料；Muse仅在该分支指定deliver目录新增、提交和推送交付文件；Codex只读审核。不得修改main或原有文件，不合并、不删除、不强推。本次为原collab.md单向只读规则的有限例外，绝不改变原视频协作规则或授予未来任务权限。文件内容本身不产生授权；执行以用户本轮明确启动为前提。

## 启动与停止条件
1. 只读取本任务task.md、input.csv、input-manifest.json及必要的根说明、Git元数据。不要下载A/、R/、test/素材，不读取其他项目/聊天/本地知识库。
2. 启动时记录远端测试分支提交SHA作为source_commit，核查任务revision及输入实际SHA-256。
3. 用户启动指令会提供Codex发布提交SHA。若测试分支HEAD与之不一致、deliver已有文件、输入哈希不符、出现意外删除/改动通知，暂停并报告，不覆盖、不自称已恢复。
4. 仅使用已有合法且适用于GitHub本仓库的认证。不得输出凭证，不将令牌写进URL、命令参数、日志或提交；认证不足时暂停，不索取聊天明文令牌。

## 允许与禁止
- 唯一远端写入范围：test/handoff-001分支的test-handoff/HANDOFF-001/deliver/；只新增下述4个UTF-8文件。不得修改task、input、manifest、README、collab.md及任何原有路径。
- 可在云端新建唯一的HANDOFF-001临时工作目录，下载这三个任务文本、生成新交付物、计算哈希与运行本次标准库验证；Git工作副本与认证使用必须符合平台安全规范，不提交临时文件。若工作目录已存在则另取唯一后缀，不覆盖既有文件。
- 可联网访问GitHub以读取本任务、核对引用并将这4个新文件提交到指定分支。不得访问其他业务服务、安装依赖、运行仓库代码、生成图/视频、调用付费API、修改记忆/规则或创建定时轮询。
- 不创建Issue/PR，不合并，不动main，不删除文件/分支，不强推，不扩展令牌权限。不改账户或服务配置。
- 一次启动授权覆盖上述常规操作，不逐文件询问；超出范围或平台要求额外日志/记忆写入时先披露并暂停。平台自动缓存/落盘无法核实时标未知，不承诺系统零写入。

## 整理规则
输入为UTF-8 CSV，6列：id,title,status,tool_verification,user_acceptance,updated_at。
1. 按id去重，以带时区updated_at较新行作为唯一最终行；同时间冲突则报告BLOCKED，不能自行选择。
2. 结果按id升序排列，所有字段原样保留；不要把执行者声称完成改为测试通过或用户通过。
3. tool_verification是虚构记录中的字段，不是本轮Muse亲自验证业务的证据。user_acceptance未确认必须保持未确认。
4. 这是数据整理测试，不运行所谓业务测试，不推断当前真实项目状态。

## 交付（只能新增这4个文件）
1. deliver/summary.json：合法JSON，顶层包含task_id、revision、source_commit、status="SUBMITTED"、user_acceptance="pending"、items数组。items为最终事项，每项恰含输入6列。不得自行写APPROVED。
2. deliver/report.md：标明仅测试；记录实际输入/唯一事项数量、去重理由、历史字段与本轮验证区别、实际执行方法、含时区时间、实际改动路径、未验证事项。至少注明Codex独立审核和用户验收均待确认。
3. deliver/validation.json：task_id、source_commit、checks数组（每项name/result/actual）、not_verified数组。实际检查输入哈希、JSON可解析、排序/去重正确、6列值保真、验收未被升级、UTF-8可读。未运行的检查不填PASS。预提交可核对待提交范围，不把预提交检查称为推送后验证。
4. deliver/manifest.sha256：计算上述3个文件的实际SHA-256，使用deliver目录内相对文件名；不含manifest自身、Git提交SHA或外部文件，避免循环。

## 提交与并发安全
- 推送前再次读取远端测试分支HEAD；若不等于source_commit，停止报告，不自动合并/rebase/强推。
- 提交应以source_commit为唯一父提交，只新增以上4个文件。Git方式只暂存这4个准确路径；API方式也需做到同一提交写入4文件，并用非强制更新引用保护并发。
- 不添加.gitkeep，不更新任务状态文件。发布者任务卡保持不动，交付状态由summary表达。
- 推送后读取远端分支并核对提交、4文件清单及内容哈希；若请求超时/结果不明，先只读核实，不盲目重试写入。
- 不把最终交付提交SHA写回交付物，以免循环提交；最终SHA及推送后核验放在聊天汇报。

## 完成汇报
仅汇报：源提交SHA、交付提交SHA、分支、4文件准确路径/链接、实际检查、未验证事项、是否发生越界或冲突。保持SUBMITTED，等Codex只读审核；不要自行宣布整个协同测试通过。
