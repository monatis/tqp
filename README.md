# tqp
Dataset and pretrained model for question paraphrasing in Turkish

## Overview
I am releasing a relatively large dataset and a pretrained model for question paraphrasing in Turkish in this repo.

## Motivation
Question paraphrasing is an important task in natural language processing with a diverse set of application areas, but Turkish lacks a publically usable dataset for this task. [Only this dataset](https://github.com/savasy/QuestionParaphrasesForTurkish) provides 1377 question paraphrases in Turkish, but it is more suitable for paraphrasing detection than a generation task. Therefore, I decided to form and share a larger dataset to be used to train a question paraphrasing model in Turkish.

## Usage
You can use the[pretrained model](https://huggingface.co/mys/mt5-small-turkish-question-paraphrasing) through Transformers Library.  The following simple code is enough to generate 5 paraphrases for the input question.

```python
from transformers import AutoTokenizer, T5ForConditionalGeneration
model_name = "mys/mt5-small-turkish-question-paraphrasing"
tokenizer = AutoTokenizer.from_pretrained(model_name)
model = T5ForConditionalGeneration.from_pretrained(model_name)

tokens = tokenizer.encode_plus("Yarın toplantı kaçta başlıyor?", return_tensors='pt')
paraphrases = model.generate(tokens['input_ids'], max_length=128, num_return_sequences=5, num_beams=5)
tokenizer.batch_decode(paraphrases, skip_special_tokens=True)
```

And the output will be something like:
```shell
['Yarın toplantı ne zaman başlıyor?',
 'Yarın toplantı saat kaçta başlıyor?',
 'Yarın toplantı saat kaçta başlar?',
 'Yarın toplantı ne zaman başlayacak?',
 'Yarın toplantı ne zaman başlar?']
```

## Building Dataset
I first downloaded [TaPaCo: A Corpus of Sentential Paraphrases for 73 Languages](https://www.aclweb.org/anthology/2020.lrec-1.848/) and extracted `tr.txt` which contains 56277 paraphrasing groups with each one having a variable number of paraphrases. Then, I selected 7384 paraphrasing groups which ends in a question mark (?). The first 84 groups were separated as a development dataset and the remaining 7300 paraphrasing groups were used as the training dataset. By rolling out sentences in each paraphrasing groups, I obtained 4588 pairs in the dev set and 51620 pairs in the train set. You can find them in the `data` directory in this repo.

## Considerations and Future work
A quick review of the dataset revealed that some sentences does not sound natural in Turkish (i.e., probably translated from another language), or some pairs may be questionable from one or more semantic perspectives. An upcoming study should fix these issues and further clean the data, but the dataset will be available as V0.1 until then as the pretrained model seems to be giving satisfactory results. 

## Citation
If you find this repo useful for your research, please [consider citation](https://zenodo.org/record/4719801#.YIatzJAzZPY).

## License
Creative Commons Attribution 2.0 Generic (CC BY 2.0)