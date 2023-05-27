# HCAParser is followed https://github.com/nttcslab-nlp/RSTParser_EMNLP22 and adapted to Clause Parsing tasks.
Hierarchical Clause Annotation subtask - Clause Parsing

## Setup
1. prepare python environment with conda
2. clone this repository
   ```bash
   git clone https://github.com/MetroVancloud/HCAParser
   cd HCAParser
   ```
3. manually install pytorch to enable GPU support
   ```bash
   conda install pytorch cudatoolkit=XXX -c pytorch
   conda install torchtext -c pytorch
   ```
4. install dependencies
   ```bash
   pip install -r requirements.txt
   ```

## Preprocess for dataset
### RSTDT
If you have the RSTDT dataset that has been preprocessed
by [Heilman's script](https://github.com/EducationalTestingService/rstfinder.git),
you can use it without doing anything.

Each data of Heilman's has following elements.
```
- doc_id
- edu_start_indices
- edu_starts_paragraph
- edu_strings
- path_basename         # not necessary
- pos_tags              # not necessary
- rst_tree
- syntax_trees          # not necessary
- token_tree_positions  # not necessary
- tokens
```

## Train and test
```bash
bash scripts/run_shift_reduce_v1.deberta-base.1e-5.sh
```
This script was genereated by `scripts/generator_rstdt.sh`.
Some enviroment variables (`CUDA_VISIBLE_DEVICES`) are hard codede in the script.

Although the maximum is 20 epochs, it converges in about 5 epochs.
(Training time is about 1h/epoch with GeForce RTX 3090)

Models are saved into `./models/rstdt/shift_reduce_v1.deberta-base.1e-5/version_?/checkpoints/`.

Saved models are followings
```
PATH/TO/checkpoints/
- epoch=3-step=?????.ckpt  # saved in training process
- epoch=3-step=?????.ckpt  # saved in training process
- epoch=4-step=?????.ckpt  # saved in training process
- last.ckpt                # saved at the end of training process
- best.ctpt                # selected the best model by validation score in evaluation process
- average.ckpt             # output of checkpoint weight averaging (CPA) at evaluation process
```


## Test only single checkpoint
```bash
python src/test.py --ckpt-path PATH/TO/CKPT --save-dir PATH/TO/TREES/ --metrics OriginalParseval
```
