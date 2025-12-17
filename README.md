# Transformerアーキテクチャに基づく日中機械翻訳 (Japanese-Chinese Translation based on Transformer)

## 1. プロジェクト紹介 (Project Overview)
本プロジェクトは、PyTorchフレームワークを用いて完全なTransformerアーキテクチャを実装し、日本語から中国語への機械翻訳を行うモデルを構築したものです。
近年、日中間の交流において高精度な翻訳技術が求められています。本研究では、従来のRNNやLSTMが抱える長距離依存の問題を、自己注意機構（Self-Attention Mechanism）を用いることで解決し、翻訳精度の向上と学習効率の改善を図りました。

主な特徴：
* **完全なTransformerアーキテクチャ**: エンコーダ・デコーダ構造の実装。
* **自己注意機構**: 長文の依存関係を捉えるMulti-Head Attentionの採用。
* **高効率**: RNNと比較して学習時間を大幅に短縮しつつ、高い精度を実現。

## 2. ファイル構成 (File Structure)

* `コード.ipynb`: 本プロジェクトの実行コード（データ前処理、Transformerモデルの定義、学習、評価のすべてを含むJupyter Notebook）。
* `repoto.pdf`: プロジェクトの報告書（詳細な仕様と実験結果の説明）。
* `データ/`: データセット関連ファイルフォルダ。
    * `spm.ja.nopretok.model` / `spm.ja.nopretok.vocab`: 日本語用SentencePieceモデルおよび語彙ファイル。
    * `spm.zh.nopretok.model` / `spm.zh.nopretok.vocab`: 中国語用SentencePieceモデルおよび語彙ファイル。
    * `zh-ja/zh-ja/`: データセットのライセンス・引用情報フォルダ。
        * `LICENSE`: 利用規約（日本電信電話株式会社(NTT)提供。研究目的限定、商用利用不可）。
        * `CITATION`: データセットの引用に関する情報。
    * *(※注: 実験に使用した対訳データセット本体はサイズが大きいため、リポジトリには含まれていません)*

## 3. データセット (Dataset)
学習には、NTTが作成した大規模な公開日英/日中パラレルコーパスである**JParaCrawl**を使用しています。

* **データソース**: JParaCrawl
* **前処理**: SentencePieceモデルを使用して、日本語および中国語のテキストをトークン化（Tokenization）しています。

## 4. 使用手法とアルゴリズム (Methodology)
本プロジェクトでは、Vaswaniら(2017)によって提案された**Transformer**アーキテクチャを採用しています。

### アーキテクチャ詳細
* **モデル構造**: Seq2Seq Transformer (Encoder-Decoder)。
* **Embedding**: 単語を固定次元のベクトルに変換し、Positional Encodingを加算して位置情報を保持。
* **Attention機構**: Scaled Dot-Product AttentionおよびMulti-Head Attentionにより、並列的に文脈情報を抽出。
* **損失関数**: CrossEntropyLoss (交差エントロピー損失)。
* **最適化アルゴリズム**: Adam (Adaptive Moment Estimation)。

## 5. 実装の詳細 (Implementation Details)
具体的な実装パラメータは以下の通りです。

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

* **学習時間**: わずか1時間の学習で収束。
* **精度 (BLEUスコア)**: **0.6414** を記録。
* **比較**: 同条件で学習したRNNモデルのBLEUスコアは0.05にとどまり、Transformerの優位性が実証されました。

本モデルにより、日本語から中国語への基礎的かつ高精度な翻訳機能が実現されました。
