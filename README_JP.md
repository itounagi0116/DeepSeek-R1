# DeepSeek-R1
<!-- markdownlint-disable first-line-h1 -->
<!-- markdownlint-disable html -->
<!-- markdownlint-disable no-duplicate-header -->

<div align="center">
  <img src="https://github.com/deepseek-ai/DeepSeek-V2/blob/main/figures/logo.svg?raw=true" width="60%" alt="DeepSeek-V3" />
</div>
<hr>
<div align="center" style="line-height: 1;">
  <a href="https://www.deepseek.com/" target="_blank" style="margin: 2px;">
    <img alt="Homepage" src="https://github.com/deepseek-ai/DeepSeek-V2/blob/main/figures/badge.svg?raw=true" style="display: inline-block; vertical-align: middle;"/>
  </a>
  <a href="https://chat.deepseek.com/" target="_blank" style="margin: 2px;">
    <img alt="Chat" src="https://img.shields.io/badge/🤖%20Chat-DeepSeek%20R1-536af5?color=536af5&logoColor=white" style="display: inline-block; vertical-align: middle;"/>
  </a>
  <a href="https://huggingface.co/deepseek-ai" target="_blank" style="margin: 2px;">
    <img alt="Hugging Face" src="https://img.shields.io/badge/%F0%9F%A4%97%20Hugging%20Face-DeepSeek%20AI-ffc107?color=ffc107&logoColor=white" style="display: inline-block; vertical-align: middle;"/>
  </a>
</div>

<div align="center" style="line-height: 1;">
  <a href="https://discord.gg/Tc7c45Zzu5" target="_blank" style="margin: 2px;">
    <img alt="Discord" src="https://img.shields.io/badge/Discord-DeepSeek%20AI-7289da?logo=discord&logoColor=white&color=7289da" style="display: inline-block; vertical-align: middle;"/>
  </a>
  <a href="https://github.com/deepseek-ai/DeepSeek-V2/blob/main/figures/qr.jpeg?raw=true" target="_blank" style="margin: 2px;">
    <img alt="Wechat" src="https://img.shields.io/badge/WeChat-DeepSeek%20AI-brightgreen?logo=wechat&logoColor=white" style="display: inline-block; vertical-align: middle;"/>
  </a>
  <a href="https://twitter.com/deepseek_ai" target="_blank" style="margin: 2px;">
    <img alt="Twitter Follow" src="https://img.shields.io/badge/Twitter-deepseek_ai-white?logo=x&logoColor=white" style="display: inline-block; vertical-align: middle;"/>
  </a>
</div>

<div align="center" style="line-height: 1;">
  <a href="LICENSE" style="margin: 2px;">
    <img alt="Code License" src="https://img.shields.io/badge/Code_License-MIT-f5de53?&color=f5de53" style="display: inline-block; vertical-align: middle;"/>
  </a>
  <a href="https://github.com/deepseek-ai/DeepSeek-R1/blob/main/LICENSE-MODEL" style="margin: 2px;">
    <img alt="Model License" src="https://img.shields.io/badge/Model_License-Model_Agreement-f5de53?&color=f5de53" style="display: inline-block; vertical-align: middle;"/>
  </a>
</div>


<p align="center">
  <a href="DeepSeekR1_pdf_JP.md"><b>論文リンク</b>👁️</a>
</p>


## 1. はじめに

私たちは第一世代の推論モデルであるDeepSeek-R1-ZeroとDeepSeek-R1を紹介します。
教師ありファインチューニング（SFT）を事前ステップとして行わず、大規模な強化学習（RL）を通じて訓練されたDeepSeek-R1-Zeroは、推論において顕著なパフォーマンスを示しました。
RLを通じて、DeepSeek-R1-Zeroは自然に多くの強力で興味深い推論行動を発現させました。
しかし、DeepSeek-R1-Zeroは無限ループ、可読性の低さ、言語の混在などの課題に直面していました。これらの問題を解決し、推論性能をさらに向上させるため、RLの前にコールドスタートデータを組み込んだDeepSeek-R1を開発しました。
DeepSeek-R1は、数学、コード、推論タスクにおいてOpenAI-o1と同等の性能を達成しています。
研究コミュニティを支援するため、DeepSeek-R1-Zero、DeepSeek-R1、およびLlamaとQwenをベースにしたDeepSeek-R1から蒸留した6つの密モデルをオープンソース化しました。DeepSeek-R1-Distill-Qwen-32Bは、さまざまなベンチマークでOpenAI-o1-miniを上回り、密モデルにおける新しいSOTAを達成しています。

**注意: DeepSeek-R1シリーズモデルをローカルで実行する前に、[使用上の推奨事項](#使用上の推奨事項)セクションを確認することを推奨します。**

<p align="center">
  <img width="80%" src="figures/benchmark.jpg">
</p>

## 2. モデル概要

---

**ポストトレーニング: ベースモデルに対する大規模強化学習**

- 教師ありファインチューニング（SFT）を事前ステップとして行わず、ベースモデルに直接強化学習（RL）を適用しました。このアプローチにより、モデルが複雑な問題解決のための連鎖的思考（CoT）を探索できるようになり、DeepSeek-R1-Zeroが開発されました。DeepSeek-R1-Zeroは自己検証、反省、長いCoTの生成などの能力を示し、研究コミュニティにとって重要なマイルストーンとなりました。特に、SFTなしで純粋にRLを通じてLLMの推論能力を促進できることを実証した初のオープンリサーチです。

- DeepSeek-R1を開発するためのパイプラインを導入しました。このパイプラインには、改善された推論パターンの発見と人間の選好へのアラインメントを目的とした2つのRLステージ、およびモデルの推論能力と非推論能力の基盤となる2つのSFTステージが含まれています。このパイプラインがより優れたモデル作成を通じて業界に貢献すると確信しています。

---

**蒸留: 小規模モデルも強力になり得る**

- 大規模モデルの推論パターンが小規模モデルに蒸留可能であり、小規模モデルでRLを通じて発見された推論パターンよりも優れたパフォーマンスを発揮することを実証しました。オープンソースのDeepSeek-R1とそのAPIは、将来より優れた小規模モデルを蒸留するための研究コミュニティに貢献します。
- DeepSeek-R1が生成した推論データを使用して、研究コミュニティで広く使用されているいくつかの密モデルをファインチューニングしました。評価結果は、蒸留された小規模密モデルがベンチマークで例外なく優れたパフォーマンスを発揮することを示しています。Qwen2.5およびLlama3シリーズをベースにした1.5B、7B、8B、14B、32B、70Bのチェックポイントをコミュニティにオープンソース化しました。

## 3. モデルダウンロード

### DeepSeek-R1モデル

<div align="center">

| **モデル** | **総パラメータ数** | **活性化パラメータ数** | **コンテキスト長** | **ダウンロード** |
| :------------: | :------------: | :------------: | :------------: | :------------: |
| DeepSeek-R1-Zero | 671B | 37B | 128K   | [🤗 HuggingFace](https://huggingface.co/deepseek-ai/DeepSeek-R1-Zero)   |
| DeepSeek-R1   | 671B | 37B |  128K   | [🤗 HuggingFace](https://huggingface.co/deepseek-ai/DeepSeek-R1)   |

</div>

DeepSeek-R1-Zero & DeepSeek-R1はDeepSeek-V3-Baseをベースに訓練されています。モデルアーキテクチャの詳細については、[DeepSeek-V3](https://github.com/deepseek-ai/DeepSeek-V3) リポジトリを参照してください。

### DeepSeek-R1-Distillモデル

<div align="center">

| **モデル** | **ベースモデル** | **ダウンロード** |
| :------------: | :------------: | :------------: |
| DeepSeek-R1-Distill-Qwen-1.5B  | [Qwen2.5-Math-1.5B](https://huggingface.co/Qwen/Qwen2.5-Math-1.5B) | [🤗 HuggingFace](https://huggingface.co/deepseek-ai/DeepSeek-R1-Distill-Qwen-1.5B)   |
| DeepSeek-R1-Distill-Qwen-7B  | [Qwen2.5-Math-7B](https://huggingface.co/Qwen/Qwen2.5-Math-7B) | [🤗 HuggingFace](https://huggingface.co/deepseek-ai/DeepSeek-R1-Distill-Qwen-7B)   |
| DeepSeek-R1-Distill-Llama-8B  | [Llama-3.1-8B](https://huggingface.co/meta-llama/Llama-3.1-8B) | [🤗 HuggingFace](https://huggingface.co/deepseek-ai/DeepSeek-R1-Distill-Llama-8B)   |
| DeepSeek-R1-Distill-Qwen-14B   | [Qwen2.5-14B](https://huggingface.co/Qwen/Qwen2.5-14B) | [🤗 HuggingFace](https://huggingface.co/deepseek-ai/DeepSeek-R1-Distill-Qwen-14B)   |
|DeepSeek-R1-Distill-Qwen-32B  | [Qwen2.5-32B](https://huggingface.co/Qwen/Qwen2.5-32B) | [🤗 HuggingFace](https://huggingface.co/deepseek-ai/DeepSeek-R1-Distill-Qwen-32B)   |
| DeepSeek-R1-Distill-Llama-70B  | [Llama-3.3-70B-Instruct](https://huggingface.co/meta-llama/Llama-3.3-70B-Instruct) | [🤗 HuggingFace](https://huggingface.co/deepseek-ai/DeepSeek-R1-Distill-Llama-70B)   |

</div>

DeepSeek-R1-Distillモデルは、DeepSeek-R1が生成したサンプルを使用してオープンソースモデルをファインチューニングしたものです。設定とトークナイザーを若干変更しています。これらのモデルを実行する際は当社の設定を使用してください。

## 4. 評価結果

### DeepSeek-R1評価
すべてのモデルで最大生成長は32,768トークンに設定されています。サンプリングを必要とするベンチマークでは、温度0.6、top-p値0.95を使用し、クエリごとに64応答を生成してpass@1を推定しています。
<div align="center">


| カテゴリ | ベンチマーク（指標） | Claude-3.5-Sonnet-1022 | GPT-4o 0513 | DeepSeek V3 | OpenAI o1-mini | OpenAI o1-1217 | DeepSeek R1 |
|----------|-------------------|----------------------|------------|--------------|----------------|------------|--------------|
| | アーキテクチャ | - | - | MoE | - | - | MoE |
| | 活性化パラメータ数 | - | - | 37B | - | - | 37B |
| | 総パラメータ数 | - | - | 671B | - | - | 671B |
| 英語 | MMLU (Pass@1) | 88.3 | 87.2 | 88.5 | 85.2 | **91.8** | 90.8 |
| | MMLU-Redux (EM) | 88.9 | 88.0 | 89.1 | 86.7 | - | **92.9** |
| | MMLU-Pro (EM) | 78.0 | 72.6 | 75.9 | 80.3 | - | **84.0** |
| | DROP (3-shot F1) | 88.3 | 83.7 | 91.6 | 83.9 | 90.2 | **92.2** |
| | IF-Eval (Prompt Strict) | **86.5** | 84.3 | 86.1 | 84.8 | - | 83.3 |
| | GPQA-Diamond (Pass@1) | 65.0 | 49.9 | 59.1 | 60.0 | **75.7** | 71.5 |
| | SimpleQA (Correct) | 28.4 | 38.2 | 24.9 | 7.0 | **47.0** | 30.1 |
| | FRAMES (Acc.) | 72.5 | 80.5 | 73.3 | 76.9 | - | **82.5** |
| | AlpacaEval2.0 (LC-winrate) | 52.0 | 51.1 | 70.0 | 57.8 | - | **87.6** |
| | ArenaHard (GPT-4-1106) | 85.2 | 80.4 | 85.5 | 92.0 | - | **92.3** |
| コード | LiveCodeBench (Pass@1-COT) | 33.8 | 34.2 | - | 53.8 | 63.4 | **65.9** |
| | Codeforces (パーセンタイル) | 20.3 | 23.6 | 58.7 | 93.4 | **96.6** | 96.3 |
| | Codeforces (レーティング) | 717 | 759 | 1134 | 1820 | **2061** | 2029 |
| | SWE Verified (解決済み) | **50.8** | 38.8 | 42.0 | 41.6 | 48.9 | 49.2 |
| | Aider-Polyglot (Acc.) | 45.3 | 16.0 | 49.6 | 32.9 | **61.7** | 53.3 |
| 数学 | AIME 2024 (Pass@1) | 16.0 | 9.3 | 39.2 | 63.6 | 79.2 | **79.8** |
| | MATH-500 (Pass@1) | 78.3 | 74.6 | 90.2 | 90.0 | 96.4 | **97.3** |
| | CNMO 2024 (Pass@1) | 13.1 | 10.8 | 43.2 | 67.6 | - | **78.8** |
| 中国語 | CLUEWSC (EM) | 85.4 | 87.9 | 90.9 | 89.9 | - | **92.8** |
| | C-Eval (EM) | 76.7 | 76.0 | 86.5 | 68.9 | - | **91.8** |
| | C-SimpleQA (Correct) | 55.4 | 58.7 | **68.0** | 40.3 | - | 63.7 |

</div>


### 蒸留モデル評価


<div align="center">

| モデル                                    | AIME 2024 pass@1 | AIME 2024 cons@64 | MATH-500 pass@1 | GPQA Diamond pass@1 | LiveCodeBench pass@1 | CodeForces レーティング |
|------------------------------------------|------------------|-------------------|-----------------|----------------------|----------------------|-------------------|
| GPT-4o-0513                          | 9.3              | 13.4              | 74.6            | 49.9                 | 32.9                 | 759               |
| Claude-3.5-Sonnet-1022             | 16.0             | 26.7                 | 78.3            | 65.0                 | 38.9                 | 717               |
| o1-mini                              | 63.6             | 80.0              | 90.0            | 60.0                 | 53.8                 | **1820**          |
| QwQ-32B-Preview                              | 44.0             | 60.0                 | 90.6            | 54.5               | 41.9                 | 1316              |
| DeepSeek-R1-Distill-Qwen-1.5B       | 28.9             | 52.7              | 83.9            | 33.8                 | 16.9                 | 954               |
| DeepSeek-R1-Distill-Qwen-7B          | 55.5             | 83.3              | 92.8            | 49.1                 | 37.6                 | 1189              |
| DeepSeek-R1-Distill-Qwen-14B         | 69.7             | 80.0              | 93.9            | 59.1                 | 53.1                 | 1481              |
| DeepSeek-R1-Distill-Qwen-32B        | **72.6**         | 83.3              | 94.3            | 62.1                 | 57.2                 | 1691              |
| DeepSeek-R1-Distill-Llama-8B         | 50.4             | 80.0              | 89.1            | 49.0                 | 39.6                 | 1205              |
| DeepSeek-R1-Distill-Llama-70B        | 70.0             | **86.7**          | **94.5**        | **65.2**             | **57.5**             | 1633              |

</div>


## 5. チャットサイト & APIプラットフォーム
DeepSeek-R1とのチャットは公式サイト [chat.deepseek.com](https://chat.deepseek.com) で可能で、"DeepThink"ボタンを有効にしてください。

DeepSeekプラットフォームでOpenAI互換APIも提供しています: [platform.deepseek.com](https://platform.deepseek.com/)

## 6. ローカルでの実行方法

### DeepSeek-R1モデル

**Hugging FaceのDeepSeek-R1をローカルデプロイする際、以下のハードウェア/OSSが使用可能です：**

1. **DeepSeek-Inferデモ**: FP8/BF16推論用簡易デモ
2. **SGLang**: BF16/FP8推論完全サポート（Multi-Token Predictionは[開発中](https://github.com/sgl-project/sglang/issues/2591)）
3. **LMDeploy**: 効率的なFP8/BF16推論（ローカル/クラウド）
4. **TensorRT-LLM**: BF16推論＆INT4/8量子化対応（FP8開発中）
5. **vLLM**: FP8/BF16モードでテンソル並列/パイプライン並列を実現
6. **AMD GPU**: SGLang経由でBF16/FP8モード実行可能
7. **Huawei Ascend NPU**: Ascendデバイス対応

公式提供FP8重みのみ（BF16変換要スクリプト）：
```shell
cd inference
python fp8_cast_bf16.py --input-fp8-hf-path /path/to/fp8_weights --output-bf16-hf-path /path/to/bf16_weights
```

> **重要**: Hugging Face Transformersの直接サポートは非対応

---

### **6.1 DeepSeek-Inferデモでの実行例**

#### 準備作業
```shell
git clone https://huggingface.co/deepseek-ai/DeepSeek-R1
cd DeepSeek-R1/inference
pip install -r requirements.txt
```

#### 重み変換
```shell
python convert.py --hf-ckpt-path /path/to/DeepSeek-R1 --save-path /path/to/DeepSeek-R1-Demo --n-experts 256 --model-parallel 16
```

#### 実行方法
対話モード：
```shell
torchrun --nnodes 2 --nproc-per-node 8 --node-rank $RANK --master-addr $ADDR generate.py --ckpt-path /path/to/DeepSeek-R1-Demo --config configs/config_r1.json --interactive --temperature 0.7 --max-new-tokens 200
```

バッチ処理：
```shell
torchrun --nnodes 2 --nproc-per-node 8 --node-rank $RANK --master-addr $ADDR generate.py --ckpt-path /path/to/DeepSeek-R1-Demo --config configs/config_r1.json --input-file $FILE
```

---

### **6.2 SGLang実装（推奨）**
[SGLang](https://github.com/sgl-project/sglang) v0.4.1対応機能：
- MLA最適化
- DP Attention
- FP8（W8A8）＆FP8 KVキャッシュ
- マルチノードテンソル並列（[H208構成例](https://github.com/sgl-project/sglang/tree/main/benchmark/deepseek_r1)）
- NVIDIA/AMD GPU両対応

詳細手順：  
https://github.com/sgl-project/sglang/tree/main/benchmark/deepseek_r1

---

### **6.3 LMDeploy実装（推奨）**
[LMDeploy](https://github.com/InternLM/lmdeploy)対応事項：
- PyTorch連携可能
- 実装ガイド：  
https://github.com/InternLM/lmdeploy/issues/2960

---

### **6.4 TensorRT-LLM実装（推奨）**
[TensorRT-LLM](https://github.com/NVIDIA/TensorRT-LLM)専用ブランチ：
- BF16/INT4/INT8量子化（FP8開発中）
- カスタムブランチ：  
https://github.com/NVIDIA/TensorRT-LLM/tree/deepseek/examples/deepseek_r1

---

### **6.5 vLLM実装（推奨）**
[vLLM](https://github.com/vllm-project/vllm) v0.6.6対応：
- FP8/BF16モード（NVIDIA/AMD GPU）
- 分散処理詳細：  
https://docs.vllm.ai/en/latest/serving/distributed_serving.html

---

### **6.6 AMD GPU最適設定**
AMD公式連携機能：
- SGLang経由のFP8/BF16完全サポート
- 実装手順：[6.3項](##6.3 LMDeploy実装（推奨）)参照

---

### **6.7 Huawei Ascend対応**
[MindIE](https://www.hiascend.com/en/software/mindie)フレームワーク：
- BF16版DeepSeek-R1適応
- 実装マニュアル：  
https://modelers.cn/models/MindIE/deepseekr1

---

### DeepSeek-R1-Distillモデル

DeepSeek-R1-DistillモデルはQwenやLlamaモデルと同様に使用できます。

例えば、[vLLM](https://github.com/vllm-project/vllm)を使用して簡単にサービスを開始できます:

```shell
vllm serve deepseek-ai/DeepSeek-R1-Distill-Qwen-32B --tensor-parallel-size 2 --max-model-len 32768 --enforce-eager
```

また、[SGLang](https://github.com/sgl-project/sglang)を使用してサービスを開始することも可能です:

```bash
python3 -m sglang.launch_server --model deepseek-ai/DeepSeek-R1-Distill-Qwen-32B --trust-remote-code --tp 2
```

---

### 使用上の推奨事項

**DeepSeek-R1シリーズモデルを活用する際（ベンチマークを含む）、期待される性能を達成するためには以下の設定を遵守することを推奨します:**

1. 無限ループや支離滅裂な出力を防ぐため、温度パラメータは0.5-0.7の範囲に設定（0.6が推奨）
2. **システムプロンプトを追加せず、全ての指示はユーザープロンプト内に含めること**
3. 数学問題では「段階的に推論し、最終答えを\boxed{}内に記載してください」などの指示をプロンプトに含めることが望ましい
4. モデル性能評価時には複数回のテストを実施し、結果を平均化することが推奨される

## 7. ライセンス
このコードリポジトリとモデルウェイトは[MITライセンス](https://github.com/deepseek-ai/DeepSeek-R1/blob/main/LICENSE)の下で提供されます。
DeepSeek-R1シリーズは商用利用をサポートし、他のLLMの訓練用蒸留を含むあらゆる修正・派生作品を許可します。ただし以下に注意:
- DeepSeek-R1-Distill-Qwen-1.5B、DeepSeek-R1-Distill-Qwen-7B、DeepSeek-R1-Distill-Qwen-14B、DeepSeek-R1-Distill-Qwen-32Bは[Apache 2.0ライセンス](https://huggingface.co/Qwen/Qwen2.5-1.5B/blob/main/LICENSE)の[Qwen-2.5シリーズ](https://github.com/QwenLM/Qwen2.5)をベースに、DeepSeek-R1でキュレートした80万サンプルでファインチューニングされています。
- DeepSeek-R1-Distill-Llama-8Bは[Llama3.1-8B-Base](https://huggingface.co/meta-llama/Llama-3.1-8B/blob/main/LICENSE)をベースにしています。
- DeepSeek-R1-Distill-Llama-70Bは[Llama3.3-70B-Instruct](https://huggingface.co/meta-llama/Llama-3.3-70B-Instruct/blob/main/LICENSE)をベースにしています。

## 8. 引用
```
@misc{deepseekai2025deepseekr1incentivizingreasoningcapability,
      title={DeepSeek-R1: Incentivizing Reasoning Capability in LLMs via Reinforcement Learning}, 
      author={DeepSeek-AI and Daya Guo and Dejian Yang and Haowei Zhang and Junxiao Song and Ruoyu Zhang and Runxin Xu and Qihao Zhu and Shirong Ma and Peiyi Wang and Xiao Bi and Xiaokang Zhang and Xingkai Yu and Yu Wu and Z. F. Wu and Zhibin Gou and Zhihong Shao and Zhuoshu Li and Ziyi Gao and Aixin Liu and Bing Xue and Bingxuan Wang and Bochao Wu and Bei Feng and Chengda Lu and Chenggang Zhao and Chengqi Deng and Chenyu Zhang and Chong Ruan and Damai Dai and Deli Chen and Dongjie Ji and Erhang Li and Fangyun Lin and Fucong Dai and Fuli Luo and Guangbo Hao and Guanting Chen and Guowei Li and H. Zhang and Han Bao and Hanwei Xu and Haocheng Wang and Honghui Ding and Huajian Xin and Huazuo Gao and Hui Qu and Hui Li and Jianzhong Guo and Jiashi Li and Jiawei Wang and Jingchang Chen and Jingyang Yuan and Junjie Qiu and Junlong Li and J. L. Cai and Jiaqi Ni and Jian Liang and Jin Chen and Kai Dong and Kai Hu and Kaige Gao and Kang Guan and Kexin Huang and Kuai Yu and Lean Wang and Lecong Zhang and Liang Zhao and Litong Wang and Liyue Zhang and Lei Xu and Leyi Xia and Mingchuan Zhang and Minghua Zhang and Minghui Tang and Meng Li and Miaojun Wang and Mingming Li and Ning Tian and Panpan Huang and Peng Zhang and Qiancheng Wang and Qinyu Chen and Qiushi Du and Ruiqi Ge and Ruisong Zhang and Ruizhe Pan and Runji Wang and R. J. Chen and R. L. Jin and Ruyi Chen and Shanghao Lu and Shangyan Zhou and Shanhuang Chen and Shengfeng Ye and Shiyu Wang and Shuiping Yu and Shunfeng Zhou and Shuting Pan and S. S. Li and Shuang Zhou and Shaoqing Wu and Shengfeng Ye and Tao Yun and Tian Pei and Tianyu Sun and T. Wang and Wangding Zeng and Wanjia Zhao and Wen Liu and Wenfeng Liang and Wenjun Gao and Wenqin Yu and Wentao Zhang and W. L. Xiao and Wei An and Xiaodong Liu and Xiaohan Wang and Xiaokang Chen and Xiaotao Nie and Xin Cheng and Xin Liu and Xin Xie and Xingchao Liu and Xinyu Yang and Xinyuan Li and Xuecheng Su and Xuheng Lin and X. Q. Li and Xiangyue Jin and Xiaojin Shen and Xiaosha Chen and Xiaowen Sun and Xiaoxiang Wang and Xinnan Song and Xinyi Zhou and Xianzu Wang and Xinxia Shan and Y. K. Li and Y. Q. Wang and Y. X. Wei and Yang Zhang and Yanhong Xu and Yao Li and Yao Zhao and Yaofeng Sun and Yaohui Wang and Yi Yu and Yichao Zhang and Yifan Shi and Yiliang Xiong and Ying He and Yishi Piao and Yisong Wang and Yixuan Tan and Yiyang Ma and Yiyuan Liu and Yongqiang Guo and Yuan Ou and Yuduan Wang and Yue Gong and Yuheng Zou and Yujia He and Yunfan Xiong and Yuxiang Luo and Yuxiang You and Yuxuan Liu and Yuyang Zhou and Y. X. Zhu and Yanhong Xu and Yanping Huang and Yaohui Li and Yi Zheng and Yuchen Zhu and Yunxian Ma and Ying Tang and Yukun Zha and Yuting Yan and Z. Z. Ren and Zehui Ren and Zhangli Sha and Zhe Fu and Zhean Xu and Zhenda Xie and Zhengyan Zhang and Zhewen Hao and Zhicheng Ma and Zhigang Yan and Zhiyu Wu and Zihui Gu and Zijia Zhu and Zijun Liu and Zilin Li and Ziwei Xie and Ziyang Song and Zizheng Pan and Zhen Huang and Zhipeng Xu and Zhongyu Zhang and Zhen Zhang},
      year={2025},
      eprint={2501.12948},
      archivePrefix={arXiv},
      primaryClass={cs.CL},
      url={https://arxiv.org/abs/2501.12948}, 
}

```

## 9. 連絡先
質問がある場合はIssueを立てるか、[service@deepseek.com](service@deepseek.com)までご連絡ください。
