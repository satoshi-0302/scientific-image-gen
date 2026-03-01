---
description: 科学実験画像生成 (Character Consistent Scientific Image Generation)
---

# 科学実験画像生成ワークフロー

このワークフローは、指定されたキャラクター（男女）の**「顔のリファレンス」**と**「全身（作業着）のリファレンス」**の**両方**をアンカーとして使用し、顔立ちと服装の完全な一貫性を保ちながら、複雑な理科実験の画像を生成するための手順です。

## 前提条件
以下のファイルが `/Users/saitosatoshi/.gemini/antigravity/scratch/scientific-image-gen/` 内に存在することを確認してください。
*   女性用: `female_face_reference.png`, `female_fullbody_navy.png`
*   男性用: `male_face_reference.png`, `male_fullbody_navy.png`

## 手順 (Execution Steps)

1.  **キャラクターの選択と画像の特定**
    *   ユーザーが指定したキャラクター（女性または男性）に応じて、使用する参照画像2枚の絶対パスを特定します。
    *   例（女性の場合）: `ImagePaths = ["/Users/saitosatoshi/.../female_face_reference.png", "/Users/saitosatoshi/.../female_fullbody_navy.png"]`

2.  **空間的制約プロンプトの構築**
    *   AI（Imagen）は「滴定」や「ピペット操作」といった抽象的な単語だけでは正確な物理接触を描画できません。
    *   以下のように、**「何が」「どこで」「どのように固定されているか」**を大文字の箇条書き（EXTREMELY STRICT RULES）で記述した強力なプロンプトを作成します。

    **【プロンプトのテンプレート】**
    ```text
    Using the EXACT facial identity and EXACT navy blue workwear from the TWO provided reference images. 
    A photorealistic image of a [25-years-old Japanese woman with brown hair tied back / 35-years-old Japanese man with short dark hair and beard].
    
    SCENE: [実験の概要、例：performing an acid-base titration in a laboratory.]

    EXTREMELY STRICT RULES:
    1. [固定具の指示、例：The tall glass burette is SECURELY HELD BY A METAL RETORT STAND AND CLAMP.]
    2. [右手の指示、例：Her RIGHT HAND is physically gripping the RED STOPCOCK VALVE.]
    3. [左手の指示、例：Her LEFT HAND is swirling the ERLENMEYER FLASK resting on the table.]
    4. [視線の指示、例：Her EYES and FACE are looking DOWNWARD, sharply focused on the flask.]

    Clean, well-lit modern science laboratory background. Masterpiece. High resolution.
    ```

3.  **`generate_image` ツールの実行**
    *   考案したプロンプトを `Prompt` 引数に渡し、**Step 1で特定した2枚の画像パス**を `ImagePaths` 引数（配列）に渡してツールを実行します。
    *   **重要:** `ImagePaths` には最大3枚まで画像を渡せます。顔と全身の2枚を**同時に**渡すことで、AIは「この顔で、この服を着ている人物」という強固な視覚アンカーを得ます。

4.  **結果の検証とエラー対応 (Inpaintingの提案)**
    *   画像生成にはAPIの利用制限（クオータ）が存在します。生成が失敗した場合は、エラーメッセージを読み解きユーザーにしばらく待つよう丁寧に報告してください。
    *   もし生成に成功しても「手が3本ある」などの人体破綻が起きていると予想される（またはユーザーから指摘された）場合は、再度ゼロから生成をやり直してクオータを消費するのではなく、**「PhotoshopやStable DiffusionのInpainting（生成塗りつぶし）機能を使って、余分な手や指だけを背景と同化させるように修正する」**という実務的なフォールバック手順をユーザーに提案してください。
