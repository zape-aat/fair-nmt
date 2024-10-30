# fair-nmt

Fair-NMT are translation models based on Opus-MT. By fine-tuning the original models, we can achieve better translation performance.

## Introduction
- Translation models based on Opus-MT.
- Lightweight and fast when used with ctranslate2.
- MIT licensed.
  
## Models
Traditional Chinese to English:  
[fair-nmt-zh_hant-en](https://huggingface.co/aarontseng/fair-nmt-zh_hant-en)

English to Traditional Chinese:  
[fair-nmt-en-zh_hant](https://huggingface.co/aarontseng/fair-nmt-en-zh_hant) 

English to Simplified Chinese:  
[fair-nmt-en-zh](https://huggingface.co/aarontseng/fair-nmt-en-zh)

## Quick start

```
from transformers import pipeline

ls = [
  'On Monday, scientists from the Stanford University School of Medicine announced the invention of a new diagnostic tool that can sort cells by type: a tiny printable chip that can be manufactured using standard inkjet printers for possibly about one U.S. cent each.',
]
translator = pipeline("translation", model="aarontseng/fair-nmt-en-zh_hant", device=0)
result = translator(ls)
print(result)

# 週一,史丹佛大學醫學院的科學家 宣佈發明了一種新的診斷工具, 可以根據類型對細胞進行分類: 一種微小的 可列印晶片, 可以使用標準噴墨印表機製造, 每個大約一美分。
```

## CTranslate2

```
ct2-transformers-converter --model aarontseng/fair-nmt-en-zh_hant --output_dir fair-nmt-en-zh_hant

#quantization
ct2-transformers-converter --model aarontseng/fair-nmt-en-zh_hant --output_dir fair-nmt-en-zh_hant --quantization int8_float32
```

```
import ctranslate2
import transformers

#import os
#os.environ["KMP_DUPLICATE_LIB_OK"]="TRUE"

translator = ctranslate2.Translator("fair-nmt-en-zh_hant")
tokenizer = transformers.AutoTokenizer.from_pretrained("aarontseng/fair-nmt-en-zh_hant")

source = tokenizer.convert_ids_to_tokens(tokenizer.encode("On Monday, scientists from the Stanford University School of Medicine announced the invention of a new diagnostic tool that can sort cells by type: a tiny printable chip that can be manufactured using standard inkjet printers for possibly about one U.S. cent each."))
results = translator.translate_batch([source])
target = results[0].hypotheses[0]

print(tokenizer.decode(tokenizer.convert_tokens_to_ids(target)))

#週一,史丹佛大學醫學院的科學家宣佈發明了一種新的診斷工具,可以根據類型對細胞進行分類:一種小的可列印晶片,可以使用標準噴墨打印機製造,每個可能只有約一美分。
```
