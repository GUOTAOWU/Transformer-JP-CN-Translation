# Transformerアーキテクチャに基づく日中機械翻訳 (Japanese-Chinese Translation based on Transformer)

## 1. プロジェクト紹介 (Project Overview)
[cite_start]本プロジェクトは、PyTorchフレームワークを用いて完全なTransformerアーキテクチャを実装し、日本語から中国語への機械翻訳を行うモデルを構築したものです[cite: 3, 11]。
[cite_start]近年、日中間の交流において高精度な翻訳技術が求められています。本研究では、従来のRNNやLSTMが抱える長距離依存の問題を、自己注意機構（Self-Attention Mechanism）を用いることで解決し、翻訳精度の向上と学習効率の改善を図りました[cite: 11, 14]。

主な特徴：
* **完全なTransformerアーキテクチャ**: エンコーダ・デコーダ構造の実装。
* **自己注意機構**: 長文の依存関係を捉えるMulti-Head Attentionの採用。
* **高効率**: RNNと比較して学習時間を大幅に短縮しつつ、高い精度を実現。

## 2. ファイル構成 (File Structure)
*(※注意：以下は想定される構成です。実際のファイル名に合わせて修正してください)*

* [cite_start]`model.py`: Transformerモデルの定義（Encoder, Decoder, Multi-Head Attention, Positional Encodingなど）[cite: 42]。
* `train.py`: モデルの学習ループ、損失関数、最適化アルゴリズムの実装。
* `evaluate.py`: BLEUスコアによるモデル評価用スクリプト。
* `data_loader.py`: データの読み込みおよび前処理（Tokenization, Embedding）を行うスクリプト。
* `requirements.txt`: 必要なライブラリ（PyTorch, SentencePieceなど）の一覧。

## 3. データセット (Dataset)
[cite_start]学習には、NTTが作成した大規模な公開日英/日中パラレルコーパスである**JParaCrawl**を使用しています[cite: 41]。

* **データソース**: JParaCrawl
* [cite_start]**前処理**: SentencePieceモデルを使用して、日本語および中国語のテキストをトークン化（Tokenization）しています[cite: 41]。

## 4. 使用手法とアルゴリズム (Methodology)
[cite_start]本プロジェクトでは、Vaswaniら(2017)によって提案された**Transformer**アーキテクチャを採用しています[cite: 18]。

### アーキテクチャ詳細
* [cite_start]**モデル構造**: Seq2Seq Transformer (Encoder-Decoder)[cite: 28]。
* [cite_start]**Embedding**: 単語を固定次元のベクトルに変換し、Positional Encodingを加算して位置情報を保持[cite: 30]。
* [cite_start]**Attention機構**: Scaled Dot-Product AttentionおよびMulti-Head Attentionにより、並列的に文脈情報を抽出[cite: 36, 38]。
* [cite_start]**損失関数**: CrossEntropyLoss (交差エントロピー損失)[cite: 52]。
* [cite_start]**最適化アルゴリズム**: Adam (Adaptive Moment Estimation)[cite: 52]。

## 5. 実装の詳細 (Implementation Details)
[cite_start]具体的な実装パラメータは以下の通りです[cite: 53-61]。

| ハイパーパラメータ | 設定値 |
| :--- | :--- |
| エンコーダ・デコーダ層数 (Layers) | 3 |
| 単語埋め込み次元 (Embedding Dim) | 512 |
| Multi-Head Attention ヘッド数 | 8 |
| Feed-Forward 隠れ層次元 | 512 |
| 学習バッチサイズ (Batch Size) | 16 (※総エポック数: 16) |
| 学習率 (Learning Rate) | 0.0001 |
| Adam パラメータ (Betas) | (0.9, 0.98) |

**実施プロセス**:
1.  **データ前処理**: JParaCrawlデータを読み込み、SentencePieceで分かち書きを行う。
2.  **モデル構築**: PyTorchを用いてTransformerクラスを定義。
3.  **学習**: マスキング（Masking）処理を行いながら、予測単語と正解単語の誤差を最小化するように学習。
4.  **評価**: テストデータを用いて翻訳を生成し、BLEUスコアを算出。

## 6. 結論と結果 (Results & Conclusion)
実験の結果、Transformerモデルは従来のRNNモデルと比較して圧倒的な学習効率と精度を示しました。

* [cite_start]**学習時間**: わずか1時間の学習で収束[cite: 65]。
* [cite_start]**精度 (BLEUスコア)**: **0.6414** を記録[cite: 63, 65]。
* [cite_start]**比較**: 同条件で学習したRNNモデルのBLEUスコアは0.05にとどまり、Transformerの優位性が実証されました[cite: 67]。

本モデルにより、日本語から中国語への基礎的かつ高精度な翻訳機能が実現されました。

---
**参考文献**
* [1] Attention is All You Need (Vaswani et al., 2017)
* [2] Neural Machine Translation by Jointly Learning to Align and Translate (Bahdanau et al., 2014)
