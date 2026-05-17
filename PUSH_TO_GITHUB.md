# 推 lius-cookbook 到 GitHub · 二選一

Bruce 早上看到後，**選 A 或 B** 跑一條，我就把所有後續事情接走。

---

## 選項 A · gh CLI 一鍵登入（推薦，需要瀏覽器）

```bash
# 1. 在這台機器登入（會印一個 8 碼 code，你開瀏覽器貼到 github.com/login/device）
gh auth login -h github.com -p https -w

# 2. 建 repo + push（一行搞定）
cd ~/coding/lius-cookbook
gh repo create lius-cc/lius-cookbook \
  --public \
  --description "Official How-We-Use Walkthroughs for the Open-Source Daoism LLM (CC0)" \
  --source=. \
  --remote=origin \
  --push
```

預估 30 秒。

---

## 選項 B · 用 SSH key（純命令列，不開瀏覽器）

把這把 SSH 公鑰加到你的 `lius-cc` GitHub 組織的 Deploy Keys（或個人 SSH keys）：

```
ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAINT23XiJ9H535LHZOfoSTRJx3tMmHAFVc0QB/mjTVwsN oracle-jarvis-to-gcp
```

加好後：

```bash
# 1. 在 github.com 用 UI 創 repo lius-cc/lius-cookbook（空 repo, no README）
# https://github.com/organizations/lius-cc/repositories/new

# 2. push
cd ~/coding/lius-cookbook
git remote add origin git@github.com:lius-cc/lius-cookbook.git
git push -u origin main 2>/dev/null || git push -u origin master
```

---

## 選項 C · 給我 PAT（最快，純自動化）

如果你有 GitHub PAT（Personal Access Token，scope: `repo`），把它丟給我：

```
GH_TOKEN=ghp_xxxxxxxxxxxx
```

我就用 `gh auth login --with-token` 自動跑完所有事情。

---

## 推完後我會自動接手做的事

1. 在 `/llm/cookbook` 頁面確認 GitHub repo URL 可達
2. 把 `LIUS-LLM-V2-IMPL-2026-05-17.md` 摘要更新「✅ 已推 GitHub」
3. 觸發第一份 Wayback snapshot 對 `github.com/lius-cc/lius-cookbook` 也存一份（公開先發證據三件套之一）
4. 把 lius-cookbook README 上的 GitHub-only badge 全部上線（Colab notebooks 也會生效）
