# Fake Job Postings

Ссылка на соревнование: [RuCode Fake Job Postings](https://www.kaggle.com/c/rucode-fake-job-postings/overview/evaluation)

Объявления о работе описываются набором из 15 признаков, требуется предсказать является объявление настоящим или нет.

Для решения задачи я использовала предобученную модель **RoBERTa** из библиотеки **Hugging Face** 🤗.

Для обучения модели использовались параметры:

LR = 5e-5 \
Batch size = 4 \
Max len = 512 (максимальная длина входной последовательности, состоящей из комбинации строковых признаков) \
Max epoch = 10

Обучение проходило на TPU в [colab.research.google.com](https://colab.research.google.com/)

Краткая история повышения Score на LB:

| Model | Combined features | Train data size (0/1) | Public F1 | 
|:-------:|:-------:|:-------:|:-------:|
| `roberta-large`      | Описание вакансии | 9238/466 | 0.77 |
| `roberta-large`      | <p>**Название**,<br>Описание вакансии,<br>**Описание компании**<p> | 9238/466 | 0.83 |
| `roberta-large`      | <p>Название,<br>Описание вакансии,<br>Описание компании<p> | 9238/**8854** | 0.86 |
| `roberta-large`      | <p>Название,<br>**Тип занятости**,<br>**Дистанционно**,<br>**Вопросы**,<br>Описание компании,<br>Описание вакансии,<br>**Требования**<p> | 9238/8854 | 0.90 |

Положительный эффект оказало устранение дисбаланса между классами (добавление нескольких копий с фейками, никакие посторонние данные не использовались), а также подбор подходящей комбинации признаков.

На Public Leaderboard лучше всего себя показали три модели:

| Version | Model | Combined features | Main idea | Epoch | Public F1 | Private F1 | Notebook | Weights |
|:-------:|:-------:|:-------:|:-------:|:-------:|:-------:|:-------:|:-------:|:-------:|
| v17 | `roberta-large`      | <p>Название,<br>Тип занятости,<br>Дистанционно,<br>Вопросы,<br>**Зарплата**,<br>Место,<br>Описание компании,<br>Описание вакансии,<br>Требования<p> | <p>Попробовать<br>добавить<br>что-нибудь к v14<p> | <p>7<br>8<br>9<p> | <p>0.90494<br>0.90566<br>**0.91320**<p> | <p>0.87979<br>0.87657<br>0.87500<p> | [v17_RoBERTa_10e](v17%20RoBERTa/v17_RoBERTa_10e.ipynb) | [Weights v17](https://drive.google.com/drive/folders/1UztnEbz6mDDcVBnZ5EDe73db_yMKFfVW?usp=sharing) |
| v14 | `roberta-large`      | <p>Название,<br>Тип занятости,<br>Дистанционно,<br>**Место**,<br>Вопросы,<br>Описание компании,<br>Описание вакансии,<br>Требования<p> | <p>Взять все часто<br>заполненные<br>признаки<br>(>8000 non-null)<p> | <p>5<br>6<p> | <p>0.90421<br>0.90769<p> | <p>0.87878<br>0.87022<p> | [v14_RoBERTa_10e](v14%20RoBERTa/v14_RoBERTa_10e.ipynb) | [Weights v14](https://drive.google.com/drive/folders/1TLMEpCLtzU-VnR60pl_3NVHiXTq9CyM8?usp=sharing) |
| v25 | `roberta-large`      | <p>Название,<br>Тип занятости,<br>Дистанционно,<br>Вопросы,<br>Описание компании,<br>Описание вакансии,<br>Требования<p> | <p>Попробовать<br>выкинуть<br>что-нибудь из v14<p> | <p>7<br>8<br>9<p> | <p>0.90000<br>0.90000<br>0.90076<p> | <p>0.88549<br>**0.88832**<br>0.88383<p> | [v25_RoBERTa_10e](v25%20RoBERTa/v25_RoBERTa_10e.ipynb) | [Weights v25](https://drive.google.com/drive/folders/1aZ7bSNvoDfDHYj5hnzhcFUfINKzrPLP9?usp=sharing) |
  
Лично у меня больше доверия вызывает v25 (f1 на тесте ближе к валидации).

Действительно, на Private LB лучший результат показала **v25 (8 epoch)**!
