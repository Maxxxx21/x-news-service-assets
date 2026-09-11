Отчёт за неделю 4–11 сентября 2026. Ниже перечислено, что сделано, по репозиториям: News Service (news-searcher-ai-service) и Voice Lab (voice-lab-app-client и -server). В News Service смержено 11 PR, в Voice Lab 5 коммитов в feature-ветках. Скриншоты сняты с локальных стендов на тестовых данных.

#### News Service (news-searcher-ai-service)

**1) Релиз 4 сентября — выполнено**, [PR #123](https://github.com/xerion-tech/news-searcher-ai-service/pull/123) смержен в main

Собрал релиз из beta: таргетинг и доверие веток (DEV-707…710) и разнообразие креативов (DEV-724). Это версия 1.0.0, к ней же вышла статья [X News Service Official: быстрый старт](https://telegra.ph/X-News-Service-Official-bystryj-start-09-04).

**2) DEV-799 Порядок «Telegram → группа команды → ветка» — выполнено**, [PR #124](https://github.com/xerion-tech/news-searcher-ai-service/pull/124) смержен

Закрыл лазейку: ветка с доставкой в личные сообщения больше не обходит проверку группы команды. Кнопки Create branch и Manual теперь блокируются с объяснением причины (нет команды → нет Telegram → нет группы), команды без группы недоступны в пикерах, гайд в Help ведёт лида по шагам.

![Гейт создания ветки у Team Lead без подключённого Telegram](https://raw.githubusercontent.com/Maxxxx21/x-news-service-assets/941575b09fddb399bd9c69093e0ec8b7b31abdf4/articles/weekly-2026-09-04--09-11/dev-799-branch-gate.jpg)

**3) DEV-795, DEV-796, DEV-797 CRM-рынки, отметки команды в Spy, пикеры — выполнено**, [PR #125](https://github.com/xerion-tech/news-searcher-ai-service/pull/125) смержен

Добавил Sync from CRM для каталога рынков. В Spy заменил Send to review на отметку Highlight for <команда>: карточка сразу попадает в Team picks у всей команды без решения администратора, старые submissions и их маршруты убраны. В пикерах Markets и Brands выбранные строки теперь показаны и отмечены. Миграция v58.

![Отметка креатива для команды в карточке Spy](https://raw.githubusercontent.com/Maxxxx21/x-news-service-assets/941575b09fddb399bd9c69093e0ec8b7b31abdf4/articles/weekly-2026-09-04--09-11/dev-796-highlight-for-team.jpg)

![Team picks во вкладке Team library](https://raw.githubusercontent.com/Maxxxx21/x-news-service-assets/941575b09fddb399bd9c69093e0ec8b7b31abdf4/articles/weekly-2026-09-04--09-11/dev-796-spy-team-picks.jpg)

![Пикер рынков в Targeting с выбранной строкой](https://raw.githubusercontent.com/Maxxxx21/x-news-service-assets/941575b09fddb399bd9c69093e0ec8b7b31abdf4/articles/weekly-2026-09-04--09-11/dev-797-targeting-pickers.jpg)

**4) Нейтральные формулировки промптов креативов — выполнено**, [PR #126](https://github.com/xerion-tech/news-searcher-ai-service/pull/126) смержен

Переписал системный промпт креативов в policy-safe тоне, регуляторные бейджи сделал условными и явно запретил имитацию экранов выплат. Внешний вид не менялся.

**5) Релиз 7 сентября — выполнено**, [PR #127](https://github.com/xerion-tech/news-searcher-ai-service/pull/127) смержен в main

Довёл в main пункты 2–4.

**6) DEV-804 Рынки брендов и гео-макросы в Discovery — выполнено**, [PR #128](https://github.com/xerion-tech/news-searcher-ai-service/pull/128) смержен

Бренд теперь ищется только в рынках, где он продаётся по данным CRM: {casino} раскрывается по рынку, {casino:all} сохраняет старый полный перебор, а макрос {xx} (например {us}, {ca}) сужает запрос до одной страны. Это снимает перегрев компилятора (~213k задач за эпоху на проде). В админке рынки CRM-брендов показаны read-only, для ручных брендов есть выбор, оценка стоимости повторяет компилятор. Миграция v59.

![Конфигурация Discovery: каталог брендов с рынками и алиасами, Sync from CRM, макросы в шаблонах запросов](https://raw.githubusercontent.com/Maxxxx21/x-news-service-assets/941575b09fddb399bd9c69093e0ec8b7b31abdf4/articles/weekly-2026-09-04--09-11/dev-804-854-discovery-config.jpg)

**7) DEV-853 Brief ветки доходит до анализа — выполнено**, [PR #132](https://github.com/xerion-tech/news-searcher-ai-service/pull/132) смержен

Раньше Brief читал только подбор материалов. Теперь он дословно попадает в системные промпты триажа, сигналов и репортёра, с одним лимитом 10 000 знаков и счётчиком под полем; при пустом Brief промпты байт-в-байт прежние. Укрепил разбор ограждённых блоков (25 регрессионных кейсов на подделку закрывающего маркера), эмбеддинг профиля строится только из Goal. Есть ops-выключатель branchBriefEnabled.

![Счётчик Pre-prompt N / 10,000 в Edit branch](https://raw.githubusercontent.com/Maxxxx21/x-news-service-assets/941575b09fddb399bd9c69093e0ec8b7b31abdf4/articles/weekly-2026-09-04--09-11/dev-853-brief-counter.jpg)

**8) DEV-854 Точечные новости по казино-брендам — выполнено**, [PR #133](https://github.com/xerion-tech/news-searcher-ai-service/pull/133) смержен

Ветки Casino / brand публиковали новости «по штатам»: режим all ничего не фильтровал, 78 из 907 «брендов» были штатами и лотереями, алиасы были пустыми. Сделал вывод алиасов с бэкфиллом, распознавание бренда в документе и бренд-гейт на обоих путях чтения пула, ось «бренд» в триаже, сигналах и постах (одна рекомендация = один бренд, хэштег бренда первым, сортировка постов по бренду), пометку подозрительных брендов и кнопку Disable all suspicious, счётчики бренд-гейта на карточке ветки и тумблеры Brand scope на странице Filters. Промпты веток без бренд-скоупа не изменились (запинено 10 SHA-256 в тестах). Полный прогон: 486 файлов / 6568 тестов.

**9) DEV-855 Гео- и бренд-гейт пула ветки — выполнено**, [PR #134](https://github.com/xerion-tech/news-searcher-ai-service/pull/134) смержен

Ветка получала документы, которых не заказывала: Bing с cc=US приносил Онтарио, трастовые фиды писали один материал во все пулы. Теперь документ отбрасывается, если явно называет страну вне рынков ветки и ни одной её страны (без географии проходит), а бренд-ветка получает только документы со своим брендом. Отказы запоминаются в branch_scope_rejections и сбрасываются при смене таргетинга. Два runtime-тумблера на странице Filters, оба включены по умолчанию. Миграции v59–v60.

![Тумблеры Branch geo gate, Brand gate и Brand prompts на странице Filters](https://raw.githubusercontent.com/Maxxxx21/x-news-service-assets/941575b09fddb399bd9c69093e0ec8b7b31abdf4/articles/weekly-2026-09-04--09-11/dev-855-scope-gates.jpg)

**10) DEV-857 CI только на main — выполнено**, [PR #135](https://github.com/xerion-tech/news-searcher-ai-service/pull/135) смержен

Убрал beta из триггеров ci.yml: каждый PR в beta стоил ~10 минут раннера, а Actions организации заблокированы биллингом. Полный гейт остался на пути beta → main. Обновил AGENTS.md и TESTING.md.

**11) Релиз 9 сентября, версия 1.1.0 — выполнено**, [PR #136](https://github.com/xerion-tech/news-searcher-ai-service/pull/136) смержен в main

Довёл в main пункты 6–10 и опубликовал релизную статью [X News Service Official release 1.1.0](https://telegra.ph/X-News-Service-Official-release-110-09-09) с обложкой и скриншотами Spy. Картинки обеих статей вынес в отдельный публичный репозиторий x-news-service-assets.

#### Voice Lab (voice-lab-app-client, voice-lab-app-server)

**12) UX Studio и серверная основа под него — в работе, ветки feature/ux-studio (клиент) и feature/server-ui-foundation (сервер), PR пока нет**

Разрабатываю новый интерфейс VoiceLab: разделы Scene, Speech и Background Sound, библиотека голосов, единый плеер, карточки заданий с Reuse, Retry и скачиванием. Клиентская часть готова и закоммичена в feature/ux-studio; сейчас подключаю к ней сервер, чтобы настройки скорости, эмоции и фона реально применялись, а статусы очереди приходили с сервера, а не считались приблизительно.

![Раздел Scene в UX Studio: промпт, голос, скорость, эмоция и фон](https://raw.githubusercontent.com/Maxxxx21/x-news-service-assets/4ad15df5b517ff80cb24a5bbc89c969ffe4b4d89/articles/weekly-2026-09-04--09-11/voicelab-ux-studio-scene.jpg)

Что уже сделано на сервере (4 коммита за 11 сентября): FFmpeg в рантайме и воспроизводимый локальный запуск (T01), стабильный безопасный контракт ошибок HTTP и заданий (T13), проверка прав на выбранный голос при приёме и инференсе (T07), документ с возможностями моделей и рубрикой акустической приёмки. В работе: атомарное сохранение заданий и корректное завершение при остановке (часть T12).

План: 18 задач в docs/plans/2026-09-11-server-ui-orca, каждая в своём worktree от beta, до трёх исполнителей параллельно плюс независимое ревью. Порядок: окружение и перенос UX Studio в beta (T01, T02) → контракт запросов (T03) → передача настроек через bridge и права (T06, T07) → Speech и Scene применяют скорость, эмоцию, три режима фона и громкости (T08–T11) → стадии исполнения и реальная позиция в очереди (T12, T13) → клиент получает новые настройки и серверные статусы (T14, T15) → приёмка на реальном генераторе и включение только проверенных capability-флагов (T16, T17). Ожидаемый результат: флаги sceneDelivery, speechDelivery, sceneMix и backgroundSeed включаются по отдельности после реального аудио на используемой модели; пока GPU не подтвердил поддержку эмоций, соответствующий флаг остаётся выключенным, остальное продукт не блокирует.
