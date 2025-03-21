# Определение

## Введение

В мире разработки программного обеспечения ресурсы часто ограничены, так как никакой бизнес не желает бесконечно вливать деньги в разработку, на выходе не получая ничего. При этом конкуренция очень сильно давит на бизнес, вынуждая его подгонять команду разработки. Стартапы зачастую имеют еще меньше ресурсов, и вынуждены конкурировать с “мастодонтами” рынка, выдавливая их из своей ниши или же формируя новую.

В веб-сервисах или веб-приложениях, фронтенд играет ключевую роль в создании продукта привлекающего и удерживающего пользователей, ведь они чаще всего приходят решить свою задачу через UI. При запуске любого продукта, можно потратить сотни и тысячи часов на реализацию инфраструктуры, качественной архитектуры, десятки слоев защиты и прочего, но при этом провалиться при выходе на рынок и потерять продукт.

Когда команда разработки фокусируется на единственной цели, многие архитектурные решения можно отложить на потом, когда будет понятно, с какой ЦА работает продукт, каким образом пользователи с ним взаимодействуют и где обнаруживается высокая нагрузка.

**Цель урока**: показать значимость фокуса на главной функциональности при запуске продукта, предоставить практические рекомендации по ее определению, продемонстрировать практики по реализации MVP.

## Почему важно определять главную функциональность

В современном мире часто возникают ситуации, когда команды разработки тратят много времени на разработку функциональности, которая не приносит ожидаемой пользы. Это приводит к перерасходу бюджета, задержкам в выпусках и неудовлетворенности пользователей. Фокус на главной функциональности помогает избежать этих проблем и создать продукт, который действительно решает проблемы целевой аудитории.

### Типичные проблемы

- **Распыление ресурсов**: команда тратит время и силы на второстепенные функции, не создавая ценности для пользователей. Программисты очень любят создавать качественные программные системы, оставляя интерфейсы на откуп дизайнерам, но не стоит забывать, что именно фронтенд-разработчики создают качественные интерфейсы, дизайнеры лишь дают им образ и систему.
- **Перегруженность продукта**: избыток функций усложняет интерфейс и затрудняет пользователям взаимодействие. К тому же, большое количество функций потребует массу времени на проработку со стороны каждой команды(бизнес, дизайнеры, бекенд, фронтенд, тестировщики, маркетинг). Чем больше функций, тем сложнее сделать их качественно.
- **Затягивание разработки**: попытка реализовать все функции сразу приводит к задержкам в выпуске продукта, на это влияет отсутствие автоматического тестирования, ошибки в проектировании и размер команды.
- **Перерасход ресурсов**: вытекает из предыдущего пункта, но мало кто учитывает, что бизнес без продукта на рынке постоянно теряет деньги, которые можно было потратить, например, на поиск людей.
- **Сложности в маркетинге**: без четкого фокуса трудно сформулировать уникальное торговое предложение, пользователи могут не понимать, что именно им предлагает продукт. А маркетологам может быть проблематично продавать плохо проработанные функции.
- **Трудности в получении обратной связи**: пользователям сложнее оценить продукт с множеством разрозненных функций. К тому же, без четкого понимания о чем продукт, сложно будет определить метрики успешности: как понять, функция не пользуется популярностью, потому что она не нужна или потому что её никто не может найти в дебрях интерфейса?
- **Сложность поддержки**: без фокуса, есть шансы сильно усложнить кодовую базу, что радикально повысит стоимость исправления багов и добавление новых функций. Сталкивались с переписыванием проекта с нуля, после 4 лет в продакшене? А после 2?

### Преимущества раннего определения фокуса

- **Быстрый выход на рынок**: концентрация на главном позволяет быстрее запустить продукт, пусть и с ограниченным набором фич, но уже работающий на реальных пользователях.
- **Прозрачное позиционирование**: четко сформулированную ценность продукта, легче донести пользователям и инвесторам.
- **Оценка рыночного потенциала**: качественно определенный фокус, поможет быстрее понять, решает ли продукт реальную проблему пользователей и что можно с этим делать.
- **Улучшение пользовательского опыта**: простой и понятный продукт с фокусом на главной функции легче в освоении, а техническим писателям будет проще создавать обучающие материалы.
- **Улучшение качества**: больше времени и внимания уделяется ключевым аспектам продукта, выстраивая фундамент для дальнейших релизов.
- **Гибкость в развитии**: проще итерировать и улучшать продукт на основе обратной связи, которую будет проще собирать, если пользователи понимают, что дает им продукт.
- **Эффективное использование ресурсов**: когда команда сосредотачивается на самом важном, это позволяет экономить время и бюджет.
- **Снижение технического долга**: чем меньше в проекте неиспользуемых функций, тем меньше кода поддерживать и обновлять.
- **Облегчение процесса принятия решений**: прозрачные приоритеты помогают в выборе следующих шагов разработки, особенно это касается архитектурных решений команды разработки.
- **Повышение мотивации команды**: видимый прогресс и концентрация на важном поддерживают энтузиазм разработчиков, когда пользователи регулярно получают обновления.

### Как понимание ключевой фичи влияет на поддержку проекта

- **Формирование сильного фундамента**: команда разработки может потратить чуть больше времени на качественную реализацию главной функции продукта, что создаст более надежную базу для дальнейшего развития. Это отлично работает, когда вся команда погружена в контекст и понимает для кого они делают продукт.
- **Создание лояльной аудитории**: пользователи, довольные ключевой функцией, более склонны оставаться с продуктом и рекомендовать его.
- **Качество важнее количества**: Для некоторых продуктов, лучше добавлять одну фичу за раз, но хорошо проработанную, чем десятки, но поверхностных. В этой образовательной программе, мы рассматриваем именно такие продукты.
- **Упрощение масштабирования**: четкое понимание core-функционала облегчает процесс роста продукта и команды.
- Повышение инвестиционной привлекательности: продукт с ясной ценностью более привлекателен для инвесторов.

### Сложности или как крайности мешают бизнесу

- При излишне долгом планировании без перехода к реализации, рынок может измениться, и продукт станет неактуальным либо потребует значительных переработок.
- Долгосрочное детальное планирование может серьезно навредить производительности, особенно в стартапах, так как потребует очень много времени, а также изменчивость рынка потребует очень часто менять планы.
- Такое может случаться, если команда раньше не имела опыта создания подобных продуктов. В этом случае, стоит использовать детальное планирование только для следующего шага, а долгосрочное планирование выполнять более крупными кусками.

### Связь с бизнес-стратегией

- Фокус на ключевой функциональности помогает держать в поле зрения только важные бизнес-задачи, не затрачивая время на то, что можно сделать позже, а освободившиеся часы, можно потратить на качественное улучшение главной функциональности.
- Если потребуется резко изменить направление развития продукта, это будет проще сделать с минимальным количеством кода и функций. Большое количество функций потребуют заново продумать их бизнес-правила, что может потребовать колоссальных усилий для доработки.
- Концентрация на ключевых функциях продукта, часто приводит к улучшению основных бизнес показателей. Например, пользователи быстрее конвертируются в платящих, если они четко понимают пользу продукта, это также относится к маркетингу, которого касались выше.

Вот несколько примеров как фокус на ключевой функции продукта улучшает бизнес показатели:

- **Коэффициент конверсии** (Conversion Rate):
  - Понятный продукт с фокусом на главной функции легче привлекает и удерживает пользователей, за счет единственного пути пользователя, от привлечения до подписки.
  - Dropbox начинал с простой синхронизации файлов, что привело к высокой конверсии trial-пользователей в платящих.
- **Коэффициент удержания пользователей** (Retention Rate):
  - Качественная реализация ключевой функции повышает удовлетворенность пользователей и их лояльность, за счет множества факторов, ключевым здесь является надежность и интуитивный дизайн.
  - Во время развития, Spotify сосредоточился на персонализированных плейлистах, что помогло значительно повысить удержание пользователей.
- **Пожизненная ценность клиента** (Customer Lifetime Value, CLV):
  - Довольные основной функцией пользователи склонны дольше оставаться с продуктом, вкладывать больше и приглашать друзей.
  - В свое время, Amazon фокусировался на удобстве покупок книг, что привело к росту CLV за счет повторных покупок.
- **Время до окупаемости** (Time to Break Even):
  - Быстрый выход на рынок с единственной главной функцией ускоряет получение выручки.
  - Airbnb довольно быстро запустил первую версию бронирования жилья, что позволило им быстрее достичь точки безубыточности.
- **Рентабельность инвестиций** (Return on Investment, ROI):
  - Эффективное использование ресурсов при фокусе на ключевых функциях повышает отдачу от инвестиций.
  - LinkedIn сосредоточился на профессиональных связях, что привело к высокому ROI для B2B-сегмента.
- Скорость итераций и выпуска новых версий:
  - Меньший объем кода и четкие приоритеты позволяют быстрее выпускать обновления.
  - [Linear.app](http://Linear.app), [Morgen.so](http://Morgen.so), [Raycast.app](http://Raycast.app), ArcBrowser, выпускают обновления своих приложений довольно часто, при этом, за один раз добавляют в основном одну фичу и много исправлений.

## Подход к выбору ключевой функциональности

Создание продукта это серьезный труд, который в современной действительности выполняется целой командой, иногда даже не одной. По каждому пункту ниже можно написать книгу, поэтому в данной секции мы соберем индексные знания, полезные разработчику.

### Представим, что у вас есть идеи продукта-стартапа-бизнеса, чтобы превратить идею в продукт, необходимо пройти несколько этапов:

1. **От проблемы к решению**

   Для начала важно выявить и прозрачно сформулировать проблему пользователей, которую будет решать ваш продукт.

   Даже если у вас есть понимание проблемы и даже конкретное решение, стоит провести брейншторм, чтобы найти еще варианты решений.

   Каждый подходящий вариант решения проблемы пользователя теперь можно конвертировать в гипотезу, что позволит её в дальнейшем проверить.

2. **Поиск "сердца" продукта**

   После брейншторма, окажется, что идей довольно много, отказываться от них больно по разным причинам, а еще некоторые противоречат друг другу. Что делать?

   Использовать разные техники приоритизации:

   - **MoSCoW** (Обязательно; Желательно; Возможно; Не нужно).
   - **Матрица Эйзенхауэра** (Важно и срочно; Важно, но не срочно; Срочно, но не важно; Не важно и не срочно).
   - **Метод RICE** (Охват; Влияние; Уверенность; Усилия).
   - **Value vs Effort Matrix** (Высокая ценность/Низкие усилия; Высокая ценность/Низкие усилия; Низкая ценность/Низкие усилия; Низкая ценность/Высокие усилия).

   Далее, используя разнообразные подходы важно выделить самую главную функциональность, что делает ваш продукт уникальным?

   Конечно, же, стоит убедиться, что ваши предположения работают, ведь недостаточно провести анализ собственных идей, чтобы рынок принял новый продукт. Здесь бизнес-команда формирует гипотезу, для проверки которой, нужно написать код и выпустить пользователям.

3. **Убираем лишнее**

   Если рассмотреть задачу на примере Brello, то главная функциональность в нем - Kanban-доска. При этом, если изучить конкурентов, то выяснится, что Kanban-доски могут быть очень гибкими и иметь десятки фич поверх.

   При определении ключевой функциональности важно указать именно базовую функциональность. Например, возможность по хоткею создавать новые Kanban-карточки строится поверх самой Kanban-доски, то есть это не базовая фича.

   Просто убираем все фичи, которые основываются на каких-то других в беклог, до тех пор, пока продукт не сломается. После этого, добавляем обратно по одной, пока не заработает. Идея подхода в том, чтобы оставить голый минимум фич для запуска и не отвлекаться на всё неважное сейчас.

   Всем стартаперам, стоит помнить: любые фичи можно добавить позже. Если фундамент хорошо проработан, то добавление не потребует множества времени.

4. **Полировка ключевой функции**

   До начала реализации продукта, стоит изучить всё, что удалось собрать для первой версии. Все эти документы и другие кусочки знаний важно собрать в одном месте, проверить их совместимость друг с другом и отдать на изучение команде разработки.

   На этом этапе, ключевая функциональность проекта может немного меняться, чтобы удовлетворить ресурсам бизнеса и команды разработки, поэтому не нужно пренебрегать логированием. Впоследствии, не раз, придется искать причины тех или иных принятых решений.

   Даже после отрезания всего лишнего, останется внушительный пласт работ, который менеджер проекта нарежет на крупные задачи, а команда разработки придумает как их реализовать.

   Если в таск-менеджере стоит задача “Сделать продукт A”, то можно сразу удалить эту задачу и писать как придумается. Либо, разбить задачу на более мелкие (декомпозировать), проанализировать какие изменения потребуется внести, провести исследование кодовой базы, архитектуры и зависимостей, сохранить это все в задаче и оценить.

5. **Проверка идеи в реальном мире**

   После реализации ключевой функциональности, можно обвесить продукт метриками и отдать первой группе ранних последователей(early adopters), то есть пользователей, готовых пробовать ваш продукт на такой ранней стадии.

   Теперь, у команды разработки есть все данные, чтобы углубить свое понимание ключевой функции продукта. Именно ранние последователи дают команде самую важную обратную связь. Слушайте своих пользователей, они скажут вам то, чего Вы не замечали или не хотели замечать.

   Обратную связь стоит регулярно обрабатывать и сохранять в виде задач/проектов или комментариев к существующим задачам и проектам. Главное, чтобы эти заметки и идеи можно было легко найти в вашей базе знаний.

   А дальше, задача команды, искать баланс между простотой и эффективностью всех вносимых изменений в продукт, выражать новые функции в виде задач в таск-трекере и гарантировать взаимодействие бизнеса и команд разработки, чтобы все в равной степени понимали, какие изменения ждут продукт.

## Анализ требований

Важно рассмотреть продукт с точки зрения пользователя (из вашей ЦА и не только). Какие шаги должен выполнить пользователь, чтобы выполнить свою задачу? Сколько он потратит на это времени? Шаги очевидны из интерфейса или придется создавать обучающие гайды?

Для описания задач продукта можно использовать несколько подходов:

- **User Characters**: описываем несколько типичных представителей вашей ЦА в виде персонажей, чтобы лучше понять их потребности и проще делиться наработками внутри компании;
- **User Stories**: пошаговый сценарий, какие действия выполняет пользователь для достижения целей UserStory. Не всегда этот инструмент подходит стартапам, но можно утверждать, что пара-тройка user story поможет разработчикам лучше реализовать поведение интерфейса;
- **Customer Journey Map**: такие карты можно составлять вручную, до запуска продукта, а также автоматически на основе аналитики (Google Analytics, например). Эта карта, с одной стороны, позволяет представить как пользователь будет “путешествовать” по различным разделам вашего продукта, а с другой стороны, увидеть как в реальности пользователи взаимодействуют с вашими интерфейсами;

На основе собранных материалов можно писать E2E-автотесты, проверяющие работоспособность user stories, составлять техническое задание команде QA, определять список необходимых изменений в продукте. И это только верхушка, способов гораздо больше, стоит поговорить с вашей командой, чтобы выяснить, как бизнес собирает аналитику и требования.

Следующим этапом будет планирование и организация требований в задачи. Здесь можно использовать куда более понятные методики: Agile, SCRUM, Kanban. Главное, иметь доску с актуальным статусом продукта. Для этого уже есть достаточно готовых продуктов: Trello, Jira, Asana, Github Projects, Linear, и так далее.

## Применение на практике: Brello

**Brello** - это аналог Trello, веб-приложение для управления задачами с помощью досок, списков и карточек. Цель проекта - предоставить пользователям простой и интуитивно понятный инструмент для организации работы.

### Определение основной функциональности

- **Создание досок**: основное пространство для организации задач.
- **Добавление списков**: разделение задач на категории или этапы.
- **Создание карточек**: индивидуальные задачи или элементы работы.
- **Перетаскивание карточек**: изменение статуса или категории задачи.

В этот список очень хочется добавить: назначение автора, комментарии, оповещения и realtime, но я напоминаю, задача сделать меньше, но качественно и быстро.

### Планирование MVP

Для начала стоит составить список фич, без которых выпуск MVP для early adopters не имеет смысла.

- **Регистрация и вход пользователя**. Не получится пригласить пользователей, если нет аутентификации. Регистрацию можно сделать позже, например по инвайтам. У каждого пользователя должно быть свое пространство независимое от других пользователей.
- **Создание и управление досками**. Пользователю может потребоваться несколько пространств, для разных проектов.
- **Добавление списков и карточек**. Kanban подразумевает наличие списков, между которыми перемещаются карточки. Добавление новых списков и изменение их порядка, уже может быть сделано позже.
- **Базовая функциональность перетаскивания**. Drag’n’Drop сопряжен с множеством UX проблем, которые хорошо бы продумать или решить заранее, пока кода и фич не очень много.

### Определение технического стека

Чем меньше зависимостей, тем лучше для продукта, но такое возможно не всегда, поэтому важно зафиксировать начальный технический стек:

- React для описания интерфейса,
- TypeScript для типизации javascript,
- Mantine UI как готовые компоненты интерфейса,
- Effector для управления состоянием,
- Supabase в качестве Auth, Database, Realtime.

### Планирование разработки MVP

После определения ключевых фич продукта, стоит подумать о том, как разделить их на более мелкие этапы, которые будет проще реализовывать и планировать. Здесь могут быть разные подходы, но самое важное в них, это атомарность задач - одна задача только про одно функциональное изменение/добавление/удаление.

Кроме этого, стоит учитывать последовательность некоторых задач. Например, не получится реализовать запросы к серверу за карточками, если репозиторий полностью пустой, а для сервера еще нет механизма выдачи API-ключей.

Так же, стоит поднять приоритет задачам, которые описывают ключевую функциональность продукта. Например, аутентификация не является ключевой функцией, да без этой функции продуктом нужно будет иначе пользоваться, но аутентификация это явно не то, за чем приходят пользователи.

План на ближайший цикл разработки может выглядеть так:

1. **Создание доски после регистрации.** Так, не потребуется сначала реализовывать список досок, форму создания, редактирования и прочего. Можно выдать пользователям доску, созданную по шаблону.
2. **Добавление карточки**.
3. **Редактирование карточки**.
4. **Удаление карточки**.
5. **Перетаскивание карточки**.

Я намеренно убрал из этого списка почти всё, оставив фокус только на карточках на доске. Здесь нет даже создания и управления списками, это можно добавить позже, когда будет готов фундамент - управление карточками.

В следующем цикле разработки, можно добавить следующие самые необходимые фичи, например управление списками или редактирование доски.

### Тестирование MVP

После того как все самые необходимые функции продукта готовы (на самом деле можно и раньше), пришло время протестировать на реальных пользователях.

Начать стоит с развертывания продукта на production-серверах и выдачи доступа своим тестировщикам и другим членам команды. Так, все участники команд разработки смогут своими руками попробовать продукт и найти точки улучшения/исправления.

Следующий этап, это **закрытое тестирование**. Перед этим обычно выкладывают landing-page со сборщиком email, что упрощает формирование первой группы пользователей. Здесь важно обвесить продукт аналитикой, предупредить об этом пользователей, добавить форму обратной связи и проводить интервью-звонки, чтобы лучше понять потребителей продукта.

Вся собранная информация должна быть отсмотрена и проанализирована, иначе собирать её было бессмысленно. На основе результатов, можно подкорректировать план, создать задачи и начать итерацию разработки.

## Тестирование ключевой функциональности

Команда тестировщиков может проверять продукт полностью перед каждым релизом, но с ростом количества функции и их разнообразного использования, время на тестирование может потребоваться запредельное.

Решением могут стать автотесты, которые помогут обеспечить качества, за счет предотвращения регрессий, быстрого выявления проблем, так как не требуется время человека на проведение всего теста вручную.

В стартапах время обычно играет самую важную роль, поэтому зачастую в командах никто даже не задумывается о написании тестов. При этом, несколько ключевых тестов написать не сложно и не долго.

### Какие тесты можно реализовать для Brello

Для начала, стоит покрыть ключевую функциональность хотя бы парой тестов. Например, можно написать End-to-End(E2E) тесты для создания, редактирования и удаления карточек. Таким образом, можно быть уверенными, что хотя бы это работает после каждого мержа в main.

После реализации аутентификации, стоит написать хотя бы один тест, что пользователь может зарегистрироваться и войти в продукт. Не редки случаи, когда аутентификация продукта в какой-то момент сломалась, а менеджеры не могут понять, почему невероятно дорогая маркетинговая кампания не приносит ни одного пользователя.

В дальнейшем, при наличии времени стоит покрыть тестами и более специфичные кейсы, вроде проверки системы на отказ - банально ввести кривые данные в инпуты и убедиться, что система не позволяет продолжить работу в некорректном состоянии.

## Заключение

Создание продукта и выведение его на рынок это огромная работа, которая может быть выполнена одним человеком, но потребует колоссальных усилий и множества знаний. Даже командой не всегда удается запустить продукт в срок, не говоря уже о его качестве.

Какие ключевые выводы можно сделать:

- **Фокус на главной функциональности** продукта позволяет эффективно использовать доступные ресурсы и быстрее выходить на рынок;
- **Планирование и приоритизация** помогают избежать лишнего рефакторинга и технического долга, выводя вперед проработанные и качественные фичи;
- **Тестирование и обратная связь** обеспечивают соответствие бизнес-стратегии и качество продукта;
- **Качество против количества** при создании веб-продуктов позволяет быстрее выходить на рынок и завоевывать доверие аудитории.

Рекомендации для разработчиков при запуске нового продукта:

- **Начинайте с малого**: реализуйте минимально необходимый набор фич.
- **Будьте гибкими**: готовьтесь вносить изменения на основе обратной связи.
- **Сотрудничайте с командой**: регулярно общайтесь с дизайнерами, бэкенд-разработчиками и бизнес-аналитиками.
- **Инвестируйте в качество**: пишите тесты, документацию и поддерживайте чистоту кода

## Дополнительные ресурсы

Какие подходы и практики используют менеджеры продукта в работе:

- [Agile-разработка по данным Harvard Business Review](https://www.atlassian.com/ru/agile/advantage/agile-is-a-competitive-advantage)
- [Impact Mapping на практике](https://habr.com/ru/articles/246401/)
- [User Story Mapping: от идеи до релиза | IAMPM](https://iampm.club/blog/user-story-mapping-ot-idei-do-reliza/)
- [ProductStar: «Что делать в первую очередь»: приоритизация задач с помощью RICE»](https://productstar.ru/rice)
- [Гайд по продуктовой разработке для продакт-менеджеров: что должен знать продакт](https://library.wannabe.ru/article/product-delivery-guide)
- [Решение сложных задач ПМ: McKinsey's Mind Map](https://testengineer.ru/mckinseys-problem-solving/)
- [Фреймворк Double Diamond (Двойной алмаз)](https://productlab.ru/blog/double-diamond)
- [Linear Method - Practices for Building](https://linear.app/method)
- [Количественные и качественные методы исследований: инструкция](https://sense23.com/post/diy-gajd-po-metodam-produktovyh-issledovanij-kachestvennye-kolichestvennye-ux-issledovaniya)

Сбор обратной связи:

- [Сбор и анализ обратной связи от пользователей](https://hsbi.hse.ru/articles/sbor-i-analiz-obrat- noy-svyazi-ot-polzovateley/)
- [Получаем от пользователей обратную связь: 5 рекомендаций](https://www.uprock.ru/articles/- poluchaem-ot-polzovateley-obratnuyu-svyaz-5-rekomendaciy)
- [UX Writing - самый игнорируемый, но при этом один из самых важных навыков для UX/UI-дизайнера](https://medium.com/design-pub/ux-writing-%D1%81%D0%B0%D0%BC%D1%8B%D0%B9-%D0%B8%D0%B3%D0%BD%D0%BE%D1%80%D0%B8%D1%80%D1%83%D0%B5%D0%BC%D1%8B%D0%B9-%D0%BD%D0%BE-%D0%BF%D1%80%D0%B8-%D1%8D%D1%82%D0%BE%D0%BC-%D0%BE%D0%B4%D0%B8%D0%BD-%D0%B8%D0%B7-%D1%81%D0%B0%D0%BC%D1%8B%D1%85-%D0%B2%D0%B0%D0%B6%D0%BD%D1%8B%D1%85-%D0%BD%D0%B0%D0%B2%D1%8B%D0%BA%D0%BE%D0%B2-%D0%B4%D0%BB%D1%8F-ux-ui-%D0%B4%D0%B8%D0%B7%D0%B0%D0%B9%D0%BD%D0%B5%D1%80%D0%B0-45360f42bf4a)
- [The Lean Startup: How Today's Entrepreneurs Use Continuous Innovation to Create Radically - Successful Businesses](https://www.amazon.com/Lean-Startup-Entrepreneurs-Continuous-Innovation/dp/0307887898)

Как писать user-specific требования:

- [User Story](https://www.productplan.com/glossary/user-story/)
- [How to Write User Stories in Agile Software Development | Easy Agile](https://www.easyagile.com/blog/how-to-write-good-user-stories-in-agile-software-development/)
- [How to Create a Customer Journey Map: Template & Guide](https://www.hotjar.com/customer-journey-map/)
- [How To Build A Customer Journey Map](https://www.abtasty.com/blog/building-customer-journey-maps/)
- [Customer journey mapping guide for beginners](https://uxpressia.com/blog/customer-journey-map-guide-examples)-
- [Что такое CJM - Customer Journey Map? | Блог Roistat](https://roistat.com/rublog/customer-journey-map/)
