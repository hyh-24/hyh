---
name: push-to-github
description: 将当前项目全部变更 git add、commit 并 force push 到 https://github.com/hyh-24/hyh.git，使用已配置的 SSH 公钥认证。当用户要求提交代码到 GitHub、push、强推或同步项目到远程仓库时使用。不用于推送到其他远程仓库。
---

# Push to GitHub

把当前项目（c:\Users\HYH\Desktop\网页设计\index）的全部变更提交并强制推送到 GitHub 远程仓库 hyh-24/hyh。

## 执行步骤

1. 运行 `git status` 查看变更与未跟踪文件。若存在 `.env`、密钥、credentials 等敏感文件，先向用户确认是否提交。
2. 若项目尚无 `.git` 目录，先执行 `git init`。
3. 执行 `git add -A` 暂存全部变更；若第 1 步发现敏感文件，改为逐个添加非敏感文件。
4. 提交，提交信息用一行简短中文说明本次变更。始终用环境变量强制作者身份为 hyh-24（全局配置的 hyh / 126.com 邮箱无法关联 GitHub 账号，禁止修改全局 git config）：

   ```
   $env:GIT_AUTHOR_NAME='hyh-24'; $env:GIT_AUTHOR_EMAIL='hyh-24@users.noreply.github.com'
   $env:GIT_COMMITTER_NAME='hyh-24'; $env:GIT_COMMITTER_EMAIL='hyh-24@users.noreply.github.com'
   git commit -F <信息文件>
   ```

   中文提交信息在 PowerShell 5 下会乱码，先把信息写入 UTF-8 临时文件再用 `git commit -F` 提交，提交完删除临时文件。若没有任何变更且远程已有提交，跳过 commit，直接进入推送。
5. 确保远程地址正确。本机 22 端口不可达，必须使用 443 端口的 SSH（公钥已配置为 hyh-24）：

   ```
   git remote remove origin    # 仅当 origin 已存在但指向其他地址时执行
   git remote add origin ssh://git@ssh.github.com:443/hyh-24/hyh.git
   ```

6. 强制推送当前分支（强覆盖）：

   ```
   git push --force -u origin HEAD
   ```

   若远程默认分支为 main 而本地分支为 master，改用 `git push --force -u origin HEAD:main`。
7. 验证：`git status` 显示分支与 origin 同步、`git log --oneline -1` 显示刚才的提交，即推送成功。

## 约束

- 远程地址固定为 `ssh://git@ssh.github.com:443/hyh-24/hyh.git`。`git@github.com:...` 形式走 22 端口会连接失败，HTTPS 不可用。
- 本机 shell 为 PowerShell 5，多条命令用 `;` 连接（不支持 `&&`）。
- `--force` 强推是本技能的默认行为（用户要求强覆盖远程历史）。
- 不使用 `--no-verify`、`--no-gpg-sign`；不执行 `git config --global`。
- 不要为凑提交而新建 README 或其他无关文件。
- 推送完成后向用户报告：提交信息、分支名、推送结果。
