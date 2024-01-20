# Основы GIT

Сначала создаем новую ветку git branch [name] потом переходим на нее git checkout [name] и коммитим туда
Кстати вот тебе совет, ты можешь создать новую ветку и переключиться на неё с помощью одной команды:  git checkout -b [yourbranchname] 
### Порядок: 
* Создаем ветку и переводим HEAD в нее
* Работаем и коммитим 

## Ветки и слияния
```git merge``` - слияние или просто мердж. Слияния в Git создают особый вид коммита, который имеет сразу двух родителей.
Сначала уходим в новую ветку и коммитим в нее код, затем переходим в главную ветку, квммитим новый снимок и мерджим в нее измененную ветвь с фичами
Порядок:
* Переводим HEAD в ветку, куда будем мержить коммит 
* Выполняем слияние 


## Git Rebase
!!! Переносит текущий коммит и все родительские
Второй способ объединения изменений в ветках - это rebasing. При ребейзе Git по сути копирует набор коммитов и переносит их в другое место.
Несмотря на то, что это звучит достаточно непонятно, преимущество rebase в том, что c его помощью можно делать чистые и красивые линейные последовательности коммитов. История коммитов будет чище, если вы применяете rebase.
Уходим на новую ветку, коммитим там, затем rebase эту ветку к актуальной главной ветке и чтобы актуализировать все кометы переходим в главную ветку и rebase ее к нашему bugFix
Порядок:
* Переводим HEAD в коммит для ребэйза
* Производим rebase
* Актуализируем ветку master в которую мы перелили коммит до той ветки, которая была создана при rebase (переходим в нее (master) и выполняем команду rebase на ветку, которую перенесли.

## Прогулка по Git
HEAD
В первую очередь, поговорим о "HEAD". HEAD - это символическое имя текущего выбранного коммита — это, по сути, тот коммит, над которым мы в данный момент работаем.
HEAD всегда указывает на последний коммит из вашего локального дерева. Большинство команд Git, изменяющих рабочее дерево, начнут с изменения HEAD.
Обычно HEAD указывает на имя ветки (например, bugFix). Когда вы делаете коммит, статус ветки bugFix меняется и это изменение видно через HEAD.
Относительные ссылки
Передвигаться по дереву Git при помощи указания хешей коммитов немного неудобно. В реальной ситуации у вас вряд ли будет красивая визуализация дерева в терминале, так что придётся каждый раз использовать git log, чтобы найти хеш нужного коммита
Более того, хеши в реальном репозитории Git намного более длинные. Например, хеш для коммита, который приведён в предыдущем уровне - fed2da64c0efc5293610bdd892f82a58e8cbc5d8. Не очень просто для произношения =)
Хорошая новость в том, что Git достаточно умён в работе с хешами. Ему нужны лишь первые несколько символов для того, чтобы идентифицировать конкретный коммит. Так что можно написать просто fed2 вместо колбасы выше.
Как мы уже говорили, указание на коммит при помощи его хеша - не самый удобный способ, поэтому Git поддерживает относительные ссылки и они прекрасны!
С относительными ссылками можно начать с какого-либо удобного места (например, с ветки bugFix или от HEAD) и двигаться от него
Относительные ссылки - мощный инструмент, но мы покажем два простых способа использования:
Перемещение на один коммит назад ^
Перемещение на несколько коммитов назад ~<num>
Для начала рассмотрим оператор каретки (^). Когда мы добавляем его к имени ссылки, Git воспринимает это как указание найти родителя указанного коммита.
Так что main^ означает "первый родитель ветки main".
main^^ означает прародитель (родитель родителя) main

## Оператор "~"
Предположим, нужно переместиться на много шагов назад по дереву. Было бы неудобно печатать ^ несколько раз (или несколько десятков раз), так что Git поддерживает также оператор тильда (~).
Укажем после ~ число коммитов, через которые надо пройти: '''git checkout HEAD~3'''
Перемещение ветки (branch forcing)
Теперь мы разбираемся в относительных ссылках, так что можно реально использовать их для дела.
Одна из наиболее распространённых целей, для которых используются относительные ссылки - это перемещение веток. Можно напрямую прикрепить ветку к коммиту при помощи опции -f. Например, команда:
```git branch -f main HEAD~3```
Переместит (принудительно) ветку main на три родителя назад от HEAD.

## Отмена изменений в Git
```git reset HEAD~1```
Есть много путей для отмены изменений в Git. Так же как и коммит, отмена изменений в Git возможна и на низком уровне (добавление в коммит отдельных файлов и наборов строк), и на высоком (как изменения реально отменяются). Сейчас сфокусируемся на высокоуровневой части.
Есть два основных способа отмены изменений в Git: первый - это git reset, а второй - git revert. Попробуем оба на следующем шаге.

```git reset``` отменяет изменения, перенося ссылку на ветку назад, на более старый коммит. Это своего рода "переписывание истории"; git reset перенесёт ветку назад, как будто некоторых коммитов вовсе и не было.
Порядок: при reset указывается комит, до которого нужно произвести сброс

## Git Revert
```git revert HEAD```
Reset отлично работает на локальных ветках, в локальных репозиториях. Но этот метод переписывания истории не сработает на удалённых ветках, которые используют другие пользователи.
Чтобы отменить изменения и поделиться отменёнными изменениями с остальными, надо использовать git revert: Забавно, появился новый коммит. Дело в том, что новый коммит C2' просто содержит изменения, полностью противоположные тем, что сделаны в коммите C2.
После revert можно сделать push и поделиться изменениями с остальными.
Порядок: при revert указывается комит, который нужно ревертнуть и создать новый коммит без изменений

## Поперемещаем изменения
Итак, мы уже освоили основы Git: коммиты, ветки, перемещение по дереву изменений. Уже этих знаний достаточно, чтобы овладеть 90% мощью Git-репозиториев и покрыть нужды разработчиков.
А оставшиеся 10% будут очень полезны при сложных workflow (или если ты попал в сложную ситуацию). Теперь речь пойдёт о перемещении изменений — возможности, позволяющей разработчику сказать "Хочу, чтобы эти изменения были вот тут, а вот эти — вон там" и получить точные, правильные результаты, не теряя при этом гибкости разработки.
```git Cherry-pick```
Первая из таких команд - это ```git cherry-pick``` Она выглядит вот так:
```git cherry-pick <Commit1> <Commit2> ```
Это очень простой и прямолинейный способ сказать, что ты хочешь копировать несколько коммитов на место, где сейчас находишься (HEAD). Мы обожаем cherry-pick за то, что в нём очень мало магии и его очень просто понять и применять.

## Git Interactive Rebase
```git rebase -i HEAD~4```
Git cherry-pick прекрасен, когда точно известно, какие коммиты нужны (и известны их точные хеши)
Но как быть в случае, когда точно не известно какие коммиты нужны? К счастью, Git позаботился о таких ситуациях! Можно использовать интерактивный rebase для этого - лучший способ отобрать набор коммитов для rebase.
Всё, что нужно для интерактивного rebase - это опция -i
Если добавить эту опцию, Git откроет интерфейс просмотра того, какие коммиты готовы к копированию на цель rebase (target). Также показываются хеши коммитов и комментарии к ним, так что можно легко понять что к чему.
После открытия окна интерактивного rebase есть три варианта для каждого коммита:
Можно сменить положение коммита по порядку, переставив строчку с ним в редакторе (у нас в окошке строку с коммитом можно перенести просто мышкой).
Можно "выкинуть" коммит из ребейза. Для этого есть pick - переключение его означает, что нужно выкинуть коммит.
Наконец, можно соединить коммиты. В этом уровне игры у нас не реализована эта возможность, но, вкратце, при помощи этой функции можно объединять изменения двух коммитов.
Применение: 
* Можно применять из места куда нужно переместить всю ветку от родителей до потомков коммитов - git rebase <имя ветки куда> <имя ветки откуда>

# Продвинутый GIT

## Жонглируем коммитами
Вот ещё одна ситуация, которая часто случается. Есть некоторые изменения (newImage) и другие изменения (caption), которые связаны так, что находятся друг поверх друга в репозитории.
Штука в том, что иногда нужно внести небольшие изменения в более ранний коммит. В таком случае надо немного поменять newImage, несмотря на то, что коммит уже в прошлом!
Преодолеть эти трудности можно следующим образом:
Переставить коммит так, чтобы нужный находился наверху при помощи git rebase -i
Внести изменения при помощи '''git commit --amend'''
Переставить всё обратно при помощи '''git rebase -i'''
И наконец, переместить main на изменённую часть дерева, чтобы закончить уровень.
Это задание можно выполнить несколькими способами (и, гляжу, ты посматриваешь на cherry-picking), но сейчас сосредоточься на вышеописанном методе.
Обрати внимание на итоговое состояние в этом уровне – так как мы дважды перемещаем коммиты, оба они получат по апострофу. Ещё один апостроф добавляется, когда мы делаем '''git commit --amend'''.
Важно, чтобы совпадало не только дерево коммитов, но и количество апострофов.

## Жонглируем коммитами №2
Перед прохождением этого уровня обязательно надо пройти предыдущий уровень – 'Жонглируем коммитами №1'
В прошлом уровне мы использовали rebase -i, чтобы переставлять коммиты. Как только нужный нам коммит оказывался в конце, мы могли спокойно изменить его при помощи --amend и переставить обратно.
Единственная проблема тут - это множество перестановок, которые могут спровоцировать конфликты. Посмотрим, как с этой же задачей справится cherry-pick.
Важно помнить, что cherry-pick поместит любой коммит сразу после HEAD (только если этот коммит не является предком HEAD)

## Теги
```git tag <tag> <hash```
В прошлых уроках мы усвоили, что ветки просто двигать туда-сюда и они часто ссылаются на разные коммиты как на изменения данных в ветке. Ветки просто изменить, они часто временны и постоянно меняют своё состояние.
В таком случае, где взять постоянную ссылку на момент в истории изменений? Для таких вещей, как релиз и большие слияния, нужно нечто более постоянное, чем ветка.

## Git Describe
Теги являются прекрасными ориентирами в истории изменений, поэтому в git есть команда, которая показывает, как далеко текущее состояние от ближайшего тега. И эта команда называется git describe
```Git describe``` помогает сориентироваться после отката на много коммитов по истории изменений. Такое может случиться, когда вы сделали git bisect или если вы недавно вернулись из отпуска =)
Git describe выглядит примерно так:
```git describe <ref```
Где ref — это что-либо, что указывает на конкретный коммит. Если не указать ref, то git будет считать, что указано текущее положение (HEAD).
Вывод команды выглядит примерно так:
<tag>_<numCommits>_g<hash>
Где tag – это ближайший тег в истории изменений, numCommits – это на сколько далеко мы от этого тега, а hash – это хеш коммита, который описывается.

## Определение родителей
```git checkout HEAD~^2~2```
Так же как тильда (~), каретка (^) принимает номер после себя.
Но в отличие от количества коммитов, на которые нужно откатиться назад (как делает ~), номер после ^ определяет, на какого из родителей мерджа надо перейти. Учитывая, что мерджевый коммит имеет двух родителей, просто указать ^ нельзя.
Git по умолчанию перейдёт на "первого" родителя коммита, но указание номера после ^ изменяет это поведение.



# Работа с удаленными репозиториями GIT

## Push & Pull - удалённые репозитории в Git!

### Git Fetch
Работа с удалёнными git репозиториями сводится к передаче данных в и из других репозиториев. До тех пор, пока мы можем отправлять коммиты туда-обратно, мы можем делиться любыми изменениями, которые отслеживает git (следовательно, делиться новыми файлами, свежими идеями, любовными письмами и т.д.).
Что делает fetch
```git fetch``` выполняет две и только две основные операции. А именно:
связывается с указанным удалённым репозиторием и забирает все те данные проекта, которых у вас ещё нет, при этом...
у вас должны появиться ссылки на все ветки из этого удалённого репозитория (например, o/main)
Фактически, ```git fetch``` синхронизирует локальное представление удалённых репозиториев с тем, что является актуальным на текущий момент времени.
Насколько вы помните, в предыдущем уроке мы сказали, что удалённые ветки отображают состояние удалённых репозиториев на тот момент когда вы 'общались' с ними в последний раз. git fetch является тем механизмом, который даёт вам возможность общаться с удалёнными репозиториями! Надеюсь, что связь между удалёнными ветками и командой git fetch теперь прояснилась.
git fetch обычно 'общается' с удалёнными репозиториями посредством Интернета (через такие протоколы, как http:// или git://).
Чего fetch не делает
Важно отметить, что команда git fetch забирает данные в ваш локальный репозиторий, но не сливает их с какими-либо вашими наработками и не модифицирует то, над чем вы работаете в данный момент.
Важно это помнить и понимать, потому что многие разработчики думают, что, запустив команду git fetch, они приведут всю свою локальную работу к такому же виду, как и на удалённом репозитории. Команда всего лишь скачивает все необходимые данные, но вам потребуется вручную слить эти данные с вашими, когда вы будете готовы. В следующих уроках мы научимся это делать :D
Одним словом, вы можете относиться к git fetch как к процедуре скачивания.

### Git Pull

Теперь, когда мы познакомились с тем, как извлекать данные из удалённого репозитория с помощью git fetch, давайте обновим нашу работу, чтобы отобразить все эти изменения!
Существует множество вариантов решений - как только у вас имеется локальный коммит, вы можете соединить его с другой веткой. Это значит, вы можете выполнить одну из команд:
```git cherry-pick o/main```
```git rebase o/main```
```git merge o/main```
и т.д.
Процедура скачивания (fetching) изменений с удалённой ветки и объединения (merging) настолько частая и распространённая, что git предоставляет вместо двух команд - одну! Эта команда - git pull.

### Git Push
Хорошо, мы скачали изменения с удалённого репозитория и включили их в наши локальные наработки. Всё это замечательно, но как нам поделиться своими наработками и изменениями с другими участниками проекта?
Способ, которым мы воспользуемся, является противоположным тому способу, которым мы пользовались ранее для скачивания наработок (git pull). Этот способ - использование команды git push!
Команда git push отвечает за загрузку ваших изменений в указанный удалённый репозиторий, а также включение ваших коммитов в состав удалённого репозитория. По окончании работы команды git push все ваши друзья смогут скачать себе все сделанные вами наработки.
Вы можете рассматривать команду git push как "публикацию" своей работы. Эта команда скрывает в себе множество тонкостей и нюансов, с которыми мы познакомимся в ближайшее время, а пока что давайте начнём с малого...
замечание - поведение команды git push без аргументов варьируется в зависимости от значения push.default, указанной в настройках git-а. Значение по умолчанию зависит от версии git, которую вы используете, однако в наших уроках мы будем использовать значение upstream. Лучше всегда проверять эту опцию прежде чем push-ить ваши настоящие проекты.
Когда наработки расходятся
Вот мы и познакомились с тем, как забирать (pull) чужие коммиты и как закачивать (push) свои наработки и изменения. Выглядит всё довольно просто, и не ясно какие же могут возникать у людей трудности со всем этим?
Сложности возникают тогда, когда история репозитория расходится. Всё, что вам нужно - перебазировать свою работу на самую последнюю версию удалённой ветки.
Существует множество способов сделать это, но наиболее простой способ 'сдвинуть' свои наработки - через перебазировку или rebasing. Давайте посмотрим, как это выглядит.
```Git fetch```
```Git rebase origin/main```
```Git push```
Мы только что обновили наш локальный образ удалённого репозитория средствами git fetch. Ещё мы перебазировали наши наработки, чтобы они отражали все изменения с удалённого репозитория, и опубликовали их с помощью git push.

А есть ещё какие-либо варианты обновить мои наработки к тому моменту, как удалённый репозиторий был обновлён? Конечно есть! Давайте ознакомимся с парочкой новых штучек, но в этот раз с помощью команды merge.
Несмотря на то, что git merge не передвигает ваши наработки (а всего лишь создаёт новый коммит, в котором Ваши и удалённые изменения объединены), этот способ помогает указать git-у на то, что вы собираетесь включить в состав ваших наработок все изменения с удалённого репозитория. Это значит, что ваш коммит отразится на всех коммитах удалённой ветки, поскольку удалённая ветка является предком вашей собственной локальной ветки.
Таким образом, если мы объединим (merge) вместо перебазирования (rebase)…
```Git fetch```
```Git merge origin/main```
```Git push```
Мы обновили наше локальное представление удалённого репозитория с помощью git fetch, объединили ваши новые наработки с нашими наработками (чтобы отразить изменения в удалённом репозитории) и затем опубликовали их с помощью git push.

А можно ли как-то сделать всё то же самое, но с меньшим количеством команд?
Конечно - ведь вы уже знаете команду git pull, которая является аналогом и более кратким аналогом для совместных fetch и merge. А команда 
```git pull --rebase``` - аналог для совместно вызванных fetch и rebase!

## Remote Rejected!
Когда вы работаете в составе большой команды разработчиков над проектом, то, вероятнее всего, ветвь main будет заблокирована. Для внесения изменений в неё в git существует понятие запроса на слияние Pull Request. В такой ситуации если вы закоммитите свои наработки непосредственно в main ветвь, а после выполните git push, то будет сгенерировано сообщение об ошибке.
Удалённый репозиторий отклонил загруженные коммиты непосредственно в main ветку потому, что на main настроена политика, которая требует использование Pull request вместо обычного git push.
Эта политика подразумевает процесс создания новой ветви разработки, внесение в неё всех необходимых коммитов, загрузка изменений в удалённый репозиторий и открытие нового Pull request. Однако вы забыли про это и закоммитили наработки непосредственно в main ветвь. Теперь вы застряли и не можете запушить свои изменения :(.
Решение:
Создайте ещё одну ветвь под названием feature и отправьте изменения на удалённый репозиторий. Также не забудьте вернуть вашу локальную main ветвь в исходное состояние (чтобы она была синхронизирована с удалённой). В противном случае у вас могут возникнуть проблемы при следующем выполнении git pull.

## Слияние фича-бранчей (веток)
Теперь, когда вы умело владеете командами fetch, pull и push, давайте применим эти навыки в сочетании с новым рабочим процессом (он же workflow).
Среди разработчиков, вовлечённых в большой проект, довольно распространённ приём — выполнять всю свою работу в так называемых фича-бранчах (вне main). А затем, как только работа выполнена, разработчик интегрирует всё, что было им сделано. Всё это, за исключением одного шага, похоже на предыдущий урок (там, где мы закачивали ветки на удалённый репозиторий)
Ряд разработчиков делают push и pull лишь на локальную ветку main - таким образом ветка main всегда синхронизирована с тем, что находится на удалённом репозитории (o/main).
Для этого рабочего процесса мы совместили две вещи:
интеграцию фича-бранчей в main (git pull —rebase)
закачку (push) и скачку (pull) с удалённого репозитория (gut push)


## Удалённые-отслеживаемые ветки
Вы можете сказать любой из веток, чтобы она отслеживала o/main, и если вы так сделаете, эта ветка будет иметь такой же пункт назначения для push и merge как и локальная ветка main. Это значит, что вы можете выполнить git push, находясь на ветке totallyNotMain, и все ваши наработки с ветки totallyNotMain будут закачены на ветку main удалённого репозитория!
Есть два способа сделать это. Первый - это выполнить checkout для новой ветки, указав удалённую ветку в качестве ссылки. Для этого необходимо выполнить команду
```git checkout -b totallyNotMain o/main```
, которая создаст новую ветку с именем totallyNotMain и укажет ей следить за o/main.
Способ №2
Другой способ указать ветке отслеживать удалённую ветку — это просто использовать команду git branch -u. Выполнив команду
```git branch -u o/main foo```
вы укажете ветке foo следить за o/main. А если вы ещё при этом находитесь на ветке foo, то её можно не указывать:
```git branch -u o/main```

## Аргументы команды Push
```git push origin main```
Отлично! Теперь, когда вы знаете, как следить за удалёнными ветками, мы можем начать изучение того, что скрыто под занавесом работы команд git push, fetch и pull. Мы будем рассматривать одну команду за другой, однако принципы у них очень схожи.
Сперва взглянем на git push. В уроке, посвящённом слежению за удалённым репозиторием, вы узнали о том, что git находит удалённый репозиторий и ветку, в которую необходимо push-ить, благодаря свойствам текущей ветки, на которой мы находимся. Это так называемое поведение без аргументов, однако команда git push может быть также использована и с аргументами. Вид команды в данном случае:
```git push <удалённый_репозиторий> <целевая_ветка```

## Подробности аргумента <пункт назначения>
Помните, когда в прошлом занятии мы указали в качестве аргумента ветку main для команды git push, мы указали совместно источник, откуда будут приходить коммиты, и пункт назначения (получатель), куда коммиты будут уходить.
Однако, вы, наверное, задаётесь вопросом - а что, если я хочу, чтобы мои источник и получатель коммитов были различными? Что, если мы хотим запушить коммиты из локальной ветки foo в ветку bar на удалённом репозитории?
В том случае, когда вам необходимо разделить источник и получатель аргумента <пункт назначения>, соедините их вместе, используя двоеточие:
```git push origin <источник>:<получатель```
Обычно это называется refspec. Refspec — это всего лишь модное имя для определения местоположения, которое git может распознать (например, ветка foo или просто HEAD~1)
Как только вы указали источник и получатель независимо друг от друга, вы можете довольно причудливо и точно использовать команды для работы с удалёнными ветками и репозиториями.
А что если пункт назначения, в который вы хотите запушить, не существует? Без проблем! Укажите имя ветки, и git сам создаст ветку на удалённом репозитории для вас.

## Аргументы git fetch
Итак, мы только что изучили всё, что касается аргументов git push, мы узнали о параметре <пункт назначения>, и даже об аргументе, задающем отдельно источник и получатель коммитов (<источник>:<получатель>). Можем ли мы применить все эти полученные знания для команды git fetch ?
Ещё бы! Аргументы для команды git fetch на самом деле очень, очень похожи на те, что мы использовали в git push. В данном случае применяется тот же подход, только в противоположном направлении (так как теперь вы скачиваете коммиты, а не закачиваете их).
Давайте ознакомимся с принципами один за одним…
Параметр <пункт назначения>
Если вы указываете пункт назначения в команде git fetch, например так, как в следующем примере:
```git fetch origin foo```
Git отправится в ветку foo на удалённом репозитории, соберёт с собой все коммиты, которые не присутствуют локально, и затем поместит их в локальную ветку под названием o/foo.
Давайте взглянем на всё это в действии (чтобы освежить в памяти).
"Что же тогда произойдёт, если я явно укажу оба параметра: и источник и получатель, пользуясь синтаксисом <источник>:<получатель> ?"
Если вы уверены в том, что хотите закачать коммиты прямиком в вашу локальную ветку, тогда да, вы можете явно указать источник и получатель через двоеточние. Вы можете воспользоваться таким приёмом лишь для ветки, на которой вы не находитесь в настоящий момент checkout.
Теперь у нас <источник> - это место на удалённом репозитории, а <получатель> - место в локальном репозитории, в который следует помещать коммиты. Аргументы в точности до наоборот повторяют git push, и немудрено, ведь теперь мы переносим данные в обратном направлении!
Как уже было сказано, разработчики редко используют такой подход на практике. Целью демонстрации этой возможности было показать, насколько схожи концептуально fetch и push. Их отличие лишь в направлении переноса данных.

## Странный <источник>
Git использует параметр <источник> странным образом. Странность заключается в том, что Вы можете оставить пустым параметр <источник> для команд git push и git fetch:
```git push origin :side```
```git fetch origin :bugFix```
Посмотрим, что же из этого выйдет…
Что же будет с веткой, на которую мы делаем git push с пустым аргументом <источник>? Она будет удалена!
Наконец, если мы попытаемся притянуть изменения(git fetch) из "ничего" к нам в локальный репозиторий, то это создаст у нас новую ветку

## Аргументы для pull
Аргументы для git pull не покажутся вам чем-то новым, учитывая, что вы уже знакомы с аргументами для git fetch и git push :)
Как мы помним, git pull сначала выполняет git fetch, а следом сразу git merge с той веткой, в которую притянулись обновления командой fetch. Другими словами, это все равно, что выполнить git fetch с теми же аргументами, которые вы указали для pull, а затем выполнить git merge с веткой, указанной в аргументе <приемник> команды pull.
Вот примеры абсолютно эквивалентных команд в git:
```git pull origin foo``` это то же самое, что сделать:
```git fetch origin foo``` 
```git merge o/foo```
И еще...
```git pull origin bar~1:bugFix``` то же, что:
```git fetch origin bar~1:bugFix```
```git merge bugFix```
Как видно, git pull используется, чтобы за одну команду выполнить fetch + merge.

----

Дополнительные источники
[шпаргалка 1](https://gist.github.com/fomvasss/8dd8cd7f88c67a4e3727f9d39224a84c) 
line
[шпаргалка 1](https://www.markdownguide.org/cheat-sheet/)
