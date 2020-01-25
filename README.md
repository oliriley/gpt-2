# gpt-2

Personal copy of code from the paper ["Language Models are Unsupervised Multitask Learners"](https://d4mucfpksywv.cloudfront.net/better-language-models/language-models.pdf). See more details in associated [blog post](https://blog.openai.com/better-language-models/).

## Development

See [DEVELOPERS.md](./DEVELOPERS.md)

## Contributors

See [CONTRIBUTORS.md](./CONTRIBUTORS.md)

## Fine tuning on custom datasets

To retrain GPT-2 117M model on a custom text dataset:

```
PYTHONPATH=src ./train.py --dataset <file|directory|glob>
```

If you want to precompute the dataset's encoding for multiple runs, you can instead use:

```
PYTHONPATH=src ./encode.py <file|directory|glob> /path/to/encoded.npz
PYTHONPATH=src ./train.py --dataset /path/to/encoded.npz
```

### Gradient Checkpointing

https://github.com/openai/gradient-checkpointing is included to reduce the memory requirements of the model, and can be enabled by `--memory_saving_gradients`. The checkpoints are currently chosen manually (poorly) by just adding layer 10 to the 'checkpoints' collection in model.py. `--memory_saving_gradients` is enabled by default for training the 345M model.

### Validation loss

Set `--val_every` to a number of steps `N > 0`, and "validation" loss against a fixed sample of the dataset will be calculated every N steps to get a better sense of training progress. N around 200 suggested. You can set `--val_dataset` to choose a separate validation dataset, otherwise it defaults to a sample from the train dataset (so not a real cross-validation loss!).

### Optimizer

You can use SGD instead of Adam with `--optimizer sgd`. This also helps conserve memory when training the 345M model. Note: the learning rate needs to be adjusted for SGD, due to not having Adam's gradient normalization (0.0006 seems to be a good number from some experiments).

## Citation

Please use the following bibtex entry:
```
@article{radford2019language,
  title={Language Models are Unsupervised Multitask Learners},
  author={Radford, Alec and Wu, Jeff and Child, Rewon and Luan, David and Amodei, Dario and Sutskever, Ilya},
  year={2019}
}
```

## License

[MIT](./LICENSE)
