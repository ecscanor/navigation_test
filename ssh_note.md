terminal: ssh (-v, optional) (user)@(192.168.1.55)

If a window file is transferred to linux, the line ending format needs to changed from crlf to lf.

git normal step:
    git add .
    git commit -m "text"
    git push

    git pull

git envirenoment setting:
    # 換行符號設定
    git config --global core.autocrlf true        # Windows 端
    git config --global core.autocrlf input       # Linux 端

    # 處理本地衝突
    git checkout --theirs <file>   # 用遠端版本
    git checkout --ours <file>     # 用本地版本
    git stash                      # 暫存本地修改
    git stash pop                  # 還原暫存

    # 設定免密碼登入 (SSH)
    ssh-keygen -t ed25519 -C "your_email@example.com"   # 產生金鑰
    cat ~/.ssh/id_ed25519.pub                             # 複製公鑰
    # 貼到 GitHub → Settings → SSH Keys

    # 把 remote 改成 SSH
    git remote set-url origin git@github.com:ecscanor/navigation_test.git