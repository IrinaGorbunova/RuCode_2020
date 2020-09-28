# Fake News classification

Ссылка на соревнование: [Fake News classification](https://competitions.codalab.org/competitions/26284#learn_the_details-overview)

Имеются тексты новостей, требуется определить является ли новость настоящей или нет.

Для решения данной задачи я использовала предобученные модели трансформеров из библиотеки **Hugging Face** 🤗 \
**XLM-RoBERTa** и **BERT**.

Так как тексты на русском, то целесообразно было использовать модели, обученные на русском языке или на нескольких языках, включая русский.

Результаты по каждой рассмотренной модели представлены в таблице:

| Model | Parameters | Language | Best mean F1 (val) | Best mean F1 (test) | 
|:-------|:-------:|:-------:|:-------:|:----------:|
| `xlm-roberta-large`      | 355M | 100 languages | 1.000 | **1.000** |
| `xlm-roberta-base`      | 125M | 100 languages | 0.998 | - |
| `bert-base-multilingual-cased`      | 110M | 104 languages | 0.985 | - |
| `DeepPavlov/rubert-base-cased`      | 110M | russian | 0.989 | - |
| `DrMatters/rubert_cased`      | 110M | russian | 0.989 | - |

Для обучения каждой модели использовались одни и те же параметры:

LR = 5e-5 \
Batch size = 8 \
Max len = 256 (максимальная длина входной последовательности)

Обучение продолжалось 10 эпох, после чего выбирался лучший результат.

Эксперименты показали, что **XLM-RoBERTa** справляется с данной задачей лучше, чем **BERT**:

| Model | Language | Best mean F1 (val) | 
|:-------|:-------:|:-------:|
| `xlm-roberta-base`      | 100 languages | 0.998 |
| `bert-base-multilingual-cased`      | 104 languages | 0.985 |

А русскоязычные модели лучше, чем многоязычные:

| Model | Language | Best mean F1 (val) | 
|:-------|:-------:|:-------:|
| `bert-base-multilingual-cased`      | 104 languages | 0.985 |
| `DeepPavlov/rubert-base-cased`      | russian | 0.989 |
| `DrMatters/rubert_cased`      | russian | 0.989 |

\* *Сравнение проводилось между одинаковыми базовыми конфигурациями моделей.*

Лучший же результат (F1 = 1.0) показала модель **XLM-RoBERTa-large**. \
Воспроизводимый (на 28.09.2020) ноутбук с комментариями можно найти в папке [xlm-roberta-large-final-version](xlm-roberta-large-final-version) \
Сохраненные веса модели (после 5 эпох) можно скачать по ссылке: [XLM_R_5.pt](https://drive.google.com/file/d/1Q0Q9VTNVHwuE3me8ey9k6FVvDCqhECfX/view?usp=sharing) \
Обратите внимание, что ноутбук запускался на [colab.research.google.com](https://colab.research.google.com) с **TPU**
