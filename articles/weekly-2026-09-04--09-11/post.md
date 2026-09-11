Отчёт за неделю 4–11 сентября 2026. Ниже перечислено, что сделано, по репозиториям: News Service (news-searcher-ai-service), CRM (xerion-client и xerion-server), PWA Manager (xerion-pwa-manager-client и -server), Voice Lab (voice-lab-app-client и -server). Всего за неделю смержено 15 PR, ещё 7 коммитов ушли напрямую в main и в feature-ветки. Скриншоты сняты с локальных стендов на тестовых данных.

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

#### CRM (xerion-client, xerion-server)

**12) DEV-664 Requested by и вкладка Not shared — выполнено**, [PR #220](https://github.com/xerion-tech/xerion-server/pull/220) и [PR #256](https://github.com/xerion-tech/xerion-client/pull/256) смержены

На сервере добавил поле requestedById для Apps, PWA's и Landings: заказчик = первый байер, с которым поделились, ставится один раз во всех девяти путях записи, есть бэкфилл-миграция. В листингах появился фильтр requestedById с сентинелом «none» и режим доступа unshared, единая валидация ObjectId. На клиенте — мультиселект Requested by с закреплённым пунктом No requester, колонка и строка в деталях, четвёртая вкладка Not shared в By Access; всё держится в URL.

![Фильтр Not shared и селектор Requested by в панели фильтров Apps](https://raw.githubusercontent.com/Maxxxx21/x-news-service-assets/941575b09fddb399bd9c69093e0ec8b7b31abdf4/articles/weekly-2026-09-04--09-11/dev-664-not-shared-filter.jpg)

![Список Requested by с пунктом No requester и байерами](https://raw.githubusercontent.com/Maxxxx21/x-news-service-assets/941575b09fddb399bd9c69093e0ec8b7b31abdf4/articles/weekly-2026-09-04--09-11/dev-664-requested-by.jpg)

**13) DEV-697 Скачивание креативов по списку creoID — выполнено**, [PR #223](https://github.com/xerion-tech/xerion-server/pull/223) и [PR #258](https://github.com/xerion-tech/xerion-client/pull/258) смержены

Новый эндпоинт принимает до 200 creoID, проверяет доступ и возвращает задачу на ZIP плюс отчёт по каждому id (queued / no access / not found); файлы в архиве названы по creoID, поддержан режим уникализации с applyCount. На клиенте — кнопка Download by IDs на странице Creatives, модалка с отчётом, кнопки Download Unique и Download Originals, задача сразу видна в Processes. Попутно починил примитив Tooltip (подсказка перекрывала меню и модалки) и добавил вложенные модалки.

![Модалка Download by creo IDs со вставленным списком](https://raw.githubusercontent.com/Maxxxx21/x-news-service-assets/941575b09fddb399bd9c69093e0ec8b7b31abdf4/articles/weekly-2026-09-04--09-11/dev-697-download-by-ids.jpg)

![Отчёт после запуска: сколько поставлено в очередь и какие id не найдены](https://raw.githubusercontent.com/Maxxxx21/x-news-service-assets/941575b09fddb399bd9c69093e0ec8b7b31abdf4/articles/weekly-2026-09-04--09-11/dev-697-report.jpg)

#### PWA Manager (xerion-pwa-manager-server, xerion-pwa-manager-client)

**14) Custom PWA builder, контракт контента v1 — выполнено, коммиты [c65ae22](https://github.com/xerion-tech/xerion-pwa-manager-server/commit/c65ae22) в main сервера и [a295e8b](https://github.com/xerion-tech/xerion-pwa-manager-client/commit/a295e8b) в ветке client-builder-fields**

Сервер: язык карточки и словари текстов en/ru/uk с переопределениями, install flow (стадии загрузки, авто-открытие оффера, Back Button URL, passthrough параметров), splash, cookie-баннер, TikTok preland v1–v3, дизайн (обложка, акцентный цвет), лайки отзывов, эндпоинты /locales, /text-defaults и POST /preview для рендера черновика; легаси-записи читаются без миграции. Клиент: билдер перестроен в двенадцать секций, текстовые поля показывают дефолт языка как placeholder, drag-and-drop скриншотов, предпросмотр черновика в iframe с debounce и переключателями состояния и ОС.

![Билдер Custom PWA: секция Basic с языком карточки и предпросмотром](https://raw.githubusercontent.com/Maxxxx21/x-news-service-assets/941575b09fddb399bd9c69093e0ec8b7b31abdf4/articles/weekly-2026-09-04--09-11/pwa-builder-basic.jpg)

![Секция Install: кнопки, стадии загрузки, iOS-подсказка](https://raw.githubusercontent.com/Maxxxx21/x-news-service-assets/941575b09fddb399bd9c69093e0ec8b7b31abdf4/articles/weekly-2026-09-04--09-11/pwa-builder-install.jpg)

#### Voice Lab (voice-lab-app-server, voice-lab-app-client)

**15) Сервер: безопасный HTTP-контракт, авторизация голосов, FFmpeg — в работе, 4 коммита в ветке feature/server-ui-foundation (11 сентября), PR пока нет**

Добавил стабильный контракт ошибок HTTP и задач, авторизацию выбранных голосов при приёме и инференсе, укрепил рантайм FFmpeg и проверил локальные аудио-сценарии, зафиксировал в документации возможности доставки моделей и рубрику акустической приёмки.

**16) Клиент: интерфейс UX Studio — в работе, коммит в ветке feature/ux-studio (11 сентября), PR пока нет**

Завершил интерфейс UX Studio и положил рядом план серверной реализации (docs/plans/2026-09-11-server-ui-orca: контракты, аудит GPU и сервера, QA-матрица, задачи T01+).
