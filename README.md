<h2 align="center"><b><h3>Does Subword Vocabulary hold back Machine Translation?</h3></b></h2><br>

<p align="center">
  <b>Avijit Thawani</b>, Saurabh Ghanekar, Dipesh Kumar, ay Pujara
</p>
<br>

<p align="center">
  <a href="https://openreview.net/pdf?id=D0fEYV_Ru4"><b>Paper (OpenReview)</b></a><br><br>
</p>

_______

Below is the original repository that we build upon: [Factorizer](https://github.com/ltgoslo/factorizer)

<br>

## Installation

```bash
git clone https://github.com/ltgoslo/factorizer.git
cd factorizer
python3 setup.py install  
```

<br>

## Pretrained factorizer models

| **Language**    | **URL**         |
| :-------------- | :-------------- |
| Arabic (`ar`)          | [`arabic.dawg` (191 MB)](https://github.com/ltgoslo/factorizer/releases/download/v1.0.0/arabic.dawg) |
| Chinese (`zh`)         | [`chinese.dawg` (180 MB)](https://github.com/ltgoslo/factorizer/releases/download/v1.0.0/chinese.dawg)) |
| Czech (`cs`)           | [`czech.dawg` (158 MB)](https://github.com/ltgoslo/factorizer/releases/download/v1.0.0/czech.dawg) |
| English (`en`)         | [`english.dawg` (129 MB)](https://github.com/ltgoslo/factorizer/releases/download/v1.0.0/english.dawg) |
| Norwegian (`no`)       | [`norwegian.dawg` (186 MB)](https://github.com/ltgoslo/factorizer/releases/download/v1.0.0/norwegian.dawg) |
| Scottish Gaelic (`gd`) | [`gaelic.dawg` (187 MB)](https://github.com/ltgoslo/factorizer/releases/download/v1.0.0/gaelic.dawg) |
| Turkish (`tr`)         | [`turkish.dawg` (206 MB)](https://github.com/ltgoslo/factorizer/releases/download/v1.0.0/turkish.dawg) |

<br>

## Usage

```python
from factorizer import Factorizer


tokenizer = Factorizer("english.dawg")
sentence = "The echo of a distant time comes willowing across the sand, and everything is green and submarine."

encoding = tokenizer(sentence)

print(f"INPUT:    {sentence}")
print(f"SUBWORDS: {' '.join(encoding.tokens)}")
print(f"INDICES:  {' '.join(str(index) for index in encoding.ids)}")
print(f"DECODED:  {tokenizer.decode(encoding.ids}")
```

This should output:
```
INPUT:    The echo of a distant time comes willowing across the sand, and everything is green and submarine.
SUBWORDS: ⸥The⸤ ⸥echo⸤ ⸥of⸤ ⸥a⸤ ⸥distant⸤ ⸥time⸤ ⸥comes⸤ ⸥wil lowing⸤ ⸥across⸤ ⸥the⸤ ⸥sand ,⸤ ⸥and⸤ ⸥everything⸤ ⸥is⸤ ⸥green⸤ ⸥and⸤ ⸥submarine .⸤
INDICES:  (52, 74, 62) (221, 21, 77) (135, 64, 137) (181, 45, 79) (248, 77, 122) (88, 92, 159) (124, 92, 64) (49, 151, 114) (79, 180, 104) (129, 186, 151) (52, 74, 219) (49, 127, 34) (35, 174, 39) (76, 101, 35) (32, 176, 191) (135, 209, 205) (44, 28, 242) (76, 101, 35) (13, 171, 144) (211, 41, 131)
DECODED:  The echo of a distant time comes willowing across the sand, and everything is green and submarine.
```

<br>

## Documentation

#### class Encoding:

A named tuple containing:
- `ids` (List[Tuple[int, int, int]])
- `tokens` (List[str])
- `perplexities` (List[float])
- `offsets` (List[Tuple[int, int]])

#### `Factorizer.__init__`

| **Argument**    | **Description** |
| :-------------- | :-------------- |
| `tokenizer_path` (str) | path to a DAWG file containing with pretrained vocabulary |
| `alpha` (float) | the alpha_split hyperparameter controling the granularity of subword splits *(default: 0.1)* |
| `sigma` (float)           | the sigma_sample hyperparameter controling the randomness (temperature) of sampling *(default: 0.0)* (no sampling) |
| `merge_unks` (bool)       | set this argument to True if you want to merge consecutive UNK tokens *(default: True)* |
| `allow_decoding` (bool)       | set this argument to True if you want to precompute the inverse vocabulary for decoding *(default: False)* |
| `sample` (bool)       | set this argument to True if you want to sample from the subword distribution; set to False if you want to always do the optimal tokenization *(default: False)* |

<br>

#### `Factorizer.__call__`

Factorizes the input string (or list of strings)

| **Argument**    | **Description** |
| :-------------- | :-------------- |
| `text` (Union[str, List[str]]) | the input string (or list of strings) |

**Returns**: Union[Encoding, List[Encoding]]

<br>

#### `Factorizer.encode`

The same functions as `Factorizer.__call__`

<br>

#### `Factorizer.decode`

Takes the factorized indices and decodes them back to string (also accepts a batched input)

| **Argument**    | **Description** |
| :-------------- | :-------------- |
| `indices` (Union[List[Tuple[int, int, int]], List[List[Tuple[int, int, int]]]]) | the factorized indices |

**Returns**: Union[str, List[str]]


<br>


## Please cite the following publication
```bibtex
@inproceedings{samuel-ovrelid-2023-tokenization,
    title = "Tokenization with Factorized Subword Encoding",
    author = "Samuel, David  and
      {\O}vrelid, Lilja",
    editor = "Rogers, Anna  and
      Boyd-Graber, Jordan  and
      Okazaki, Naoaki",
    booktitle = "Findings of the Association for Computational Linguistics: ACL 2023",
    month = jul,
    year = "2023",
    address = "Toronto, Canada",
    publisher = "Association for Computational Linguistics",
    url = "https://aclanthology.org/2023.findings-acl.890",
    doi = "10.18653/v1/2023.findings-acl.890",
    pages = "14143--14161",
    abstract = "In recent years, language models have become increasingly larger and more complex. However, the input representations for these models continue to rely on simple and greedy subword tokenization methods. In this paper, we propose a novel tokenization method that factorizes subwords onto discrete triplets using a VQ-VAE model. The effectiveness of the proposed tokenization method, referred to as the Factorizer, is evaluated on language modeling and morpho-syntactic tasks for 7 diverse languages. Results indicate that this method is more appropriate and robust for morphological tasks than the commonly used byte-pair encoding (BPE) tokenization algorithm.",
}
```
