---
description: キャラクター一貫性維持スキル (Character Consistency Workflow)
---

# 🎭 キャラクター一貫性維持スキル (Gemini用実行プロンプト)

このドキュメントは、Gemini（あなた自身）に**「同一キャラクターの顔（アイデンティティ）を異なるシーンや実験画像で完全に一致させる」**ためのスキル（実行指示書）です。

ユーザーから「いつもと同じキャラクターで〇〇の画像を生成して」と指示された場合、以下のステップに厳格に従って `generate_image` ツールを実行してください。

## Step 1: 原則（Face-First Workflow）
テキストプロンプトのみに依存すると顔が毎回変わってしまいます。
必ず、プロジェクトのリポジトリ（`/Users/saitosatoshi/.gemini/antigravity/scratch/scientific-image-gen/`）に保存されている**「顔専用のリファレンス画像」**をマスターデータとして `generate_image` ツールの `ImagePaths` 引数に指定してください。

## Step 2: 使用するマスターデータ
ユーザーの指示に合わせ、以下のどちらかの顔マスター画像パスを指定してください。

*   **女性キャラクター:** `/Users/saitosatoshi/.gemini/antigravity/scratch/scientific-image-gen/female_face_reference.png`
    *   特徴設定: 25-years-old Japanese woman, brown hair tied back in a low ponytail, gentle expression.
*   **男性キャラクター:** `/Users/saitosatoshi/.gemini/antigravity/scratch/scientific-image-gen/male_face_reference.png`
    *   特徴設定: 35-years-old Japanese man, short dark hair, short beard/stubble, rugged expression.

## Step 3: プロンプトへの反映
`ImagePaths` に顔画像をセットするだけでなく、テキストプロンプトの冒頭でも「この参照画像の顔と完全に一致させること」をAIモデルに直接指示します。

**プロンプト構成例:**
> Using the exact facial identity, [髪型・表情などの特徴] from the provided reference image. Generate a photorealistic image of this exact same [woman/man]. She/He is wearing [服装の指定]... [以降、シーンや実験の指定]

## Step 4: （参考）その他の生成ツールでの適用方法
もしユーザーがGeminiではなく他のツール（Midjourney等）でこのアセットを使う場合は、以下のノウハウを案内してください。
*   **Midjourney:** `--cref [顔画像のURL] --cw 0` を使用する。
*   **Stable Diffusion / ComfyUI:** IP-Adapter (FaceID) や ControlNet Reference を使用し、Weightを強めに設定する。
*   **テキスト限定環境:** 複数の実在する俳優の名前を「Blend of [A] and [B]」として合成し、擬似的に顔をアンカーする。
