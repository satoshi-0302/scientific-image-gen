# 🎭 キャラクター一貫性維持ガイド (Face-First Workflow)

AI画像生成において、同一キャラクターの顔（アイデンティティ）を異なるシーンや実験画像で完全に一致させるための、最も確実なワークフローです。テキストプロンプトのみに依存すると顔が毎回変わってしまうため、**「顔専用のリファレンス画像」**をマスターデータとして使用します。

## 1. 原則：顔から体を作る（Face-First）

これまでは「全身画像」を先に作り、そこから顔を統一させようとして失敗していました。
今後は、**「顔専用の3面図（マスター）」をAIに強制的に読み込ませ、そこに『作業着を着ている』などのプロンプトを加える**ことで、常に同じ顔の人物を作ります。

## 2. ツール別の適用方法

このプロジェクトのリポジトリには、以下のマスター顔データが保存されています。
*   `female_face_reference.png` （茶髪・一つ結びの女性）
*   `male_face_reference.png` （短髪・髭の男性）

使用するAIツールに応じて、以下の方法でこの画像を適用してください。

### A: Midjourney を使用する場合（推奨）
Midjourneyの強力なキャラクター参照機能（`--cref`）を使用します。

1.  プロンプトの末尾に `--cref [女性の顔画像のURL]` を追加します。
2.  顔への忠実度を最大にするため、`--cw 0` を追加します（これにより、髪型と顔のみが固定され、服は自由にプロンプトで変更できます）。

**プロンプト例（エバポレーターの実験をする女性）:**
> A photorealistic image of a young Japanese woman in a laboratory, wearing an unbuttoned white lab coat over a light blue shirt. She is holding a round bottom flask... [実験の空間的制約プロンプト]... `--cref https://.../female_face_reference.png --cw 0`

### B: Stable Diffusion / ComfyUI / nanobanana pro の場合
IP-Adapter（FaceIDモジュール）や、ControlNetのReference Modeを使用します。

1.  ツールの「Image Prompt」または「FaceID」入力スロットに、マスター顔データ（例：`male_face_reference.png`）をアップロードします。
2.  影響力（Weight）を0.8〜1.0程度に強めに設定します。
3.  テキストプロンプトには、顔の描写（Japanese man, short dark hair, short beard）を念のため含めつつ、服装と動作を詳細に記述します。

### C: テキストしか使えない環境での裏技（Name Anchoring）
画像をアップロードできないAIを使う場合の苦肉の策です。
顔の造形がブレないよう、AIが知っている特定の複数のモデルや俳優の名前を合成してプロンプトの先頭に固定します。

**プロンプト例:**
> The person's face is an exact blend of [俳優A] and [俳優B]. A photorealistic image of a Japanese man...

## 3. リポジトリ内のアセット

*   `female_face_reference.png` (顔マスター・女)
*   `male_face_reference.png` (顔マスター・男)
*   `female_fullbody_navy.png` (全身の作業着姿・女 ※上記ガイドラインで顔マスターから生成したもの)
*   `male_fullbody_navy.png` (全身の作業着姿・男 ※同上)
