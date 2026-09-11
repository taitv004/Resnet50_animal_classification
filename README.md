# ResNet50 Animal Classification

A PyTorch project for training a custom ResNet-style CNN to classify animal images.

This is mainly a learning project, but the repository is structured so that you can clone it, prepare the dataset, and run the training pipeline directly.

## 1. Clone the repository

```bash
git clone https://github.com/taitv004/Resnet50_animal_classification.git
cd Resnet50_animal_classification
```

## 2. Create a virtual environment

```bash
python -m venv .venv
source .venv/bin/activate
```

On Windows:

```bash
.venv\Scripts\activate
```

## 3. Install dependencies

Install PyTorch first according to your operating system and hardware:

https://pytorch.org/get-started/locally/

Then install the remaining dependencies:

```bash
pip install pillow numpy opencv-python
pip install scikit-learn tensorboard tqdm
```

## 4. Prepare the dataset

The dataset should be organized as follows:

```text
animals/
├── train/
│   ├── cat/
│   ├── dog/
│   ├── horse/
│   └── ...
│
└── test/
    ├── cat/
    ├── dog/
    ├── horse/
    └── ...
```

Each folder inside `train` and `test` represents one class.

The dataset loader in `animal.py` automatically scans these folders and assigns labels based on the class directories.

## 5. Train the model

The main training script is:

```bash
python trainresnet.py
```

You can also specify the main training parameters:

```bash
python trainresnet.py \
    --epochs 10 \
    --batch_size 128 \
    --image_size 224 \
    --root /path/to/animals
```

### Parameters

| Argument       | Description                    |         Default |
| -------------- | ------------------------------ | --------------: |
| `--epochs`     | Number of training epochs      |            `10` |
| `--batch_size` | Batch size                     |           `128` |
| `--image_size` | Input image size               |           `224` |
| `--root`       | Path to the dataset            | project default |
| `--logging`    | TensorBoard log directory      | project default |
| `--title`      | Checkpoint directory/name      | project default |
| `--checkpoint` | Path to an existing checkpoint |        optional |

The images are resized to:

```text
224 × 224
```

before being passed to the model.

## 6. Hardware

The training script automatically uses the best available device:

```text
Apple Silicon (MPS)
        ↓
NVIDIA CUDA
        ↓
CPU
```

So the project can run on:

* Apple Silicon Macs
* NVIDIA GPUs
* CPU-only machines

## 7. Resume training from a checkpoint

The training script supports loading an existing checkpoint.

For example:

```bash
python trainresnet.py \
    --checkpoint ./trained_model/last.pt
```

The checkpoint contains the model state, optimizer state, and training epoch so that training can continue from the saved state.

## 8. TensorBoard

Training statistics are logged to TensorBoard.

Start TensorBoard with:

```bash
tensorboard --logdir tensorboard
```

Then open the URL shown in the terminal.

The training process records metrics such as:

```text
Loss/train
Val/Accuracy
```

This can be useful for checking whether the model is actually learning and whether validation performance is improving.

## 9. Model

The main model is implemented in:

```text
resnet.py
```

The model is a custom ResNet-style architecture built from convolutional and bottleneck blocks.

The project also contains:

```text
simplemodel.py
```

which is a smaller CNN used for experimentation and comparison.

## 10. Project structure

```text
Resnet50_animal_classification/
│
├── animals/
│   ├── train/
│   └── test/
│
├── model_trained/
├── trained_model/
├── tensorboard/
│
├── animal.py
├── resnet.py
├── simplemodel.py
├── trainresnet.py
└── README.md
```

### Main files

`animal.py`

Dataset loading and image preprocessing.

`resnet.py`

Custom ResNet-style model.

`simplemodel.py`

Small CNN used for experimentation.

`trainresnet.py`

Main training and evaluation script.

## 11. Typical workflow

After cloning the repository, the basic workflow is:

```bash
git clone https://github.com/taitv004/Resnet50_animal_classification.git
cd Resnet50_animal_classification

python -m venv .venv
source .venv/bin/activate

pip install pillow numpy opencv-python
pip install scikit-learn tensorboard tqdm

python trainresnet.py
```

To monitor training in another terminal:

```bash
tensorboard --logdir tensorboard
```

## 12. Notes

This project is primarily intended for learning and experimentation with PyTorch and image classification.

The model architecture, training parameters, and dataset pipeline can be modified freely for experimentation.
