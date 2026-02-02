# Train Piper

Follow the setup instructions at [ilspeech-train/piper.txt](https://github.com/thewh1teagle/ilspeech-train/blob/main/piper.txt), then run:

```console
uv pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu130

uv run python -m piper_train \
    --dataset-dir "./train" \
    --accelerator 'gpu' \
    --devices 1 \
    --batch-size 24 \
    --validation-split 0 \
    --num-test-examples 0 \
    --max_epochs 990000 \
    --resume_from_checkpoint ./epoch=4641-step=3104302.ckpt \
    --checkpoint-epochs 1 \
    --precision 32
```