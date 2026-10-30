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
4. 提交，提交信息用一行简短中文说明本次变更。若 git 报缺少身份配置，用临时参数提交（禁止修改全局 git config）：

   ```
   git -c user.name="hyh-24" -c user.email="hyh-24@users.noreply.github.com" commit -m "提交信息"
   ```

   若没有任何变更且远程已有提交，跳过 commit，直接进入推送。
5. 确保远程地址正确（必须使用 SSH，依赖用户已配置的公钥）：

   ```
   git remote remove origin    # 仅当 origin 已存在但指向其他地址时执行
   git remote add origin git@github.com:hyh-24/hyh.git
   ```

6. 强制推送当前分支（强覆盖）：

   ```
   git push --force -u origin HEAD
   ```

   若远程默认分支为 main 而本地分支为 master，改用 `git push --force -u origin HEAD:main`。
7. 验证：`git status` 显示分支与 origin 同步、`git log --oneline -1` 显示刚才的提交，即推送成功。

## 约束

- 远程地址固定为 `git@github.com:hyh-24/hyh.git`，不要改用 HTTPS。
- `--force` 强推是本技能的默认行为（用户要求强覆盖远程历史）。
- 不使用 `--no-verify`、`--no-gpg-sign`；不执行 `git config --global`。
- 不要为凑提交而新建 README 或其他无关文件。
- 推送完成后向用户报告：提交信息、分支名、推送结果。
