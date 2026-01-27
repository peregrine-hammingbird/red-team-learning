# Special Privileges & Security Descriptors (Defensive Notes)

## TL;DR
- SeBackupPrivilege / SeRestorePrivilege はファイルアクセス制御（DACL）を実質的に迂回し得る強い権限。
- “権限付与”と“サービス（例：WinRM）へのアクセス制御（Security Descriptor）”は、リモート操作の可否を左右する。
- 防御は「権限の割当」「セッション構成/アクセス許可の変更」を監査できることが重要。

## Key concepts
- **SeBackupPrivilege**：バックアップ目的で読み取りを強化（DACL影響を受けにくい）
- **SeRestorePrivilege**：復元目的で書き込みを強化（DACL影響を受けにくい）
- **Security Descriptor**：サービスやオブジェクトに対するアクセス制御（ACLに近い概念）

## Why it matters (SOC view)
攻撃では「権限（Privilege）」と「アクセス許可（Descriptor）」の両方を組み合わせて、
- 重要ファイルへのアクセス
- リモート管理経路の確保
を狙う。

## Telemetry / Detection ideas
- ユーザーへの特権割当（権限の追加/変更）
- リモート管理（WinRM/PowerShell Remoting等）の構成変更
- セッション構成や許可設定の変更（PowerShell関連ログも含む）

> 重要：PowerShell運用ログ（ScriptBlock Logging等）を有効化すると調査が一気に楽になる。

## Triage checklist (SOC L1)
1. どのユーザーに、どの権限が付与/変更された？
2. その変更はチケット/運用手順に沿っている？
3. 直後にリモートログオン/管理操作が発生していないか？
4. セッション構成（リモート管理）の変更が同時に起きていないか？
5. 不審なら：影響範囲（同一実行者が他にも変更していないか）

## Hardening / Prevention
- 強い特権は最小限に（バックアップ/復元権限の付与は厳格運用）
- WinRM/Remotingは許可を絞る（接続元制限、認証強化、監査）
- PowerShellログ強化（監査・検知に直結）
- “設定変更”を行えるアカウントを厳格化（PAM/JIT）

## Notes (what I learned)
- 「特権を与える」だけでなく、「リモート経路の許可」をセットで見ないと危ない。
- SOCとしては “権限変更→リモート操作” の連鎖を優先的に相関する。
