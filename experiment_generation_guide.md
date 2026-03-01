---
description: 科学実験画像生成スキル (Scientific Image Generation Workflow)
---

# 🤖 科学実験画像自動生成スキル (Gemini用実行プロンプト)

このドキュメントは、Gemini（あなた自身）に**「キャラクターの一貫性を保ちながら、正確な理科実験の画像を自動生成させる」**ためのスキル（実行指示書）です。

ユーザーから「〇〇の実験画像を生成して」と指示された場合、以下のステップに厳格に従って `generate_image` ツールを実行してください。

## Step 1: キャラクターリファレンスの指定
画像を生成する際は、必ずプロジェクトディレクトリ（`/Users/saitosatoshi/.gemini/antigravity/scratch/scientific-image-gen/`）内にある以下の顔リファレンス画像のどちらか（または両方）を `ImagePaths` 引数に指定し、画像をアンカー（固定）してください。

*   **女性キャラクター:** `female_face_reference.png`
    *   特徴設定: 25-years-old Japanese woman, brown hair tied back in a low ponytail, gentle expression.
*   **男性キャラクター:** `male_face_reference.png`
    *   特徴設定: 35-years-old Japanese man, short dark hair, short beard/stubble, rugged expression.

## Step 2: 空間的・物理的制約プロンプトの構築
AIは「実験名（例：滴定）」だけでは正しく器具を持てません。必ず以下のルールに基づいて、空間的な配置を**大文字で強調した箇条書き（EXTREMELY STRICT RULES）**としてプロンプトに組み込んでください。

### 構成フォーマット（例）
```
Using the exact facial identity from the provided reference image(s).
A photorealistic image of [キャラクターの特徴] wearing [服装（例：navy blue industrial workwear jacket）].

SCENE: [実験のシーン説明]

EXTREMELY STRICT RULES:
1. The tall glass burette is SECURELY HELD BY A METAL RETORT STAND AND CLAMP.
2. His/Her RIGHT HAND is physically gripping the RED STOPCOCK VALVE attached halfway down the burette.
3. His/Her LEFT HAND is swirling the ERLENMEYER FLASK resting on the table.
4. His/Her EYES and FACE are looking DOWNWARD, sharply focused exactly on the flask.

Clean, well-lit modern science laboratory background. Masterpiece.
```

## Step 3: 画像生成の実行
準備したリファレンス画像パス（`ImagePaths`）と、構築した強力な空間制約プロンプト（`Prompt`）を用いて、`generate_image` ツールを実行します。

## Step 4: プレビューと自己検証 (Self-Correction)
出力された画像に対して、以下の点を検証してください（ツールで画像を見ることはできないため、生成結果の傾向としての想定です）。
もし明らかに指定した制約（手が3本ある、器具が浮いているなど）に違反していると推測される致命的なプロンプトミスがあった場合は、プロンプトを修正して再生成を試みます。

## （補足）エラーへの対処方針についてユーザーへ伝えること
Geminiの実行制限（APIクオータ）に引っかかった場合は、素直に「現在API制限中のため〇時間後に再開可能です」とユーザーに報告してください。
また、手が3本になるなどのAI特有の不可避な描画エラーに対しては、プロンプトの調整を繰り返してクオータを消費するよりも、「画像編集ソフトによるInpainting（部分修正）」をユーザーに提案してください。
