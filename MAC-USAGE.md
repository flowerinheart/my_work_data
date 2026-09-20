# 在另一台 Mac 上使用迁移压缩包

仓库：https://github.com/flowerinheart/my_work_data  
文件名示例：`ai-testing-lab-migrate-20260920-1925.zip`

容器里已验证：解包成功，Meridian 登录页 200、admin 登录 302、dashboard 200。

## 1. 下载

```bash
cd ~/Downloads
# 若仓库是公开的：
curl -L -o ai-testing-lab-migrate.zip \
  https://github.com/flowerinheart/my_work_data/raw/main/ai-testing-lab-migrate-20260920-1925.zip

# 或 git clone 后在仓库目录里找 zip
# git clone https://github.com/flowerinheart/my_work_data.git
```

GitHub 网页：打开仓库 → 点 zip → Download。

## 2. 系统依赖（Mac）

```bash
# Homebrew：https://brew.sh
brew install python@3.12 openjdk@21 maven
export PATH="$(brew --prefix python@3.12)/bin:$(brew --prefix openjdk@21)/bin:$PATH"
```

安装 [Cursor](https://cursor.com)，登录**同一账号**（Memories / 部分 Rules 在云端）。

## 3. 解包到 `~/code`

zip 根目录是 `ai-testing-lab-migrate/`。用包内脚本（不要用 Windows 的 `.ps1`）：

```bash
cd ~/Downloads
unzip -o ai-testing-lab-migrate-20260920-1925.zip
cd ai-testing-lab-migrate

export HOME="$HOME"
export DEST_CODE_ROOT="$HOME/code"
export SOURCE_USER="hma"
bash migrate-kit/restore-migrate.sh "$(pwd)/../ai-testing-lab-migrate-20260920-1925.zip"
# 若 zip 在当前目录的上一级；更稳妥：

ZIP="$HOME/Downloads/ai-testing-lab-migrate-20260920-1925.zip"
bash "$HOME/Downloads/ai-testing-lab-migrate/migrate-kit/restore-migrate.sh" "$ZIP"
```

解完后目录应是：

```text
~/code/
  AI_TESTING_AGENT_ZONES.md
  ai-testing-learning-lab/
  ai-testing-lab-whiteboard/
  ai-testing-lab-evaluator/
  playwright-api-lab/
  meridian-erp/
```

## 4. Python / Playwright / 检查树

```bash
export CODE_ROOT="$HOME/code"
export SKIP_PLAYWRIGHT=0
bash "$CODE_ROOT/ai-testing-learning-lab/migrate/setup-env.sh"
```

把 `~/code/ai-testing-learning-lab/migrate/user-rules-to-paste.md` 贴进 Cursor → Settings → Rules → User Rules。

新开 Chat，`@`：

`~/code/ai-testing-learning-lab/migrate/AGENT-MIGRATE-GUIDE.md`

## 5. 启动网站（冒烟）

```bash
export CODE_ROOT="$HOME/code"
bash "$CODE_ROOT/ai-testing-learning-lab/migrate/run-meridian-smoke.sh"
```

浏览器打开 http://127.0.0.1:8080/login  
账号：`admin@erp.com` / `Admin@1234`

## 不要做的事

- 不要在白板聊天里打开 `ai-testing-lab-evaluator/oracle`
- 不要指望 Cursor 旧聊天气泡自动出现；JSONL 只在 `~/.cursor/projects/.../agent-transcripts`
- Docker 里验证用的是 **Linux 容器**，不是 macOS 系统镜像；真机 Mac 按上面 brew 走即可
