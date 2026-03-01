# 🧪 正確な理科実験画像生成のための「物理配置プロンプト」テンプレート

AI（nanobanana pro 等）に「あり得ない物理挙動・不自然な実験器具の持ち方」をさせないための、最も効果的なワークフローとプロンプトのテンプレートです。

## ワークフローの鉄則

1.  **模範となる実写写真のソース**を1〜3枚見つける（Wikipedia Commons等が最良）。
2.  **Image-to-Image（画像参照）**機能にその写真をセットする。
3.  以下のプロンプトテンプレートを使って、**手順書の「物理的・空間的な配置」を強烈な制約（大文字）**としてAIに指示する。

---

## 📋 プロンプト・テンプレート（コピペ用）

以下の `[ ]` で囲まれた部分を、テストしたい実験内容やご自身のキャラクター設定に書き換えて使用してください。

```text
A photorealistic, highly detailed image of [25-years-old Japanese woman, short black bob hair with bangs]. She is wearing [clear safety goggles, wearing a crisp white unbuttoned lab coat over a light blue collared shirt].

She is in a science laboratory performing [an acid-base titration].

EXTREMELY STRICT RULES:
1. The tall glass burette is SECURELY HELD BY A METAL RETORT STAND AND CLAMP.
2. Her RIGHT HAND is physically gripping the RED STOPCOCK VALVE attached halfway down the burette.
3. Her LEFT HAND is swirling the ERLENMEYER FLASK resting on the table.
4. Her EYES and FACE are looking DOWNWARD, sharply focused exactly on the pink liquid INSIDE the Erlenmeyer flask.

No floating objects. Perfect anatomy. Real science laboratory. Wide shot showing the metal stand base. Masterpiece.
```

### 💡 なぜこのプロンプトが効くのか？
*   **「EXTREMELY STRICT RULES」というブロック:** AIは長文のストーリー調の説明よりも、このように番号付きで大文字強調された「空間的・物理的な配置」のキーワード（RIGHT HAND, GRIPPING, STOPCOCK, LOOKING DOWNWARD）に強く反応しやすくなります。
*   **「〜しているところ」ではなく「どこに何があるか」を描写:** AIは「滴定」という概念を理解していません。したがって、「右手がコックを握り」「視線はフラスコの中にあり」「ガラス管は金属のスタンドで保持されている」という**ジオメトリ（幾何学・配置）の制約**として指示することで、一発OKの確率が飛躍的に高まります。

---

## 応用例（ピペット操作の場合）

```text
EXTREMELY STRICT RULES:
1. Her RIGHT HAND is holding the TOP PLUNGER of the MICROPIPETTE.
2. The bottom TIP of the micropipette is inserted into a SMALL PLASTIC EPPENDORF TUBE.
3. Her LEFT HAND is holding the EPPENDORF TUBE securely.
4. Her EYES are sharply focused on the TIP of the micropipette.
```

---

## 🚨 【最重要】生成後の検証と「手バリ」「人体破綻」の修正 (Inpainting)

どれだけプロンプトを完璧にしても、AI画像生成の構造上、**複数人が絡む構図や複雑な器具の操作では「手が3本になる」「指が融合する」といったエラーが一定確率で必ず発生します。**

素晴らしい顔や構図が出たのに手が崩れている場合は、画像を捨てるのではなく**「Inpainting（部分描き直し）」**で救済するのが最も実務的で効率の良いワークフローです。

### 修正ワークフロー (Generative Fill / Inpainting)

1.  **目視チェック:** 画像生成後、必ず「右手が1本か」「左手が1本か」「意図しない場所から腕が生えていないか」を指差し確認する。
2.  **マスクの作成:** Photoshopの「生成塗りつぶし (Generative Fill)」、または Stable Diffusion / Midjourney(Vary Region) 等の Inpaint 機能を開く。
3.  **不要な手の消去:** 「3本目の手」や「融合してしまった指」の部分だけをブラシで大まかに塗りつぶす（マスクする）。
4.  **再生成指示:** プロンプトを空にするか、シンプルに `empty space, background, lab coat`（空白、背景、白衣）などと入力して部分再生成を実行する。

**💡 ポイント:** 
複雑なものを新しく描かせるより、「不要なものを消して背景と同化させる」方がAIは圧倒的に得意です。構図全体を作り直すより、エラー部分だけを削り取るアプローチを優先してください。
