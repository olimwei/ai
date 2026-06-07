---
title: "VS code + Github 多账户使用配置"
layout: blog
excerpt: "整理如何在一台电脑的VS code里使用不同Github账户。"
read_time: true
comments: true
share: true
# author_profile: true
classes: wide
categories:
  - 欧耶之AI
tags:
  - Gemini
  - Visual Studio Code
  - Github
---
Gemini根据我和他的对话，整理了一份在 macOS 上设置并同时使用两个 GitHub 账户（一个全局 Personal，一个特定文件夹下的 Work）的完整指南。

-----

### 在 macOS 上设置两个 GitHub 账户 (Personal & Work) 并与 VS Code 集成

这个指南假设：

  * 你有两个 GitHub 账户：一个用于个人项目 (Personal)，一个用于工作项目 (Work)。
  * 你想将 Personal 账户设置为全局默认。
  * Work 账户仅用于特定文件夹下的仓库。
  * 你会使用 SSH 密钥进行认证。
  * 你使用的是 macOS 操作系统。

-----

#### 概览

1.  **生成和管理 SSH 密钥对**：为每个 GitHub 账户生成独立的 SSH 密钥对。
2.  **配置 SSH 客户端**：在 `~/.ssh/config` 文件中配置 SSH 别名，以便 Git 知道使用哪个密钥连接到哪个 GitHub 账户。
3.  **上传公钥到 GitHub**：将生成的公钥上传到对应的 GitHub 账户。
4.  **配置 Git 用户信息**：设置全局 Git 用户信息，并为工作文件夹设置条件性的用户信息。
5.  **克隆或更新仓库**：使用正确的 SSH URL 克隆或更新你的 Git 仓库。
6.  **在 VS Code 中进行操作**：验证 VS Code 如何处理这两个账户。
7.  **SSH 密钥密码短语的记忆 (macOS 特有)**：确保 macOS 钥匙串能记住你的 SSH 密钥密码短语，避免每次输入。

-----

#### 详细步骤

#### 步骤 1：生成和管理 SSH 密钥对

为避免混淆，给每个账户生成独立的 SSH 密钥文件。

1.  **打开“终端” (Terminal) 应用。**
2.  **为个人账户生成 SSH 密钥：**
    ```bash
    ssh-keygen -t rsa -b 4096 -C "你的个人邮箱@example.com" -f ~/.ssh/id_rsa_personal
    ```
      * 当系统提示输入密码短语 (passphrase) 时，**强烈建议设置一个复杂但你能记住的密码短语**。这是为了密钥的安全。
      * `id_rsa_personal` 是这个密钥文件的名称。
3.  **为工作账户生成 SSH 密钥：**
    ```bash
    ssh-keygen -t rsa -b 4096 -C "你的工作邮箱@example.com" -f ~/.ssh/id_rsa_work
    ```
      * 同样，为这个密钥设置一个密码短语。
4.  **设置密钥文件权限：**
    ```bash
    chmod 600 ~/.ssh/id_rsa_personal
    chmod 600 ~/.ssh/id_rsa_work
    ```
      * 这确保只有你可以读写这些私钥文件，提高安全性。

-----

#### 步骤 2：配置 SSH 客户端 (`~/.ssh/config`)

这个文件告诉 SSH 客户端，当它尝试连接到特定的主机时，应该使用哪个密钥。

1.  **打开 `~/.ssh/config` 文件：**

    ```bash
    code ~/.ssh/config
    # 如果 code 命令不工作，请使用：nano ~/.ssh/config 或者在 Finder 中手动导航到 ~/.ssh 文件夹并打开 config 文件
    ```

      * 如果 `.ssh` 文件夹或 `config` 文件不存在，请手动创建：`mkdir -p ~/.ssh && touch ~/.ssh/config`。

2.  **添加以下内容到 `~/.ssh/config` 文件中：**

    ```ini
    # --- macOS Keychain Integration (for remembering passphrases) ---
    Host *
      AddKeysToAgent yes
      UseKeychain yes
      # IdentityAgent ~/Library/Group\ Containers/243LU875E5.com.microsoft.VSCode/User/settings.json/.ssh/agent.sock
      # ^ 备注：上面这行通常不需要，如果遇到问题再考虑，并且路径可能需要根据VS Code实际路径调整

    # --- Personal GitHub Account ---
    Host github.com-personal
      HostName github.com
      User git
      IdentityFile ~/.ssh/id_rsa_personal
      IdentitiesOnly yes
      # PreferredAuthentications publickey

    # --- Work GitHub Account ---
    Host github.com-work
      HostName github.com
      User git
      IdentityFile ~/.ssh/id_rsa_work
      IdentitiesOnly yes
      # PreferredAuthentications publickey
    ```

      * **`Host *` 部分：** 启用 macOS 钥匙串集成，确保 SSH 代理能记住密码短语。
      * **`github.com-personal` 和 `github.com-work`：** 这些是自定义的别名。当你克隆仓库时，将 `git@github.com:` 替换为 `git@github.com-personal:` 或 `git@github.com-work:`，SSH 就会知道使用哪个密钥。
      * `IdentitiesOnly yes`：确保 SSH 只使用 `IdentityFile` 指定的密钥，而不会尝试其他默认密钥，避免混淆。

3.  **保存并关闭 `~/.ssh/config` 文件。**

-----

#### 步骤 3：上传公钥到 GitHub

你需要将每个私钥对应的公钥上传到各自的 GitHub 账户。

1.  **复制个人账户的公钥：**
    ```bash
    cat ~/.ssh/id_rsa_personal.pub
    ```
      * 复制终端中显示的全部内容。
2.  **上传到你的个人 GitHub 账户：**
      * 登录到你的**个人 GitHub 账户** (github.com)。
      * 点击右上角的头像 -\> `Settings` (设置) -\> 左侧 `SSH and GPG keys`。
      * 点击 `New SSH key` (或 `Add SSH key`)。
      * 为密钥起一个标题 (例如：`My Personal Mac Key`)。
      * 将你复制的公钥粘贴到“Key”字段。
      * 点击 `Add SSH key`。
3.  **复制工作账户的公钥：**
    ```bash
    cat ~/.ssh/id_rsa_work.pub
    ```
      * 复制终端中显示的全部内容。
4.  **上传到你的工作 GitHub 账户：**
      * 登录到你的**工作 GitHub 账户** (github.com)。
      * 同样，导航到 `Settings` -\> `SSH and GPG keys`。
      * 点击 `New SSH key`。
      * 为密钥起一个标题 (例如：`My Work Mac Key`)。
      * 将你复制的公钥粘贴到“Key”字段。
      * 点击 `Add SSH key`。

-----

#### 步骤 4：配置 Git 用户信息

这将告诉 Git 在提交时使用哪个用户名和邮箱。

1.  **设置全局默认 Git 用户信息 (你的个人账户)：**

    ```bash
    git config --global user.name "你的个人GitHub用户名"
    git config --global user.email "你的个人邮箱@example.com"
    ```

2.  **为工作项目设置条件性 Git 用户信息：**
    这个设置告诉 Git：当你在特定的文件夹（例如 `/Users/你的用户名/Documents/WorkProjects/`）中时，使用不同的用户名和邮箱。

      * **首先，创建你的工作项目根目录（如果它不存在）：**

        ```bash
        mkdir -p /Users/你的用户名/Documents/WorkProjects/ # 将此路径替换为你的实际工作文件夹路径
        ```

      * **然后，编辑 Git 的全局配置文件 (`~/.gitconfig`)：**

        ```bash
        code ~/.gitconfig
        # 如果 code 命令不工作，请使用：nano ~/.gitconfig 或者手动打开
        ```

      * 在文件的**最底部**，添加以下内容：

        ```ini
        # Conditional Git config for Work projects
        [includeIf "gitdir:~/Documents/WorkProjects/"] # 将此路径替换为你的实际工作文件夹路径
            path = .gitconfig-work
        ```

          * `gitdir:~/Documents/WorkProjects/`: 这表示当 Git 仓库位于此路径或其子目录时，包含 `path` 指定的配置文件。请确保替换为你的**实际路径**。
          * `path = .gitconfig-work`: 指定要包含的配置文件的名称。

      * **保存并关闭 `~/.gitconfig` 文件。**

      * **现在，创建 `~/.gitconfig-work` 文件并添加工作账户信息：**

        ```bash
        code ~/.gitconfig-work
        ```

          * 在 `~/.gitconfig-work` 文件中添加以下内容：
            ```ini
            [user]
                name = 你的工作GitHub用户名
                email = 你的工作邮箱@example.com
            ```
          * **保存并关闭 `~/.gitconfig-work` 文件。**

-----

#### 步骤 5：克隆或更新仓库

现在，你可以使用正确的 SSH URL 来克隆或更新你的 Git 仓库了。

1.  **克隆个人仓库 (使用个人 SSH 别名)：**

      * 在 GitHub 上复制个人仓库的 SSH URL (例如：`git@github.com:your_personal_username/your_personal_repo.git`)。
      * 在终端中，克隆时修改 URL：
        ```bash
        git clone git@github.com-personal:your_personal_username/your_personal_repo.git
        ```
      * **验证 Git 用户信息：** 进入该仓库目录，运行 `git config user.name` 和 `git config user.email`，确认是你的个人账户信息。

2.  **克隆工作仓库 (使用工作 SSH 别名)：**

      * 将工作仓库克隆到你在步骤 4 中设置的**工作文件夹** (`~/Documents/WorkProjects/`) 下。
      * 在 GitHub 上复制工作仓库的 SSH URL (例如：`git@github.com:your_work_username/your_work_repo.git`)。
      * 在终端中，先进入工作文件夹，然后克隆并修改 URL：
        ```bash
        cd ~/Documents/WorkProjects/ # 进入你的工作文件夹
        git clone git@github.com-work:your_work_username/your_work_repo.git
        ```
      * **验证 Git 用户信息：** 进入该工作仓库目录，运行 `git config user.name` 和 `git config user.email`，确认是你的工作账户信息。

3.  **如果你已经有本地仓库：**

      * 进入仓库目录。
      * 检查现有远程 URL：`git remote -v`。
      * 如果显示的是 `git@github.com:...`，请修改它：
        ```bash
        git remote set-url origin git@github.com-work:your_work_username/your_repo.git # 或 -personal
        ```
      * 再次 `git remote -v` 确认 URL 已经更新为带有 `-work` 或 `-personal` 别名的形式。

-----

#### 步骤 6：在 VS Code 中进行操作

VS Code 的 Git 集成应该会无缝地使用你上述配置。

1.  **打开 VS Code。**
2.  **打开你的个人仓库文件夹。**
3.  **进行修改，提交，然后点击 VS Code 状态栏的同步（Sync Changes）按钮进行推送。** 此时，Git 应该会使用你的个人密钥。
4.  **打开你的工作仓库文件夹。**
5.  **进行修改，提交，然后点击 VS Code 状态栏的同步（Sync Changes）按钮进行推送。** 此时，Git 应该会使用你的工作密钥。

-----

#### 步骤 7：SSH 密钥密码短语的记忆 (macOS 特有)

这是你之前一直遇到的痛点，确保 macOS 钥匙串能记住密码短语。

1.  **设置 Git 配置，强制使用 SSH 代理和钥匙串：**

      * 在终端中运行（这会影响所有 Git 仓库）：
        ```bash
        git config --global core.sshCommand "ssh -o 'AddKeysToAgent yes' -o 'UseKeychain yes'"
        ```
          * 这会确保 Git 在调用 `ssh` 命令时，总是带上 `AddKeysToAgent yes` 和 `UseKeychain yes` 选项，从而强制 macOS 使用钥匙串来管理密码短语。

2.  **清理 SSH 代理并测试：**

      * **完全关闭所有终端窗口和 Visual Studio Code。**
      * **重新打开一个新的终端窗口。**
      * **运行以下命令，确保 SSH 代理是空的：**
        ```bash
        killall ssh-agent # 终止所有 ssh-agent 进程 (可能没有输出)
        eval "$(ssh-agent -s)" # 启动一个新的 ssh-agent
        ssh-add -D # 清空代理中所有已加载的密钥
        ```
          * 运行 `ssh-add -l` 确认输出是 `The agent has no identities.`。
      * **现在，在新的终端中，尝试直接连接到 GitHub（这会触发密钥和密码短语的提示）：**
        ```bash
        ssh -T git@github.com-work # 尝试连接工作账户
        ssh -T git@github.com-personal # 尝试连接个人账户
        ```
          * **关键点：** 当你运行这些命令时，**macOS 系统应该会弹出钥匙串访问对话框**。
          * **请务必输入你的 SSH 密钥密码短语，并寻找并勾选“始终允许”（Always Allow）或“记住密码”（Remember password）的选项。** 勾选后，点击确认。
          * 看到 `Hi your_username! You've successfully authenticated, but GitHub does not provide shell access.` 这句话就表示 SSH 认证成功了。

3.  **验证密码短语是否被记住：**

      * **完全关闭所有终端窗口和 Visual Studio Code。**
      * **重新打开一个新的终端窗口。**
      * **直接运行：**
        ```bash
        ssh-add -l
        ```
          * **这一次，它应该** **不会** **再要求你输入密码短语了。** 如果它能直接列出你的两个密钥，就说明密码短语已经成功地存储在钥匙串中，`ssh-agent` 可以在需要时自动获取。
      * **回到 VS Code，尝试在两个仓库中进行推送。** 此时，应该不再需要输入密码短语了。
