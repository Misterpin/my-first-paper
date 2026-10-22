# Аналитический обзор: модели источника в ЭЭГ-спеллере

Дата поиска: 21.10.2026. Рассмотрено **40 уникальных публикаций/препринтов**. Источники: записи Europe PMC с авторскими аннотациями (запрос `"speller" AND "language model"`) и API arXiv по идентификаторам работ из исходного обзора, плюс отчёт Qwen2.5. Библиографические поля сверены с этими записями. Это целевой обзор, не систематический PRISMA-анализ.

**Глубина чтения:** ниже приведён анализ доступных аннотаций, а не утверждение о полном прочтении всех текстов или воспроизведении их кода. Для базовой работы Speier et al. (2012) дополнительно проверены разделы Methods полного текста. Препринты помечены отдельно; опубликованные численные преимущества не переносятся в наш эксперимент. Старые работы сохранены как основания байесовского декодирования, HMM и ранней остановки; свежие работы 2024–2026 годов определяют актуальную границу задачи.

## Синтез и исследовательский вопрос

Спеллер — экранная клавиатура, в которой выбор буквы оценивается по ответу мозга на стимулы. Модель источника оценивает вероятность следующей буквы по уже введённому тексту. Это отличается от подсказок целых слов, исправления готового текста и восстановления прочитанного текста непосредственно из ЭЭГ.

Литература уже содержит сравнение нескольких LLM и настройки веса prior. Поэтому нельзя объявлять объединение языка и ЭЭГ или multi-model сравнение новым методом. Задача проекта — воспроизводимый русскоязычный анализ при неизменных свидетельствах: какие prior помогают, насколько ошибочный контекст снижает пользу и какой вычислительной ценой достигается улучшение.

**Базовые решения:** (1) равномерный prior; (2) частоты букв как абляция контекста; (3) буквенная n-грамма. Нейросетевая модель, словная KenLM и динамический интерфейс — следующие сравнения, а не выполненный результат пилота. Работа не должна скрывать отсутствие превосходства над базовым методом, если оно обнаружится.

**Сопоставимость:** онлайн P300 ближе к целевому применению; архивное декодирование позволяет фиксировать наблюдения; синтетический пилот проверяет код и гипотезы модели; SSVEP, ЭКоГ и ЭЭГ при чтении дают контекст, но не сопоставимые скорости.

## Карточки работ

### 01. Near-optimal P300 speller performance using large language models: A multi-model analysis with performance bounds (2026)

**Авторы:** Parthasarathy, Nithin, Soetedjo, James, Panchavati, Saarang, Parthasarathy, Nitya, Lee, Dongwoo, Arnold, Corey и др.. **Источник:** [PloS one](https://doi.org/10.1371/journal.pone.0349281). **BibTeX:** `p42821610`. **Проверено:** публикация; аннотация.

- **Подход / результат:** Сопоставляет несколько LLM и оракульную верхнюю границу на симуляциях по ЭЭГ 78 участников. Это ближайший аналог по вопросу выбора источника.
- **Ограничение для нашей задачи:** Предел зависит от используемого декодера и протокола; не является универсальным пределом нейроинтерфейса.
- **Как используем:** Отказаться от претензии на первое multi-model сравнение; изучать русский язык и вклад prior отдельно.

### 02. Postprocessing of P300 Speller Output with a Large Language Model (2026)

**Авторы:** Paplavsky, Nikita, Lebedev, Mikhail. **Источник:** [Препринт](https://doi.org/10.64898/2026.06.24.734268). **BibTeX:** `pPPR1259817`. **Проверено:** препринт; аннотация.

- **Подход / результат:** Сравнивает LLM и ByT5 на искусственных ошибках, в том числе пропусках и заменах символов.
- **Ограничение для нашей задачи:** Препринт; восстановление готового текста не измеряет качество следующего символьного prior.
- **Как используем:** Отключить медленную коррекцию в основном сравнении источников.

### 03. ChatBCI-Assist: An Intent-Based P300 Speller With A Locally Deployed LLM and Adaptive Stopping Strategy Enabling Record Online Spelling Performance (2026)

**Авторы:** Hong, Jiazhen, Rao, Pradyumna, Wang, Weinan, Najafizadeh, Laleh. **Источник:** [IEEE transactions on bio-medical engineering](https://doi.org/10.1109/tbme.2026.3693965). **BibTeX:** `p42139128`. **Проверено:** публикация; аннотация.

- **Подход / результат:** Локальная адаптированная LLM, подсказки фраз и ранняя остановка объединены в онлайн-системе.
- **Ограничение для нашей задачи:** Одновременно меняются модель, интерфейс и критерий общения; общий прирост нельзя приписать одному prior.
- **Как используем:** Фиксировать интерфейс; измерять задержку и вклад модели раздельно.

### 04. Limits of Trial-Adaptive Neural–Language Fusion Across Large Language Models in P300 Brain–Computer Interfaces (2026)

**Авторы:** Gorenshtein, Alon, Omar, Mahmud, Jia, Eric, Adiniaev, Yosef, Daniel, Oved, Kruskal, Jonathan и др.. **Источник:** [Препринт](https://doi.org/10.64898/2026.08.30.26361777). **BibTeX:** `pPPR1312732`. **Проверено:** препринт; аннотация.

- **Подход / результат:** Сопоставляет адаптивный и настроенный постоянный вес для 22 prior на архивных выборах 47 людей с БАС.
- **Ограничение для нашей задачи:** Препринт и один архив; отсутствие выигрыша адаптации не доказывает его невозможность во всех задачах.
- **Как используем:** Настраивать постоянный alpha на валидации с равным бюджетом для моделей.

### 05. Quantifying Large Language Model Influence in Brain Computer Interface Communication for Amyotrophic Lateral Sclerosis (2026)

**Авторы:** Gorenshtein, Alon, Omar, Mahmud, Jia, Eric, Adiniaev, Yosef, Daniel, Oved, Kruskal, Jonathan и др.. **Источник:** [Препринт](https://doi.org/10.64898/2026.08.20.26360941). **BibTeX:** `pPPR1305153`. **Проверено:** препринт; аннотация.

- **Подход / результат:** Анализирует вклад языка и нейронных свидетельств в итоговое распределение для 25 источников.
- **Ограничение для нашей задачи:** Препринт; результат зависит от определения вклада и калибровки.
- **Как используем:** Добавить контроль без информативного сигнала; не приравнивать точность к чтению намерения.

### 06. Intent Drift in LLM-Assisted BCI Communication: An In-Silico Benchmark Under Simulated Decoder Corruption (2026)

**Авторы:** Gorenshtein, Alon, Omar, Mahmud, Jia, Eric, Adiniaev, Yosef, Daniel, Oved, Kruskal, Jonathan и др.. **Источник:** [Препринт](https://doi.org/10.64898/2026.08.20.26360939). **BibTeX:** `pPPR1306894`. **Проверено:** препринт; аннотация.

- **Подход / результат:** Изучает изменение намеренного сообщения после LLM-коррекции при моделируемых ошибках.
- **Ограничение для нашей задачи:** Препринт, in-silico и автоматическая оценка смысла с ограниченным согласием экспертов.
- **Как используем:** В дальнейшем измерять испорченные сообщения; в пилоте не включать коррекцию.

### 07. Connecting a P300 speller to a large language model (2025)

**Авторы:** Lebedev, Mikhail, Makarova, Anna, Kleeva, Daria, Maysuradze, Archil. **Источник:** [Препринт](https://doi.org/10.1101/2025.11.06.686984). **BibTeX:** `pPPR1116152`. **Проверено:** препринт; аннотация.

- **Подход / результат:** Предлагает исправлять ошибки P300-вывода LLM с контекстом предложения; оценивает на симуляции по существующим данным.
- **Ограничение для нашей задачи:** Препринт; не доказывает онлайн-прирост и не сравнивает символьные prior.
- **Как используем:** Развести два эксперимента: prior до выбора и исправление после выбора.

### 08. Large language models integrated into brain-computer interfaces for communication and control: a systematic review (2026)

**Авторы:** Gorenshtein, Alon, Omar, Mahmud, Barash, Yiftach, Nadkarni, Girish N, Klang, Eyal. **Источник:** [Biomedical physics & engineering express](https://doi.org/10.1088/2057-1976/ae737b). **BibTeX:** `p42202831`. **Проверено:** публикация; аннотация.

- **Подход / результат:** Классифицирует 11 работ по пяти способам интеграции LLM и отмечает недостаточное описание задержек.
- **Ограничение для нашей задачи:** Набор работ и выводы ограничены критерием отбора; клинический эффект включённых решений не установлен.
- **Как используем:** Явно назвать наш способ интеграции и не смешивать разные метрики общения.

### 09. ChatBCI, a P300 speller BCI with context-driven word prediction leveraging large language models, from concept to evaluation (2026)

**Авторы:** Hong, Jiazhen, Wang, Weinan, Najafizadeh, Laleh. **Источник:** [Scientific reports](https://doi.org/10.1038/s41598-025-25660-7). **BibTeX:** `p41698950`. **Проверено:** публикация; аннотация.

- **Подход / результат:** ChatBCI использует удалённый GPT-3.5 и дополнительные клавиши слов; есть онлайн-проверка на семи участниках.
- **Ограничение для нашей задачи:** Сокращение числа нажатий не равно улучшению вероятностей следующей буквы; зависимость от API.
- **Как используем:** Рассматривать как альтернативу интерфейса, а не прямой числовой baseline.

### 10. Comparing P300 flashing paradigms in online typing with language models (2025)

**Авторы:** Chandravadia, Nand, Pendekanti, Shrita, Roberts, Dustin, Tran, Robert, Panchavati, Saarang, Arnold, Corey и др.. **Источник:** [PloS one](https://doi.org/10.1371/journal.pone.0303390). **BibTeX:** `p39899514`. **Проверено:** публикация; аннотация.

- **Подход / результат:** Сравнивает row-column, checkerboard и combinatorial при языковой модели и динамической остановке.
- **Ограничение для нашей задачи:** В данных этой работы различия парадигм не дали значимого эффекта; перенос на другую задачу требует проверки.
- **Как используем:** Зафиксировать row-column, чтобы не смешивать фактор модели и стимуляции.

### 11. Towards Predictive Communication: The Fusion of Large Language Models and Brain-Computer Interface (2025)

**Авторы:** Carìa, Andrea. **Источник:** [Sensors (Basel, Switzerland)](https://doi.org/10.3390/s25133987). **BibTeX:** `p40648241`. **Проверено:** публикация; аннотация.

- **Подход / результат:** Обсуждает объединение LLM и BCI, адаптивность, задержки и устойчивость к шуму.
- **Ограничение для нашей задачи:** Perspective, не отдельный сравнительный эксперимент.
- **Как используем:** Использовать для карты проблем, но не как подтверждение численного выигрыша.

### 12. Noninvasive decoding of typed sentences from human brain activity (2026)

**Авторы:** Lévy, Jarod, Zhang, Mingfang, Pinet, Svetlana, Rapin, Jérémy, Banville, Hubert, d'Ascoli, Stéphane и др.. **Источник:** [Nature neuroscience](https://doi.org/10.1038/s41593-026-02303-2). **BibTeX:** `p42374156`. **Проверено:** публикация; аннотация.

- **Подход / результат:** Brain2Qwerty декодирует предложения при реальном наборе на клавиатуре по ЭЭГ и МЭГ у 35 добровольцев.
- **Ограничение для нашей задачи:** Движения при наборе и другая модальность; не P300-выбор символа вниманием.
- **Как используем:** Объяснить читателю границы задачи спеллера и исключить прямое сравнение CER.

### 13. Language Model-Guided Classifier Adaptation for Brain-Computer Interfaces for Communication (2022)

**Авторы:** Chen, Xinlin J, Collins, Leslie M, Mainsah, Boyla O. **Источник:** [Conference proceedings. IEEE International Conference on Systems, Man, and Cybernetics](https://doi.org/10.1109/smc53654.2022.9945561). **BibTeX:** `p36776946`. **Проверено:** публикация; аннотация.

- **Подход / результат:** Языковая информация помогает отбирать псевдометки для адаптации классификатора между сессиями.
- **Ограничение для нашей задачи:** Симуляция; язык меняет сам декодер, а не только априорное распределение.
- **Как используем:** Заморозить генератор свидетельств и не обучать классификатор по прогнозам модели.

### 14. A Bayesian dynamic stopping method for evoked response brain-computer interfacing (2024)

**Авторы:** Ahmadi, Sara, Desain, Peter, Thielen, Jordy. **Источник:** [Frontiers in human neuroscience](https://doi.org/10.3389/fnhum.2024.1437965). **BibTeX:** `p39776784`. **Проверено:** публикация; аннотация.

- **Подход / результат:** Байесовский риск позволяет управлять ошибками и компромиссом скорости и точности.
- **Ограничение для нашей задачи:** Обоснование опирается на модель распределений; показатель качества зависит от стоимости ошибки.
- **Как используем:** Первый пилот вести при фиксированном числе повторений; раннюю остановку изучать отдельно.

### 15. Extending Brain-Computer Interface Access with a Multilingual Language Model in the P300 Speller (2022)

**Авторы:** Loizidou, P, Rios, E, Marttini, A, Keluo-Udeke, O, Soetedjo, J, Belay, J и др.. **Источник:** [Brain computer interfaces (Abingdon, England)](https://doi.org/10.1080/2326263x.2021.1993426). **BibTeX:** `p35574291`. **Проверено:** публикация; аннотация.

- **Подход / результат:** Переносит языковую модель P300 на испанский и греческий; онлайн-проверка с 30 носителями трёх языков.
- **Ограничение для нашей задачи:** Отсутствие значимого различия не доказывает равенство языков; русский не проверен.
- **Как используем:** Проводить собственный русскоязычный пилот, а не переносить чужие скорости.

### 16. Brain-Computer Interface Speller Based on Steady-State Visual Evoked Potential: A Review Focusing on the Stimulus Paradigm and Performance (2021)

**Авторы:** Li, Minglun, He, Dianning, Li, Chen, Qi, Shouliang. **Источник:** [Brain sciences](https://doi.org/10.3390/brainsci11040450). **BibTeX:** `p33916189`. **Проверено:** публикация; аннотация.

- **Подход / результат:** Обзор SSVEP-спеллеров объясняет связь частот стимулов, интерфейса и усталости.
- **Ограничение для нашей задачи:** SSVEP имеет другую физиологическую основу и временные масштабы.
- **Как используем:** Дать определение спеллера и уточнить, что эксперимент относится к P300-подобной модели.

### 17. Adaptive Sequence-Based Stimulus Selection in an ERP-Based Brain-Computer Interface by Thompson Sampling in a Multi-Armed Bandit Problem (2021)

**Авторы:** Ma, Tianwen, Huggins, Jane E, Kang, Jian. **Источник:** [Proceedings. IEEE International Conference on Bioinformatics and Biomedicine](https://doi.org/10.1109/bibm52615.2021.9669724). **BibTeX:** `p35692622`. **Проверено:** публикация; аннотация.

- **Подход / результат:** Thompson sampling выбирает последовательности стимулов в checkerboard-парадигме.
- **Ограничение для нашей задачи:** Выигрыш в симуляции зависит от сходства генератора с реальными данными.
- **Как используем:** Не переносить цифры ускорения; динамическую раскладку оставить отдельной гипотезой.

### 18. Language-Model Assisted And Icon-based Communication Through a Brain Computer Interface With Different Presentation Paradigms (2018)

**Авторы:** Ahani, Asieh, Moghadamfalahi, Mohammad, Erdogmus, Deniz. **Источник:** [IEEE transactions on neural systems and rehabilitation engineering : a publication of the IEEE Engineering in Medicine and Biology Society](https://doi.org/10.1109/tnsre.2018.2859432). **BibTeX:** `p30047890`. **Проверено:** публикация; аннотация.

- **Подход / результат:** IconMessenger объединяет ERP, sem-gram и несколько способов показа иконок; проверен на десяти участниках.
- **Ограничение для нашей задачи:** Иконки представляют смысловые единицы, а не одинаковые буквенные выборы.
- **Как используем:** Фиксировать единицу анализа: символ, а не слово или иконка.

### 19. Feedback Related Potentials for EEG-Based Typing Systems (2021)

**Авторы:** Gonzalez-Navarro, Paula, Celik, Basak, Moghadamfalahi, Mohammad, Akcakaya, Murat, Fried-Oken, Melanie, Erdoğmuş, Deniz. **Источник:** [Frontiers in human neuroscience](https://doi.org/10.3389/fnhum.2021.788258). **BibTeX:** `p35145386`. **Проверено:** публикация; аннотация.

- **Подход / результат:** Объединяет ERP, языковой контекст и потенциалы обратной связи в байесовском решении; 12 участников.
- **Ограничение для нашей задачи:** Дополнительный источник FRP меняет объём информации на один выбор.
- **Как используем:** Не добавлять feedback evidence только одной сравниваемой модели.

### 20. Generalizable spelling using a speech neuroprosthesis in an individual with severe limb and vocal paralysis (2022)

**Авторы:** Metzger, Sean L, Liu, Jessie R, Moses, David A, Dougherty, Maximilian E, Seaton, Margaret P, Littlejohn, Kaylo T и др.. **Источник:** [Nature communications](https://doi.org/10.1038/s41467-022-33611-3). **BibTeX:** `p36347863`. **Проверено:** публикация; аннотация.

- **Подход / результат:** Речевой нейропротез распознаёт попытки произнести кодовые слова для букв у одного участника.
- **Ограничение для нашей задачи:** Инвазивный сигнал и отдельный протокол; его скорость не является P300-baseline.
- **Как используем:** Использовать только для контекста задачи восстановления общения.

### 21. Improving P300 Spelling Rate using Language Models and Predictive Spelling (2018)

**Авторы:** Speier, William, Arnold, Corey, Chandravadia, Nand, Roberts, Dustin, Pendekanti, Shrita, Pouratian, Nader. **Источник:** [Brain computer interfaces (Abingdon, England)](https://doi.org/10.1080/2326263x.2017.1410418). **BibTeX:** `p30560145`. **Проверено:** публикация; аннотация.

- **Подход / результат:** Разделяет и объединяет языковую классификацию и выбор целых слов; 12 здоровых участников.
- **Ограничение для нашей задачи:** Общий выигрыш объединяет несколько механизмов.
- **Как используем:** В основном эксперименте отключить клавиши слов, чтобы изолировать prior.

### 22. Integrating language models into classifiers for BCI communication: a review (2016)

**Авторы:** Speier, W, Arnold, C, Pouratian, N. **Источник:** [Journal of neural engineering](https://doi.org/10.1088/1741-2560/13/3/031002). **BibTeX:** `p27153565`. **Проверено:** публикация; аннотация.

- **Подход / результат:** Систематизирует использование языка: классификация, остановка, коррекция, обучение и предсказание слов.
- **Ограничение для нашей задачи:** Обзор 2016 года не покрывает современные LLM.
- **Как используем:** Взять структуру понятий, дополнив публикациями 2024–2026 годов.

### 23. A comparison of stimulus types in online classification of the P300 speller using language models (2017)

**Авторы:** Speier, William, Deshpande, Aniket, Cui, Lucy, Chandravadia, Nand, Roberts, Dustin, Pouratian, Nader. **Источник:** [PloS one](https://doi.org/10.1371/journal.pone.0175382). **BibTeX:** `p28406932`. **Проверено:** публикация; аннотация.

- **Подход / результат:** Сравнивает знакомые лица и инверсию символов вместе с языковым particle filter, в том числе онлайн.
- **Ограничение для нашей задачи:** Эффекты зрительного стимула и модели нельзя приписывать одному источнику.
- **Как используем:** Фиксировать тип стимуляции во всех условиях сравнения.

### 24. Online BCI Typing using Language Model Classifiers by ALS Patients in their Homes (2017)

**Авторы:** Speier, William, Chandravadia, Nand, Roberts, Dustin, Pendekanti, S, Pouratian, Nader. **Источник:** [Brain computer interfaces (Abingdon, England)](https://doi.org/10.1080/2326263x.2016.1252143). **BibTeX:** `p29051907`. **Проверено:** публикация; аннотация.

- **Подход / результат:** Онлайн-использование P300 с языковым классификатором проверено у шести людей с БАС дома.
- **Ограничение для нашей задачи:** Малая клиническая выборка; это не доказательство качества нашего русскоязычного prior.
- **Как используем:** Планировать последующую проверку в целевой популяции, отдельно от синтетического пилота.

### 25. Brain-Computer Interface Spellers: A Review (2018)

**Авторы:** Rezeika, Aya, Benda, Mihaly, Stawicki, Piotr, Gembler, Felix, Saboor, Abdul, Volosyak, Ivan. **Источник:** [Brain sciences](https://doi.org/10.3390/brainsci8040057). **BibTeX:** `p29601538`. **Проверено:** публикация; аннотация.

- **Подход / результат:** Обзор классифицирует спеллеры по P300, SSVEP и motor imagery, связывая физиологию с GUI.
- **Ограничение для нашей задачи:** Объективное сравнение систем затрудняют разные условия и метрики.
- **Как используем:** Использовать для понятного определения спеллера и ограничить наши выводы одним протоколом.

### 26. Increasing BCI communication rates with dynamic stopping towards more practical use: an ALS study (2015)

**Авторы:** Mainsah, B O, Collins, L M, Colwell, K A, Sellers, E W, Ryan, D B, Caves, K и др.. **Источник:** [Journal of neural engineering](https://doi.org/10.1088/1741-2560/12/1/016013). **BibTeX:** `p25588137`. **Проверено:** публикация; аннотация.

- **Подход / результат:** Проверяет динамическую остановку и языковую информацию в онлайн-эксперименте с БАС.
- **Ограничение для нашей задачи:** Скорость зависит от индивидуального сигнала и алгоритма остановки.
- **Как используем:** Фиксированное число повторений позволяет сначала отделить качество prior от политики остановки.

### 27. Word-level language modeling for P300 spellers based on discriminative graphical models (2015)

**Авторы:** Saa, Jaime F Delgado, Pesters, Adriana de, McFarland, Dennis, Çetin, Müjdat. **Источник:** [Journal of neural engineering](https://doi.org/10.1088/1741-2560/12/2/026007). **BibTeX:** `p25686293`. **Проверено:** публикация; аннотация.

- **Подход / результат:** Графическая модель слов использует ограниченный словарь и может пересматривать предыдущие буквы.
- **Ограничение для нашей задачи:** Ограниченный словарь и пересмотр прошлого отличаются от свободного посимвольного ввода.
- **Как используем:** Включать редкие слова и измерять ошибки без обратного исправления истории.

### 28. Incorporating advanced language models into the P300 speller using particle filtering (2015)

**Авторы:** Speier, W, Arnold, C W, Deshpande, A, Knall, J, Pouratian, N. **Источник:** [Journal of neural engineering](https://doi.org/10.1088/1741-2560/12/4/046018). **BibTeX:** `p26061188`. **Проверено:** публикация; аннотация.

- **Подход / результат:** Частицы позволяют использовать вероятностный автомат вместо простых n-грамм; есть offline и online оценка.
- **Ограничение для нашей задачи:** Приближённый вывод и число частиц добавляют вычислительную стоимость.
- **Как используем:** Сравнивать не только точность, но и затраты на предсказание.

### 29. Utilizing a language model to improve online dynamic data collection in P300 spellers (2014)

**Авторы:** Mainsah, Boyla O, Colwell, Kenneth A, Collins, Leslie M, Throckmorton, Chandra S. **Источник:** [IEEE transactions on neural systems and rehabilitation engineering : a publication of the IEEE Engineering in Medicine and Biology Society](https://doi.org/10.1109/tnsre.2014.2321290). **BibTeX:** `p24808413`. **Проверено:** публикация; аннотация.

- **Подход / результат:** Добавляет prior к байесовскому динамическому накоплению; онлайн-проверка на 17 участниках.
- **Ограничение для нашей задачи:** Выигрыш относится к совместному выбору и остановке, а не только качеству источника.
- **Как используем:** Служит основанием формулы объединения; раннюю остановку не имитировать скоростью CPU.

### 30. Integrating dynamic stopping, transfer learning and language models in an adaptive zero-training ERP speller (2014)

**Авторы:** Kindermans, Pieter-Jan, Tangermann, Michael, Müller, Klaus-Robert, Schrauwen, Benjamin. **Источник:** [Journal of neural engineering](https://doi.org/10.1088/1741-2560/11/3/035005). **BibTeX:** `p24834896`. **Проверено:** публикация; аннотация.

- **Подход / результат:** Объединяет перенос, адаптацию, язык и остановку в вероятностном ERP-спеллере на 22 участниках в симуляции.
- **Ограничение для нашей задачи:** Вклад переноса и адаптации велик; нельзя приписать весь результат языковой модели.
- **Как используем:** Менять один фактор при неизменной модели наблюдений.

### 31. Natural language processing with dynamic classification improves P300 speller accuracy and bit rate (2012)

**Авторы:** Speier, William, Arnold, Corey, Lu, Jessica, Taira, Ricky K, Pouratian, Nader. **Источник:** [Journal of neural engineering](https://doi.org/10.1088/1741-2560/9/1/016004). **BibTeX:** `p22156110`. **Проверено:** публикация; аннотация.

- **Подход / результат:** SWLDA, наивный Байес и триграмма показывают пользу контекста на небольшом P300-пилоте.
- **Ограничение для нашей задачи:** Шесть здоровых участников; оценка по девяти пятибуквенным словам не покрывает свободный ввод.
- **Как используем:** Базовая формула log prior + log likelihood; новые фразы и накопление собственных ошибок.

### 32. Integrating language information with a hidden Markov model to improve communication rate in the P300 speller (2014)

**Авторы:** Speier, William, Arnold, Corey, Lu, Jessica, Deshpande, Aniket, Pouratian, Nader. **Источник:** [IEEE transactions on neural systems and rehabilitation engineering : a publication of the IEEE Engineering in Medicine and Biology Society](https://doi.org/10.1109/tnsre.2014.2300091). **BibTeX:** `p24760927`. **Проверено:** публикация; аннотация.

- **Подход / результат:** HMM и Viterbi объединяют язык с сигналом и позволяют исправлять прежние решения.
- **Ограничение для нашей задачи:** Вывод по последовательности отличается от жадного посимвольного выбора.
- **Как используем:** Зафиксировать алгоритм решения; не смешивать эффект поиска с качеством prior.

### 33. PolyMorph: Increasing P300 Spelling Efficiency by Selection Matrix Polymorphism and Sentence-Based Predictions (2015)

**Авторы:** Alberto Casagrande, Joanna Jarmolowska, Marcello Turconi, Francesco Fabris, Pierpaolo Busan, Piero Paolo Battaglini. **Источник:** [arXiv](https://arxiv.org/abs/1502.04485). **BibTeX:** `a150204485`. **Проверено:** версия arXiv; аннотация.

- **Подход / результат:** PolyMorph меняет состав матрицы и предсказывает слова из контекста предложения.
- **Ограничение для нашей задачи:** Изменение числа кандидатов одновременно меняет процесс наблюдений.
- **Как используем:** Отделить динамическую стратегию от текущего эксперимента с фиксированным алфавитом.

### 34. MindChat: Enhancing BCI Spelling with Large Language Models in Realistic Scenarios (2025)

**Авторы:** JIaheng Wang, Yucun Zhong, Chengjie Huang, Lin Yao. **Источник:** [arXiv](https://arxiv.org/abs/2507.21435). **BibTeX:** `a250721435`. **Проверено:** версия arXiv; аннотация.

- **Подход / результат:** MindChat использует GPT-4o для контекстных предложений слов и фраз в онлайн-SSVEP.
- **Ограничение для нашей задачи:** Количество сохранённых выборов не измеряет качество P300-символьного prior.
- **Как используем:** Развести выбор готовой фразы и посимвольное декодирование.

### 35. DeWave: Discrete EEG Waves Encoding for Brain Dynamics to Text Translation (2023)

**Авторы:** Yiqun Duan, Jinzhao Zhou, Zhen Wang, Yu-Kai Wang, Chin-Teng Lin. **Источник:** [arXiv](https://arxiv.org/abs/2309.14030). **BibTeX:** `a230914030`. **Проверено:** версия arXiv; аннотация.

- **Подход / результат:** DeWave соединяет дискретные коды ЭЭГ с языковым представлением для перевода в текст.
- **Ограничение для нашей задачи:** Другие входные данные и критерии; зависимость от сегментации отличается от P300.
- **Как используем:** Обозначить соседнее направление, не смешивать выбор символов с восстановлением прочитанного текста.

### 36. Open Vocabulary Electroencephalography-To-Text Decoding and Zero-shot Sentiment Classification (2021)

**Авторы:** Zhenhailong Wang, Heng Ji. **Источник:** [arXiv](https://arxiv.org/abs/2112.02690). **BibTeX:** `a211202690`. **Проверено:** версия arXiv; аннотация.

- **Подход / результат:** Переводит ЭЭГ при чтении в открытый словарь с предобученной BART.
- **Ограничение для нашей задачи:** Задача чтения отличается от выбора буквы; позднейший аудит оценки требует осторожности с результатами.
- **Как используем:** Не использовать BLEU этой работы как доказательство пригодности спеллера.

### 37. High Performance P300 Spellers Using GPT2 Word Prediction With Cross-Subject Training (2024)

**Авторы:** Nithin Parthasarathy, James Soetedjo, Saarang Panchavati, Nitya Parthasarathy, Corey Arnold, Nader Pouratian и др.. **Источник:** [arXiv](https://arxiv.org/abs/2405.13329). **BibTeX:** `a240513329`. **Проверено:** версия arXiv; аннотация.

- **Подход / результат:** Сочетает межсубъектное обучение, GPT-2 и оптимизацию выбора, включая редкие слова.
- **Ограничение для нашей задачи:** Оценка преимущественно моделирует ввод по ЭЭГ; несколько факторов меняются вместе.
- **Как используем:** Ближайшая нейросетевая линия сравнения; не брать её скорость как baseline нашего пилота.

### 38. Qwen2.5 Technical Report (2024)

**Авторы:**  Qwen,  :, An Yang, Baosong Yang, Beichen Zhang, Binyuan Hui и др.. **Источник:** [arXiv](https://arxiv.org/abs/2412.15115). **BibTeX:** `a241215115`. **Проверено:** версия arXiv; аннотация.

- **Подход / результат:** Qwen2.5 описывает семейство базовых и instruction-tuned моделей разных размеров, включая малые.
- **Ограничение для нашей задачи:** Общие NLP-бенчмарки не доказывают хорошее распределение русских букв и приемлемую задержку.
- **Как используем:** Кандидат следующего этапа: фиксировать ревизию, размер, квантизацию и способ маргинализации.

### 39. Are EEG-to-Text Models Working? (2024)

**Авторы:** Hyejeong Jo, Yiqian Yang, Juhyeok Han, Yiqun Duan, Hui Xiong, Won Hee Lee. **Источник:** [arXiv](https://arxiv.org/abs/2405.06459). **BibTeX:** `a240506459`. **Проверено:** версия arXiv; аннотация.

- **Подход / результат:** Проверяет teacher forcing и сравнение с шумовым входом; показывает риск завышенной оценки ЭЭГ→текст.
- **Ограничение для нашей задачи:** Аудит относится к конкретному семейству моделей и не опровергает все BCI.
- **Как используем:** Отделить teacher-forced оценку prior от авторегрессионного декодирования и добавить d=0.

### 40. Enhancing EEG-to-Text Decoding through Transferable Representations from Pre-trained Contrastive EEG-Text Masked Autoencoder (2024)

**Авторы:** Jiaqi Wang, Zhenxi Song, Zhengyu Ma, Xipeng Qiu, Min Zhang, Zhiguo Zhang. **Источник:** [arXiv](https://arxiv.org/abs/2402.17433). **BibTeX:** `a240217433`. **Проверено:** версия arXiv; аннотация.

- **Подход / результат:** CET-MAE использует внутри- и межмодальное обучение ЭЭГ и текста, далее BART.
- **Ограничение для нашей задачи:** Требуются парные данные чтения; не проверяет одинаковые likelihood с разными prior.
- **Как используем:** Представить как альтернативный способ объединения, вне основного эксперимента.

## 41. Heafield (2011): KenLM: Faster and Smaller Language Model Queries

Источник: https://aclanthology.org/W11-2123/ . Первичная публикация, WMT 2011, страницы 187–197.

**Метод:** компактные структуры данных и эффективное выполнение запросов к n-граммной языковой модели. **Полезное свойство:** быстрые оценки вероятностей слов, нужные для символьного prior через сумму вероятностей продолжений префикса. **Граница применения:** работа посвящена вычислению языковых вероятностей; показатели ЭЭГ-декодирования в ней не измеряются. **Роль в проекте:** объясняет выбор KenLM как вычислительного backend; качество конкретной русской модели оценивается в наших экспериментах отдельно.
