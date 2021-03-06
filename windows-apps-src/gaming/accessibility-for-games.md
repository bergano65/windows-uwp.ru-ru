---
title: Создание игр со специальными возможностями
description: Узнайте, как сделать игры доступными всем пользователям без исключения. Принцип инклюзивного дизайна поможет вам реализовать в своей игре необходимые специальные возможности.
ms.assetid: f5ba1e60-0d7c-11e6-91ec-0002a5d5c51b
ms.date: 11/09/2017
ms.topic: article
keywords: windows 10, uwp, специальные возможности, игры
ms.localizationpriority: medium
ms.openlocfilehash: 347f5c6900806ef4658b81b8db15957029d39116
ms.sourcegitcommit: 6af7ce0e3c27f8e52922118deea1b7aad0ae026e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/20/2020
ms.locfileid: "77478650"
---
#  <a name="making-games-accessible"></a>Создание игр со специальными возможностями

Благодаря специальным возможностям каждый человек и каждая организация на планете могут достигать большего, и к специальным возможностям в играх это относится в неменьшей степени. Эта статья предназначена для разработчиков, дизайнеров и продюсеров игр. В ней собраны рекомендации из различных источников (перечисленных в разделе "Использованные материалы" ниже) по реализации специальных возможностей в играх и рассмотрено использование принципа инклюзивного (его также называют универсальным) дизайна для создания игр, доступных максимальному количеству пользователей.

## <a name="gaming-for-everyone"></a>Gaming for Everyone

Корпорация Майкрософт полагает, что игры должны быть доступны всем. Мы "считали, что должны делать больше, чтобы игровой мир стал инклюзивной средой, радость от которой мог бы получать каждый. Мы твердо уверены, что все, что мы создаем для наших поклонников и то, как мы это преподносим — внутри Майкрософт и за пределами компании — говорит о том, кто мы есть. Мы разработали программу для отражения наших базовых ценностей как организации и верим, что эта программа способна стать катализатором позитивных изменений не только внутри нашего коллектива, но и в продуктах, которые мы разрабатываем для своей базы игроков". ([Запись из блога](https://blogs.microsoft.com/blog/2016/06/13/gaming-for-everyone/) Фила Спенсера (Phil Spencer))

Мы хотим создавать увлекательную, многообразную и инклюзивную среду, играть в которой смогут все. "Для перемен, которые будут заметны в долгосрочной перспективе, нужен культурный сдвиг, который не произойдет за неделю-две. Тем не менее наша команда каждый день стремится становиться лучше. Мы учим друг друга останавливаться в процессе принятия решений и задумываться обо всем разнообразии потребностей, возможностей и интересов любителей игр по всему миру". ([Запись из блога](https://blogs.microsoft.com/blog/2016/06/13/gaming-for-everyone/) Фила Спенсера (Phil Spencer))

Мы надеемся, что вы присоединитесь к инициативе [Gaming for Everyone](https://news.microsoft.com/gamingforeveryone/) и поможете нам сделать игры доступными для всех. 

##  <a name="why-make-games-accessible"></a>Для чего нужны специальные возможности в играх?

### <a name="increased-gamer-base"></a>Увеличение базы игроков

На самом базовом уровне реализация специальных возможностей в играх имеет очевидное экономическое обоснование:

количество пользователей, которые могут играть в вашу игру X увлекательность игры = продажи игры

Если вы создали замечательную игру, однако она настолько сложна или замысловата, что играть в нее может только ограниченный круг людей, вы автоматически уменьшили свои продажи. Аналогично, создавая игру, в которую не могут играть люди с физическими, сенсорными или когнитивными нарушениями, вы уменьшаете количество ее потенциальных покупателей. Принимая во внимание, что, например, [19% населения США имеют ту или иную форму инвалидности](https://www.census.gov/newsroom/releases/archives/miscellaneous/cb12-134.html), [примерно 14% взрослых в США испытывают затруднения с чтением](https://nces.ed.gov/naal/estimates/overview.aspx) и [примерно у 10% лиц мужского пола наблюдается то или иное нарушение цветного зрения](https://www.aao.org/eye-health/diseases/color-blindness-risk), это может существенно сказаться на выручке от вашей игры. 

Другие бизнес-обоснования можно найти в статье [Making Video Games Accessible](https://docs.microsoft.com/windows/desktop/DxTechArts/accessibility-best-practices).

### <a name="better-games"></a>Более качественные игры

Реализация в игре специальных возможностей может в конечном итоге повысить качество игры. 

Примером тому могут быть субтитры в играх. В прошлом игры редко поддерживали субтитры или скрытые субтитры с диалогами персонажей. Сегодня же субтитры или скрытые субтитры в играх стали нормой. Эта перемена произошла не из-за игроков с ограниченными возможностями, а в результате локализации: выяснилось, что многие игроки предпочитали играть с субтитрами независимо от языка — просто потому, что так им было удобнее. Пользователи включают субтитры и скрытые субтитры, когда играют в шумной обстановке, плохо различают речь персонажей из-за звуковых эффектов или звукового окружения либо просто тогда, когда им хотелось бы уменьшить громкость, чтобы не беспокоить окружающих. Субтитры и скрытые субтитры не только сделали игры комфортнее, но и дали возможность играть в них людям с нарушениями слуха.

Переназначение контроллеров — еще одна возможность, которая постепенно становится стандартом игровой индустрии по этим же причинам. Обычно оно предлагается как преимущество для всех игроков. Некоторые игроки любят настраивать игровую среду под себя, а другим просто нравится отходить от первоначального замысла разработчиков. Большинство людей не осознает, что возможность переназначить кнопки на устройстве ввода — это на самом деле специальная возможность, которая задумывалась, чтобы играть в игру могли люди с различными нарушениями двигательных функций, которые физически не могут пользоваться определенными частями контроллера или которым тяжело это делать.

В конечном итоге усилия, вложенные в реализацию в игре специальных возможностей, во многих случаях повышают ее качество, потому что так она становится удобнее для всех ваших игроков и предоставляет им больше способов персонализации.

### <a name="social-space-and-quality-of-life"></a>Социальное пространство и качество жизни

Видеоигры — одно из самых доходных направлений развлекательной индустрии, способное дарить людям многие часы удовольствий. Но для тех, кто прикован к больничной койке, страдает от хронических болей или серьезного социального тревожного расстройства, гейминг — это не только развлечение, но и отдушина. Игроки переносятся в мир, где становятся главными персонажами в видеоигре. Играя, они могут создавать для себя социальное пространство, которое позволяет им отвлечься от ежедневных трудностей, обусловленных их физическим или психическим расстройством, и дает возможность общаться с людьми, с которыми они не могут взаимодействовать иным образом. 

Игры — это еще и культура. Способность принимать участие в том, о чем говорят все и вся, может иметь огромную ценность с точки зрения качества жизни человека.

##  <a name="is-the-game-you-are-making-today-accessible"></a>Есть ли в ваших сегодняшних играх специальные возможности?

Если вы впервые планируете реализовать в своей игре специальные возможности, вот некоторые вопросы, которые вам нужно себе задать:

* Можно ли играть в игру одной рукой? 
* Может ли среднестатистический человек поднять игровое устройство и играть с его помощью?
* Можно ли эффективно играть в игру на маленьком мониторе или на телевизоре, сидя на расстоянии от него?
* Поддерживает ли игра более одного типа устройства ввода, которые можно было бы использовать на всем ее протяжении?
* Можно ли играть в игру с выключенным звуком?
* Можно ли играть в игру, переведя монитор в черно-белый режим?
* Если загрузить последний сохраненный сеанс игры через месяц, легко ли понять, где в игре вы находитесь и что нужно сделать, чтобы двигаться дальше?

Если на большинство вопросов вы ответили "нет" или "не знаю", пора задуматься о специальных возможностях в вашей игре.

## <a name="defining-disability"></a>Что такое "ограниченные возможности"?

Ограниченные возможности можно определить как "несоответствие между потребностями человека и предлагаемыми услугой, продуктом или средой" ([видеоролик об инклюзивности](https://www.microsoft.com/design/inclusive/) на Microsoft.com). Это значит, что ограничение возможностей может возникнуть у каждого, и может представлять собой краткосрочное или ситуативное состояние. Представьте себе, с какими затруднениями могут столкнуться игроки в таком состоянии при попытке сыграть в вашу игру, и подумайте, как улучшить дизайн игры для их удобства. Вот некоторые ограничения возможностей, которые стоит принять во внимание.

### <a name="vision"></a>Зрение

*   Долгосрочные медицинские состояния, такие как глаукома, катаракта, дальтонизм, близорукость и диабетическая ретинопатия.
*   Краткосрочные ситуативные состояния, такие как небольшой размер монитора или экрана, низкое разрешение или блики на мониторе или экране мобильного устройства из-за солнечного света.
        
### <a name="hearing"></a>Слух

* Долгосрочные медицинские состояния, такие как полная или частичная утрата слуха вследствие заболеваний или генетических нарушений.
* Краткосрочные ситуативные состояния, такие как чрезмерный фоновый шум, низкое качество аудио или необходимость ограничивать громкость звука, чтобы не беспокоить окружающих.
        
### <a name="motor"></a>Двигательные способности

* Долгосрочные медицинские состояния, такие как болезнь Паркинсона, боковой амиотрофический склероз, артрит и мышечная дистрофия.
* Краткосрочные ситуативные состояния, такие как травма руки, чашка в руке или ребенок на руках.
  
### <a name="cognitive"></a>Когнитивные способности

* Долгосрочные медицинские состояния, такие как дислексия, эпилепсия, синдром дефицита внимания с гиперактивностью, слабоумие и амнезия.
* Краткосрочные ситуативные состояния, такие как алкогольное опьянение, недостаток сна, или временные отвлекающие факторы, например сирена проезжающей под окнами машины скорой помощи.

### <a name="speech"></a>Голосовые функции

* Долгосрочные медицинские состояния, такие как повреждение голосовых связок, дизартрия и апраксия.
* Краткосрочные ситуативные состояния, такие как стоматологические приспособления во рту, прием пищи или напитков.


## <a name="how-to-make-games-more-accessible"></a>Как сделать игры доступнее для пользователей с ограниченными возможностями?

### <a name="design-shift-inclusive-game-design-approach"></a>Инклюзивный дизайн: подойдите к проектированию игры по-новому

Инклюзивный (универсальный) дизайн предполагает создание продуктов и услуг с повышенной доступностью для более широкого спектра потребителей, включая людей с ограниченными возможностями.

Чтобы добиться успеха, сегодня недостаточно создавать игры, которые кажутся увлекательными их авторам. Разработчикам игр нужно понимать, как их решения в плане дизайна влияют на доступность игры в целом, т. е. способность всех членов потенциальной аудитории — включая тех, чьи возможности ограничены — играть в нее.

Поэтому традиционные парадигмы дизайна игр необходимо пересмотреть с учетом концепции инклюзивного дизайна. Инклюзивный дизайн означает выход за пределы базового назначения игры — развлечения целевой аудитории — и создание дополнительных или видоизмененных персонажей для включения в нее более широкого спектра людей. С особым вниманием необходимо подходить к проектированию препятствий в игре и следить за тем, чтобы они не создавали ненужных затруднений, которые сделают игровой процесс менее приятным.

Выявив недостатки, вы сможете переработать и оптимизировать первоначальную концепцию игры, сделать ее совершеннее, чтобы она могла стать источником удовольствия для более широкого круга людей. Уделите в процессе проектирования игры время тому, чтобы сделать ее инклюзивной, и готовый продукт вашего труда будет доступен большему числу пользователей. Не бывает игр, которые подходили бы всем; игры по определению должны предполагать некоторые трудности, однако включение специальных возможностей поможет гарантировать, что никто не будет исключен из вашей целевой аудитории без объективных на то причин.

### <a name="empower-gamers-give-gamers-options"></a>Комфорт игроков: дайте игрокам возможность выбора

Реализация практически всех специальных возможностей сводится к одному из двух принципов. Первый — это дать игрокам возможность индивидуально настраивать свое взаимодействие с игрой. Если у вас уже огромная база поклонников, возможно, значительная часть аудитории не захочет никаких изменений. Это нормально. Дайте своим игрокам возможность включать и отключать эти функции, а также настраивать их по отдельности. Пусть люди взаимодействуют с игрой так, как им больше нравится исходя из потребностей и предпочтений.

### <a name="reinforce-communicate-information-in-more-than-one-way"></a>Дублирование: доносите информацию до игрока несколькими способами

Второй принцип основан на концепции универсального проектирования. Это единый подход, который позволит не только привлечь больше игроков, но и сделает игру комфортнее для всех. Например, информация может доноситься до пользователя и в виде изображения, и в виде текста или и в виде символа, и в виде цвета. Схема, основанная на диапазоне различных цветных маркеров, не только может быть использована для любителей, она также неприятно для всех остальных пользователей, которым необходимо запомнить все, что эквивалентно. Добавление символов позволит упростить взаимодействие с игрой для всех пользователей.

### <a name="innovate-be-creative"></a>Новаторство: дайте волю творчеству

Существует множество оригинальных способов улучшить вашу игру с точки зрения специальных возможностей. Подходите к вопросу творчески и не стесняйтесь заимствовать идеи из других игр, создатели которых уже позаботились об их доступности. Если у вас есть существующая игра, поработайте над выявлением в ней функций, которые можно было бы усовершенствовать, сохранив базовую механику и сюжет игры неизменными. Как было сказано выше, специальные возможности в играх — это прежде всего возможность настраивать принципы взаимодействия с игрой на свой вкус. Это может быть дублирование информации или донесение ее до игрока несколькими способами. 

Учет специальных возможностей позволяет по-новому подойти к дизайну и, возможно, реализовать идеи, которые иначе не пришли бы вам в голову. Результатом такого подхода бывает не только появление интересных концепций, но и создание продуктов, получающих широкое распространение и успех на массовом рынке. Среди примеров — предиктивный ввод текста, распознавание речи, скошенные бордюры, динамики, печатная машинка и оптическое распознавание текста (OCR). Идеи, которые легли в основу этих продуктов, появились в результате поиска решений для людей с ограниченными возможностями.

### <a name="adopt-quality-means-accessible-features"></a>Адаптация: качество означает доступность для всех

Специальные возможности — это одна из мер качества. Они должны быть обязательной функцией, а не просто приятным дополнением. Например, адаптацию мини-карты для дальтоников нельзя считать низкоприоритетной задачей, которой вы займетесь, если у вас будет лишнее время. Если эта задача не решена, это значит, что компонент "мини-карта" не готов, и его нельзя включать в продукт.

### <a name="evangelize-make-accessibility-a-priority-in-your-game-studio"></a>Смещение акцентов: сделайте специальные возможности приоритетом для своих разработчиков

Разработка игр всегда происходит в жесткие сроки, поэтому приоритизация специальных возможностей помогает облегчить процесс. Один из способов — это с самого начала проектировать игру с учетом специальных возможностей. Чем раньше вы задумаетесь о специальных возможностях, тем проще и дешевле будет их реализовать. 

Делитесь знаниями о специальных возможностях со своей командой, подводите под них бизнес-основания и развеивайте распространенные заблуждения — что специальные возможности нужны очень немногим людям, что они нарушают вашу игровую механику и что реализовывать их сложно и дорого.

### <a name="review-constantly-evaluate-your-game"></a>Контроль: постоянно оценивайте свою игру

В ходе разработки имеет смысл предусмотреть процесс проверки, чтобы убедиться, что ни на одном из этапов вы не забыли специальных возможностей. Составьте контрольный список вроде приведенного ниже, чтобы помочь своей команде понять, получается ли разрабатываемый продукт доступным или нет.

| Контрольный список                                         | Специальные возможности                                                                                                         |
|---------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------|
| Кинематика игры                                | Имеет субтитры и скрытые субтитры; игра протестирована на предмет того, не будет ли она вызывать приступов у людей с фотосенситивной эпилепсией                                                                           |
| Графика (двухмерная и трехмерная)              | Цвета и параметры, не зависящие от цветового скрытия, полностью зависят от цвета для идентификации, но также используют фигуры и шаблоны.|
| Начальный экран, меню параметров и другие меню       | Возможность произнесения названий и значений параметров вслух, возможность запоминания настроек, альтернативный способ ввода команд, регулируемый размер шрифта пользовательского интерфейса  |
| Игровой процесс                                          | Обширный выбор уровней сложности, субтитры и скрытые субтитры, хорошая визуальная и звуковая обратная связь                           |
| HUD-дисплей                                       | Регулируемое расположение экрана, изменяемый размер шрифта, удобный режим цвета с ослабленным зрением                                                  |        
| Ввод управляющего воздействия                                     | Возможность сопоставления видов управляющего воздействия различным устройствам ввода, поддержка нестандартных контроллеров, возможность упрощенного ввода                               |        

### <a name="playtest-and-iterate-get-gamers-feedback"></a>Тестирование и итерация: узнайте мнение игроков

Пригласите на сеансы тестирования плейтестеров с ограниченными возможностями, для которых предназначена ваша игра, и попросите их поиграть. Не забудьте уделить внимание специальным возможностям в своем опроснике для бета-тестеров. Местные организации людей с ограниченными возможностями — прекрасный источник кандидатов для участия в тестировании. Понаблюдайте за их игрой и узнайте их мнение. Выясните, какие изменения необходимо внести, чтобы сделать игру лучше.

Используйте социальные сети и форум по вашей игре, чтобы узнать, какие специальные возможности наиболее востребованы и как их лучше реализовать. 

### <a name="shout-it-out-let-the-world-know-your-game-is-accessible"></a>Информационная поддержка: сообщите потенциальным игрокам о специальных возможностях в вашей игре

Пользователи должны знать, что в вашу игру могут играть люди с ограниченными возможностями. Четко укажите это на веб-сайте, в пресс-релизах и на упаковке игры, чтобы люди, покупающие вашу игру, знали, на что могут рассчитывать. Не забудьте также сделать веб-сайт и все каналы продаж доступными для людей с ограниченными возможностями. И самое главное — обратитесь к сообществу игроков с ограниченными возможностями и расскажите им о своей игре.

## <a name="game-accessibility-features"></a>Специальные возможности в играх

В этом разделе рассматриваются некоторые функции, которые сделают ваши игры доступными людям с ограниченными возможностями. Эти функции являются производными от рекомендаций, полученных на веб-сайте с [рекомендациями по специальным возможностям для игр](http://gameaccessibilityguidelines.com/) . Этот ресурс представляет результаты группы совместной работы Studios, специалистов и учебных заведений.

### <a name="color-blind-friendly-graphics-and-user-interface"></a>Удобная графическая система и пользовательский интерфейс с цветовым зрением

Сетчатка глаза содержит два типа светочувствительных клеток: колбочки, обеспечивающие дневное зрение, и палочки, обеспечивающие сумеречное зрение. 

Колбочки бывают трех видов (красные, зеленые и синие), что позволяет нам правильно различать цвета. Цветовая заглушка возникает, когда один или несколько из этих трех типов Light конес не работают должным образом. Степень освещенности цветов может варьироваться от почти обычного восприятия цвета с меньшим уровнем чувствительности к красному, зеленому или синему свету, что позволяет полностью неспособно воспринимать любой цвет. 

Так как менее распространена меньшая чувствительность к синему свету, при проектировании для скрытия цвета выделенные цвета применяются к людям, которые имеют красный или зеленый цвет.
 
  + Используйте сочетания цветов, которые могут быть различны людям с ослаблением красного и зеленого цвета:
  
    * цвета, которые кажутся похожими: все оттенки красного и зеленого, включая коричневый и оранжевый;
    * цвета, которые выделяются на общем фоне: синий и желтый.
    
  + Нельзя использовать для дифференциации игровых объектов исключительно цвет. Используйте для этого также форму и рисунок.
  + Если приходится использовать только цвет, задайте цвета по умолчанию, но также предусмотрите возможность свободно выбирать другие цвета. Так люди, которым это нужно, смогут адаптировать цвета под свои потребности, а другие смогут не тратить на это время.
  + Используйте симулятор цвета для тестирования ваших макетов, чтобы можно было просматривать их с помощью глаз цвета. Это поможет вам избежать распространенных проблем с контрастностью. [Цвет Oracle](https://www.colororacle.org) — это бесплатный симулятор цвета, который может имитировать три наиболее распространенных типа недостатков цветовых концепций — деутеранопиа, протанопиа и тританопиа.
  
### <a name="closed-captioning-and-subtitles"></a>Субтитры и скрытые субтитры

Задача разработки субтитров и скрытых субтитров — предоставить игрокам удобочитаемые субтитры, чтобы в вашу игру можно быть играть при выключенном звуке. У пользователя должна быть возможность видеть компоненты игры — диалоги, звуковое окружение и звуковые эффекты — в виде текста на экране.

Вот некоторые основные правила, которые необходимо учитывать при создании субтитров и скрытых субтитров:

*   Выбирайте простой, удобочитаемый шрифт.
*   Выбирайте достаточно большой размер шрифта или предусмотрите возможность его регулировки. (Идеальный размер шрифта зависит от размера экрана, расстояния от игрока до экрана и т. п.)
*   Обеспечьте высокий контраст между цветом фона и цветом шрифта. Используйте четкий контур и заметные тени для текста. Используйте для субтитров наложение с темным фоном и не забудьте предусмотреть параметры, позволяющие включать и выключать их. (Дополнительные сведения о коэффициентах контрастности можно найти [здесь](https://docs.microsoft.com/windows/uwp/accessibility/accessible-text-requirements).)
* Показывайте на экране короткие предложения — не более 38 символов на строку и не более 2–3 строк одновременно. (Следите за тем, чтобы не раскрывать сюжет преждевременно, т. е. не выводить текст до того, как произойдет соответствующее событие.)
*   Указывайте, что является источником звука или кто из персонажей говорит. (Например, "Дэниел: Привет!")
*   Предусмотрите параметр для включения и выключения субтитров и скрытых субтитров. (Дополнительная функция: возможность выбора объема отображаемой информации о звуках в зависимости от ее важности.)

### <a name="game-chat-transcription"></a>Транскрибирование игрового чата

Если ваша игра позволяет игрокам общаться в голосовом чате и отправлять друг другу текстовые сообщения, необходимо предусмотреть функциональность для преобразования текста в речь и речи в текст.

Так люди, которые играют на устройстве без микрофона, все равно смогут общаться с теми, у кого есть возможность говорить. Они смогут вводить текст в окно чата, и эти сообщения будут преобразовываться в речь. Это также даст возможность людям, которые не очень хорошо слышат, читать транскрибированные текстовые сообщения от человека, с которым они разговаривают в чате.

Для разработчиков, участвующих в программе ID@Xbox и программе управляемых партнеров, функции преобразования текста в речь и речи в текст доступны в составе [специальных возможностей Game Chat 2](https://docs.microsoft.com/gaming/xbox-live/multiplayer/chat/using-game-chat-2.md#accessibility) в службе Xbox Live. Дополнительные сведения см. в разделе [Общие сведения о Game Chat 2](https://docs.microsoft.com/gaming/xbox-live/multiplayer/chat/game-chat-2-overview.md).

### <a name="sound-feedback"></a>Звуковая обратная связь

Звук в игре обеспечивает обратную связь для игрока, в дополнение к визуальной обратной связи. Хороший звуковой дизайн игры может сделать ее более доступной для игроков с нарушениями зрения. Вот некоторые моменты, которые стоит принять во внимание.

*   Используйте трехмерные аудиоподсказки для предоставления дополнительной пространственной информации.
* Предусмотрите отдельные регуляторы громкости для музыки, речи и звуковых эффектов.
*   Пишите голосовые сообщения так, чтобы игроки могли получать из них значимую информацию. (Пример: "Враги приближаются" или "Враги заходят через черный ход".)
*   Убедитесь, что речь проговаривается в разумном темпе; желательно также предусмотреть элемент управления для регулировки темпа.

### <a name="fully-mappable-controls"></a>Полностью переназначаемые элементы управления

Существуют компании и организации, такие как [Special Effect](https://www.specialeffect.org.uk/), которые занимаются разработкой нестандартных игровых контроллеров для различных игровых систем, таких как Windows и Xbox One. Такие нестандартные контроллеры позволяют людям с различными физическими нарушениями играть в игры, в которые они иначе играть не смогли бы. Дополнительные сведения о людях, которые благодаря нестандартным контроллерам теперь могут играть самостоятельно, можно найти [здесь](https://www.specialeffect.org.uk/who-we-helped).

Разрабатывая игру, вы можете увеличить ее доступность, сделав элементы управления полностью переназначаемыми, чтобы у игроков была возможность подключать свои нестандартные контроллеры и сопоставлять кнопки в соответствии со своими потребностями.

Возможность полностью переназначать элементы управления полезна и людям, которые пользуются стандартными контроллерами. Ваши игроки могут разработать для себя схему, которая отвечает их индивидуальным потребностям.

Стандартные контроллеры Xbox One и Xbox Elite поддерживают возможность настройки контроллеров, позволяя играть с максимальной точностью. Чтобы можно было полностью использовать возможности переназначения, __рекомендуется, чтобы разработчики включали переназначение непосредственно в игру__. Дополнительные сведения см. в описаниях [Xbox One](https://support.xbox.com/xbox-one/accessories/customize-standard-controller-with-accessories-app) и [Xbox Elite](https://support.xbox.com/xbox-one/accessories/use-accessories-app-configure-elite-controller).

### <a name="wider-selection-of-difficulty-levels"></a>Более широкий выбор уровней сложности

Видеоигра — это в первую очередь развлечение. Соответственно, перед разработчиками стоит задача настроить уровень сложности игры так, чтобы она представляла для игрока достаточную сложность, ни более, ни менее. Во-первых, не у всех игроков одинаковый уровень навыков и способностей, поэтому более широкий выбор вариантов сложности повышает шансы на то, что каждый из игроков найдет для себя подходящий вариант. В то же время такой более широкий выбор позволяет сделать вашу игру доступнее, потому что так играть в нее потенциально сможет больше людей с ограниченными возможностями. Помните, что игроки хотят преодолевать в игре трудности и получать за это поощрение. Им не нужна игра, в которой они не могут выиграть.

Оптимизация уровней сложности игры — очень тонкий процесс. Если игра слишком легкая, игроку она наскучит. Если она слишком трудная, игрок может сдаться и больше к ней не возвращаться. Нахождение равновесия — это и искусство, и наука. Существует несколько способов создать уровень игры с достаточной степень сложности. В некоторых играх предусмотрены варианты упрощенного ввода, например возможность играть нажатием одной кнопки, функции перемотки и повтора, делающие игру более "снисходительной" к игроку, а также возможность уменьшить количество врагов и сделать их слабее, чтобы после нескольких попыток игроку было проще продвигаться вперед.

### <a name="photosensitivity-epilepsy-testing"></a>Тестирование на предмет провоцирования приступов фотосенситивной эпилепсии

Фотосенситивная эпилепсия — это заболевание, приступы которого вызываются световой стимуляцией, например вспышками света или определенными движущимися формами и рисунками. Фотосенситивной эпилепсией страдают примерно три процента людей; в большей степени она распространена среди детей и подростков. В количественном выражении это примерно [1 из 4000 человек в возрасте от 5 до 24 лет](https://www.epilepsy.com/learn/professionals/about-epilepsy-seizures/reflex-seizures-and-related-epileptic-syndromes-0).

Существует множество факторов, способных вызвать подобную реакцию при игре в видеоигры, включая длительность игры, частоту вспышек, интенсивность света, контраст фона и света, расстояние между экраном и игроком, а также длину волны света.

Многие люди обнаруживают, что у них эпилепсия, только после припадка. Первый припадок нередко возникает именно при игре в видеоигры и может привести к физической травме. Вот некоторые советы по разработке игр, которые помогут снизить риск припадков, вызванных фотосенситивной эпилепсией:

Избегайте использования:
* Мигающего света с частотой от 5 до 30 вспышек в секунду (герц), потому что вспышки света с частотой в этом диапазоне с наибольшей вероятностью вызывают припадки.
* Любых последовательностей мерцающих изображений, длительность которых составляет более 5 секунд.
* Более трех вспышек в течение секунды, занимающих четверть и более экрана.
* Движущихся повторяющихся рисунков или однородного текста, занимающих четверть и более экрана.
* Статичных повторяющихся рисунков или однородного текста, занимающих четверть и более экрана.
* Мгновенных резких изменений яркости/контраста (включая быстрые переходы) или изменений цвета с красного или на красный.
* Пяти или более повторяющихся через равномерные промежутки высококонтрастных полос — строк или столбцов, таких как в таблице или шахматном рисунке, которые могут состоять из более мелких повторяющихся элементов, например кружков.
* Более пяти строк текста, состоящего исключительно из прописных букв с небольшим межбуквенным интервалом и междустрочным интервалом той же высоты, что и буквы (из-за чего текст превращается в равномерное чередование высококонтрастных полос).

Используйте автоматизированную систему для проверки игры на предмет стимулов, которые могут спровоцировать приступ фотосенситивной эпилепсии. (Например, [тест Хардинга](https://www.hardingtest.com/index.php?page=test) и [анализатор вспышек и паттернов Хардинга (FPA) G2](https://www.hardingfpa.com/harding-fpa-for-games/), разработанные компанией Cambridge Research System Ltd и профессором Грэмом Хардингом.) 

Добавьте в настройки параметр **Мерцание вкл./выкл.** и установите параметр **Мерцание** по умолчанию в значение **Выкл.** . Так вы защитите тех своих игроков, которые еще не знают о своей склонности к припадкам.

Предусматривайте перерывы между уровнями, чтобы у игроков был повод отдохнуть от непрерывной игры.

## <a name="other-accessibility-resources"></a>Другие ресурсы по специальным возможностям

Вот несколько внешних сайтов, на которых можно найти дополнительную информацию о специальных возможностях в играх.

### <a name="game-accessibility-guidelines"></a>Game accessibility guidelines
* [Рекомендации по специальным возможностям игр](http://gameaccessibilityguidelines.com/) (используются в качестве ссылок в этом разделе)
* [Рекомендации по Аблегамерс Foundation](https://accessible.games/accessible-player-experiences/) (используется в качестве ссылки в этом разделе)
* [Разработка игр с универсальным доступом (UA)](https://www.ics.forth.gr/hci/ua-games/index_main.php?l=e&c=555)

### <a name="custom-input-controllers"></a>Нестандартные контроллеры ввода
* [Специальное действие](https://www.specialeffect.org.uk/)
* [Фигхтер на War-занятии](https://www.warfighterengaged.org/)

### <a name="other-references-used"></a>Другие используемые ссылки
* [Некоторая осведомленность о цвете, заинтересованная компания сообщества](https://www.colourblindawareness.org/colour-blindness/types-of-colour-blindness/)
* [Как правильно разместить субтитры&mdash;статье блога Гамасутра by Ян Хэмилтон](https://www.gamasutra.com/blogs/IanHamilton/20150715/248571/How_to_do_subtitles_well__basics_and_good_practices.php)
* [Инновации для всех программ](https://www.inclusivedesign.no/practical-tools/definitions-article56-127.html)
* [Епилепси Foundation](https://www.epilepsy.com/)

### <a name="related-links"></a>Дополнительные ссылки
* [Включающая конструкция](https://www.microsoft.com/design/inclusive/)
* [Центр разработчиков специальных возможностей Майкрософт](https://developer.microsoft.com/windows/accessible-apps)
* [Разработка доступных приложений UWP](https://docs.microsoft.com/windows/uwp/accessibility/accessibility)
* [Электронная книга по проектированию специальных возможностей](https://www.microsoft.com/download/details.aspx?id=19262)