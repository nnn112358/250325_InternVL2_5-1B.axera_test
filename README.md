---
library_name: transformers
license: bsd-3-clause
base_model:
- OpenGVLab/InternVL2_5-1B
- OpenGVLab/InternVL2_5-1B-MPO
tags:
- InternVL2_5
- InternVL2_5-1B
- InternVL2_5-1B-MPO
- Int8
- VLM
---

# InternVL2_5-1B-Int8

This version of InternVL2_5-1B has been converted to run on the Axera NPU using **w8a16** quantization.

This model has been optimized with the following LoRA: 

Compatible with Pulsar2 version: 3.3

## Convert tools links:

For those who are interested in model conversion, you can try to export axmodel through the original repo : 
https://huggingface.co/OpenGVLab/InternVL2_5-1B

[Pulsar2 Link, How to Convert LLM from Huggingface to axmodel](https://pulsar2-docs.readthedocs.io/en/latest/appendix/build_llm.html) 

[AXera NPU HOST LLM Runtime](https://github.com/AXERA-TECH/ax-llm/tree/internvl2) 

[AXera NPU AXCL LLM Runtime](https://github.com/AXERA-TECH/ax-llm/tree/axcl-llm-internvl)

## Support Platform

- AX650
  - AX650N DEMO Board
  - [M4N-Dock(爱芯派Pro)](https://wiki.sipeed.com/hardware/zh/maixIV/m4ndock/m4ndock.html)
  - [M.2 Accelerator card](https://axcl-docs.readthedocs.io/zh-cn/latest/doc_guide_hardware.html)
- AX630C
  - [爱芯派2](https://axera-pi-2-docs-cn.readthedocs.io/zh-cn/latest/index.html)
  - [Module-LLM](https://docs.m5stack.com/zh_CN/module/Module-LLM)
  - [LLM630 Compute Kit](https://docs.m5stack.com/zh_CN/core/LLM630%20Compute%20Kit)
 
|Chips|image encoder 448|ttft|w8a16|
|--|--|--|--|
|AX650| 350 ms | 420 ms |32 tokens/sec|

|Chips|image encoder 364|ttft|w8a16|
|--|--|--|--|
|AX630C| 1120 ms | 1150 ms |11 tokens/sec|


## How to use

Download all files from this repository to the device

**If you using AX650 Board**
```
root@ax650:/mnt/qtang/llm-test/temp/InternVL2_5-1B# tree -L 1
.
|-- config.json
|-- internvl2_5_1b_448_ax650
|-- internvl2_5_tokenizer
|-- internvl2_5_tokenizer_448.py
|-- main_internvl2_5_448_prefill
|-- run_internvl2_5_448_ax650.sh
`-- ssd_car.jpg
```

**If you using AX630C Board**
```
root@ax630c:/mnt/qtang/llm-test/internvl2_5-1b-mpo# tree -L 1
.
|-- config.json
|-- internvl2_5_1b_364_ax630c
|-- internvl2_5_tokenizer
|-- internvl2_5_tokenizer_364.py
|-- main
`-- run_internvl2_5_364_ax630c.sh
```

#### Install transformer

```
pip install transformers==4.41.1
```

#### Start the Tokenizer service

**If you using AX650 Board**
```
root@ax650:/mnt/qtang/llm-test/temp/InternVL2_5-1B# python3 internvl2_5_tokenizer_448.py --port 12345
None None 151645 <|im_end|>
[151644, 8948, 198, 56568, 104625, 100633, 104455, 104800, 101101, 32022, 102022, 99602, 100013, 9370, 90286, 21287,
42140, 53772, 35243, 26288, 104949, 3837, 105205, 109641, 67916, 30698, 11, 54851, 46944, 115404, 42192, 99441, 100623,
48692, 100168, 110498, 1773, 151645, 151644, 872, 198, 151665, 151667, 151667, 151667, 151667, 151667, 151667, 151667,
151667, 151667, 151667, 151667, 151667, 151667, 151667, 151667, 151667, 151667, 151667, 151667, 151667, 151667, 151667,
......
151667, 151667, 151667, 151667, 151667, 151667, 151667, 151667, 151667, 151667, 151667, 151667, 151667, 151667, 151667,

 198, 5501, 7512, 279, 2168, 19620, 13, 151645, 151644, 77091, 198]
310
[151644, 8948, 198, 56568, 104625, 100633, 104455, 104800, 101101, 32022, 102022, 99602, 100013, 9370, 90286, 21287,
42140, 53772, 35243, 26288, 104949, 3837, 105205, 109641, 67916, 30698, 11, 54851, 46944, 115404, 42192, 99441, 100623,
48692, 100168, 110498, 1773, 151645, 151644, 872, 198, 14990, 1879, 151645, 151644, 77091, 198]
47
http://localhost:12345
```

**If you using AX630C Board**
```
root@ax650:/mnt/qtang/llm-test/temp/InternVL2_5-1B# python internvl2_5_tokenizer_364.py --port 12345
Special tokens have been added in the vocabulary, make sure the associated word embeddings are fine-tuned or trained.
None None 151645 <|im_end|>
[151644, 8948, 198, 56568, 104625, 100633, 104455, 104800, 101101, 32022, 102022, 99602, 100013, 9370, 90286, 21287,
42140, 53772, 35243, 26288, 104949, 3837, 105205, 109641, 67916, 30698, 11, 54851, 46944, 115404, 42192, 99441, 100623,
48692, 100168, 110498, 1773, 151645, 151644, 872, 198, 151665, 151667, 151667, 151667, 151667, 151667, 151667, 151667,
......
151667, 151667, 151667, 151667, 151667, 151667, 151667, 151667, 151667, 151667, 151667, 151667, 151666, 198, 5501, 7512,
279, 2168, 19620, 13, 151645, 151644, 77091, 198]
223
[151644, 8948, 198, 56568, 104625, 100633, 104455, 104800, 101101, 32022, 102022, 99602, 100013, 9370, 90286, 21287, 42140,
53772, 35243, 26288, 104949, 3837, 105205, 109641, 67916, 30698, 11, 54851, 46944, 115404, 42192, 99441, 100623, 48692,
100168, 110498, 1773, 151645, 151644, 872, 198, 14990, 1879, 151645, 151644, 77091, 198]
47
http://localhost:12345
```

#### Inference with AX650 Host, such as M4N-Dock(爱芯派Pro) or AX650N DEMO Board

- input text

```
Describe the picture
```

- input image

![](./ssd_car.jpg)

Open another terminal and run `./run_internvl2_5_448_ax650.sh`

```
root@ax650:/mnt/qtang/llm-test/temp/InternVL2_5-1B# ./run_internvl2_5_448_ax650.sh
[I][                            Init][ 127]: LLM init start
bos_id: -1, eos_id: 151645
  3% | ██                                |   1 /  28 [0.01s<0.14s, 200.00 count/s] tokenizer init ok
[I][                            Init][  26]: LLaMaEmbedSelector use mmap
100% | ████████████████████████████████ |  28 /  28 [1.42s<1.42s, 19.66 count/s] init vpm axmodel ok,remain_cmm(2859 MB)
[I][                            Init][ 275]: max_token_len : 1023
[I][                            Init][ 280]: kv_cache_size : 128, kv_cache_num: 1023
[I][                            Init][ 288]: prefill_token_num : 320
[I][                            Init][ 290]: vpm_height : 448,vpm_width : 448
[I][                            Init][ 299]: LLM init ok
Type "q" to exit, Ctrl+c to stop current running
prompt >> Describe the picture
image >> ssd_car.jpg
[I][                          Encode][ 358]: image encode time : 362.987000 ms, size : 229376
[I][                             Run][ 569]: ttft: 426.75 ms

The image depicts a scene on a city street with a prominent red double-decker bus in the background.
The bus is adorned with an advertisement that reads, "THINGS GET MORE EXCITING WHEN YOU SAY YES."
The bus is traveling on a road with a white bicycle lane marked on it. The street is lined with buildings,
and there is a black car parked on the side of the road. A woman is standing in the foreground, smiling at the camera.
She is wearing a black jacket and a scarf. The overall atmosphere suggests a typical urban setting,
possibly in a city known for its iconic double-decker buses.

[N][                             Run][ 708]: hit eos,avg 31.90 token/s

prompt >> q
root@ax650:/mnt/qtang/llm-test/temp/InternVL2_5-1B# 
```

#### Inference with M.2 Accelerator card

[What is M.2 Accelerator card?](https://axcl-docs.readthedocs.io/zh-cn/latest/doc_guide_hardware.html), Show this DEMO based on Raspberry PI 5.

*TODO*

#### Inference with AX630C Host, such as 爱芯派2, Module-LLM, LLM630 Compute Kit and AX630C DEMO Board

- input text

```
Describe the picture
```

- input image

![](./panda.jpg)

Open another terminal and run `./run_internvl2_5_364_ax630c.sh`

```
/mnt/qtang/llm-test/internvl2_5-1b-mpo # ./run_internvl2_5_364_ax630c.sh
[I][                            Init][ 106]: LLM init start
bos_id: -1, eos_id: 151645
  3% | ██                                |   1 /  28 [0.01s<0.14s, 200.00 count/s] tokenizer init ok
[I][                            Init][  26]: LLaMaEmbedSelector use mmap
100% | ████████████████████████████████ |  28 /  28 [9.48s<9.48s, 2.95 count/s] init vpm axmodel ok,remain_cmm(905 MB)
[I][                            Init][ 254]: max_token_len : 1023
[I][                            Init][ 259]: kv_cache_size : 128, kv_cache_num: 1023
[I][                            Init][ 267]: prefill_token_num : 256
[I][                            Init][ 269]: vpm_height : 364,vpm_width : 364
[I][                            Init][ 278]: LLM init ok
Type "q" to exit, Ctrl+c to stop current running

prompt >> Please describe the image
image >> panda.jpg
[I][                          Encode][ 337]: image encode time : 1156.637939 ms, size : 151424
[I][                             Run][ 548]: ttft: 1120.15 ms

The image features a red panda in a natural setting, likely in a zoo or a forested area.
The red panda has distinctive reddish-brown fur with white markings around its eyes and ears.
It is leaning on a wooden structure, possibly a platform or a log, with a background of green foliage.
The red panda appears to be looking directly at the camera with a calm expression.

[N][                             Run][ 687]: hit eos,avg 10.94 token/s
```
