# NIM ブートキャンプ

このブートキャンプは、開発者が実際のGenAIアプリケーションを構築することで、NVIDIA® NIM™を使い始めることができるように設計されています。
ラボでは、参加者がNIM Dockerコンテナをセットアップし、推論リクエストを提供するためにREST API Endpointを利用することをガイドします。さらに、参加者は、LLaMA-3 8Bモデルのアダプターをファインチューニングする実習を体験しながら、LoRAのようなPEFT（Parameter Efficient Fine-Tuning）テクニックを使ったモデルのファインチューニングを探求します。

## ラボのデプロイ

### 前提条件

このチュートリアルを実行するには、Ampereまたはそれ以降の世代の80GB GPUを最低1つ搭載したラップトップ/ワークステーション/DGXマシンが必要です。

- 最新の[Docker](https://docs.docker.com/engine/install/)をインストールし、GPUアクセスを可能にするNVIDIA Container Toolkitがインストールに含まれていることを確認してください。
- ファインチューニングのラボでは、モデルの重みをダウンロードするためにHuggingfaceセキュリティトークンが必要です。手順は[こちらのリンク]( https://huggingface.co/docs/hub/en/security-tokens )にあります。

### テスト環境

Ampere A100 GPUを搭載したDGXマシンですべてのラボをテスト・実行しました。

#### 1. 仮想環境のセットアップ

まず、このリポジトリをクローンし、プロジェクト・ディレクトリに移動します：
```bash
git clone https://github.com/openhackathons-org/NIM-Bootcamp/tree/main
cd NIM-Bootcamp
```

新しい仮想環境を作成し、アクティブにします：
```bash
# Create virtual environment
python -m venv nim-bootcamp-env

# Activate virtual environment
source nim-bootcamp-env/bin/activate
```

#### 2. 必要なパッケージのインストール

仮想環境を起動した状態で、必要なパッケージをインストールします：
```bash
# Upgrade pip (recommended)
pip install --upgrade pip

# Install requirements
pip install -r https://github.com/openhackathons-org/NIM-Bootcamp/blob/main/bootcamp_requirements.txt  
```

#### 3. GPU アクセスの検証

お使いの環境が GPU リソースにアクセスできることを確認するには、以下の Python コマンドを実行します：
```python
import torch

# Check if CUDA is available
print(f"CUDA available: {torch.cuda.is_available()}")

# If CUDA is available, show device information
if torch.cuda.is_available():
    print(f"Current device: {torch.cuda.get_device_name(0)}")
    print(f"Device count: {torch.cuda.device_count()}")
```

これらのコマンドは、Pythonターミナルで実行することも、簡単なスクリプトを作成することもできます。

#### 4. JupyterLab の起動

ポート8888でJupyterLabを起動します：
```bash
#Choose the desired workspace:
cd workspace-nim-with-rag
# Basic start
jupyter lab --port 8888

# If you want to make it accessible from other machines on your network
jupyter lab --port 8888 --ip 0.0.0.0

# If you want to specify a particular browser
jupyter lab --port 8888 --browser="chrome"
```

コマンドを実行すると、次のような出力が表示されるはずです：
```
[I 2025-01-29 10:00:00.000 LabApp] JupyterLab extension loaded from /path/to/extension
[I 2025-01-29 10:00:00.000 LabApp] JupyterLab application directory is /path/to/app
[I 2025-01-29 10:00:00.000 ServerApp] Serving notebooks from local directory: /path/to/your/project
[I 2025-01-29 10:00:00.000 ServerApp] Jupyter Server 1.x is running at:
[I 2025-01-29 10:00:00.000 ServerApp] http://localhost:8888/lab
```

出力されたURLをコピーし、ブラウザに貼り付けます。トークンの入力を求められたら、ターミナルの出力でそれを見つけることができます。

#### トラブルシューティング

何らかの問題が発生した場合:

1. **仮想環境の問題**
   - 仮想環境を作成する際に、正しいディレクトリにいることを確認してください。
   - 仮想環境が有効になっていることを確認します。(ターミナルプロンプトに `(nim-bootcamp-env)` と表示されるはずです)

2. **Package Installation Issues**
   - Try updating pip before installing requirements: `pip install --upgrade pip`
   - If a package fails to install, try installing it separately

3. **GPU アクセスの問題**
   - NVIDIAドライバが正しくインストールされているか確認します。
   - CUDAツールキットがインストールされていて、PyTorchのバージョンと一致しているか確認します。
   - ターミナルで `nvidia-smi` を実行して GPU が認識されていることを確認します。

4. **JupyterLab アクセスの問題**
   - ポート8888が他のアプリケーションによって使用されていないか確認してください。
   - 他のマシンからアクセスする場合、ファイアウォール設定が接続を許可していることを確認します。
   - ポート8888が使用できない場合は、別のポートを試してください。

追加のヘルプが必要な場合は、GitHubリポジトリでissueをオープンして下さい。

`http://localhost:8888`でブラウザを開き、`Start_Here.ipynb`をクリックします。
残りのラボの作業が終わり次第、`File > Shut Down`を選択してjupyterラボをシャットダウンし、ターミナルウィンドウで`exit`とタイプするか、`ctrl+d`を押してコンテナをシャットダウンします。

これでNIM Bootcampのビルドとデプロイは完了です！









