# Human-parity-on-machine-translations

Traditional Machine Learning techniques for tackling translation have seen big State of the art improvements in the last few years. However, they still struggle with languages that are far apart on the language family tree. For example, English and Chinese/Korean/Japanese.

Because of the nature of why these models struggle with these tasks (inability to extrapolate context, wildly mismatched grammar etc), I wondered how a pretrained large language model (LLM) at sufficient scale trained on multilingual corpora would perform. Could a bilingual LLM approximate a bilingual human on translation tasks?

The first step of course was choosing a model for testing. There are very few bilingual or multilingual models that are both trained at sufficient scale and have equal or near equal training data representation for the two languages in question. I thank the team at THUDM for training and releasing GLM-130B, a bilingual LLM trained on 200 billion tokens each of English and Chinese (400b total). (https://github.com/THUDM/GLM-130B). 

This is the main model used for testing. 
Demo available here - https://huggingface.co/spaces/THUDM/GLM-130B
Because GLM-130B isn't Instruction-finetuned, a few-shot or one-shot prompting strategy for translations is required. 
In preliminary tests, I notice some correlation in the complexity and quality of translations with the complexity and quality of few shot examples. As a result, my one-shot prompt includes a short passage and a corresponding translation from a Chinese book translated and published in English. 

# My One-shot prompt for GLM-130B
```bash
Chinese: 同北京许许多多同龄的老市民一样，薛大娘现在绝不是一个真正迷信的人，她知道迷信归根结蒂都是瞎掰，遇上听人讲述哪里有个老太太信神信鬼闹出乱子，她还会真诚地拍著大腿笑著说几句嘲讽的话；但她又同许许多多同龄的老市民一样，内心还揣著个求吉利的想法。

English: Like many Beijingers her age, she isn’t really superstitious—when you come right down to it, it’s just a bunch of random nonsense. Stories of old ladies fussing about visits from gods or ghosts have her slapping her thigh and making some cutting remark. Yet, also like many Beijingers her age, she has her own ideas about summoning good luck.

Chinese: Chinese text to translate

English: [gMASK]
```
Parameters are default except for 
- BeamSearchStrategy instead of BaseStrategy
- Seed 32615

Open AI's GPT models are multilingual with an extreme English bias (~92.6% English by Word Count) (https://github.com/openai/gpt-3/blob/master/dataset_statistics/languages_by_word_count.csv). However, since competence in one language seems to bleed into competence in other languages in an LLM of sufficient scale (On the Multilingual Capabilities of Very Large-Scale English Language Models-https://arxiv.org/abs/2108.13349 ), I also include chatGPT translations in the comparisons. As chatGPT is Instruction-aligned, a simple Translate command is sufficient and used. Specific instructions or examples to prioritize fluency and fluidity may yield better results.

No Language Left Behind, NLLB-200 from Meta achieved State of the Art Results on machine translation benchmarks and is also compared. 

For my tests, I pick Literature, an especially difficult domain for machine translation. 20 passages translated with GLM-130B and compared with Deepl, Google Translate, chatGPT and NLLB-200-1.3b-distilled. The passages are sampled from 5 novels. The Wedding Party by Liu Xinwu, Strange Beasts of China by Yan Ge, The Amber Sword by Fei YanFu, Wolf Totem by Jiang Rong and SuperGene.
The passages are chosen at random. They are not cherrypicked or regenerated. I present 5 passage comparisons here in the README and the rest in the document uploaded.
