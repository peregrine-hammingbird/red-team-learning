# Hijacking File Associations (Defensive Notes)

## TL;DR
- ファイル関連付け（ProgID / open command）はレジストリにより管理され、変更されると“ファイルを開く”動作が乗っ取られ得る。
- HKLM（全体）/HKCU（ユーザー単位）どちらも監視対象。
- 防御は「関連付けキーの変更監視」＋「ファイルオープン起点の不審プロセス」を見る。

## Why it matters
ユーザーが自然に行う「ファイルを開く」操作に悪性動作を紐づけられるため、
- 永続化
- 初期実行
- ユーザー操作の悪用
に使われやすい。

## Key concepts
- 拡張子（例：.txt）→ ProgID → open command の参照
- レジストリ階層：HKLM / HKCU のどちらで上書きされるかが重要

## Telemetry / Detection ideas
### 監視ポイント
- ファイル関連付け（ProgIDやopen command）に関わるレジストリキー変更
- “ファイルを開いた直後”に、通常出ないプロセスが起動していないか
  - 例：notepad起点なのにスクリプト/コマンド実行が走る等

## Triage checklist (SOC L1)
1. 対象拡張子は何？（.txtなど）
2. open command が変わっていないか？（通常と比較）
3. 変更者・変更時刻・変更端末
4. 影響範囲：他ユーザー/他端末でも同様の変更がないか
5. 直後の挙動：不審なプロセス連鎖/ネットワーク通信がないか

## Hardening / Prevention
- HKLMの変更は管理者限定、HKCUも監視対象にする
- 重要端末では関連付け変更を制限（ポリシー/管理ツール）
- AppLocker / WDAC で不審実行を抑制
- レジストリ監視（Sysmon等）を導入し、変更をSIEMでアラート化

## Notes (what I learned)
- レジストリの“open command”を書き換えると、ユーザー操作に自然に混ぜられる。
- SOC的には「関連付け変更イベント」＋「ファイルオープン直後のプロセスツリー」で追うのが強い。
