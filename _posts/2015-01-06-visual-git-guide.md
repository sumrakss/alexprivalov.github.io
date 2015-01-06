---
layout: post
title: "Git: наглядная справка"
permalink: visual-git-guide
tags: git
---

Краткая наглядная справка для наиболее часто используемых команд git. Если у вас уже есть небольшой опыт работы с git, эта страница поможет вам закрепить ваши знания. Создано на основе этого [сайта](https://marklodato.github.io/visual-git-guide/index-en.html).

---

<head>
  <!-- CSS -->
  <link rel="stylesheet" href="{{ site.baseurl }}public/css/visual-git-guide.css">

</head>

  <h2 id="contents">Содержание</h2>
  <ol>
    <li><a href="#basic-usage">Основные команды</a></li>
    <li><a href="#conventions">Соглашения</a></li>
    <li><a href="#commands-in-detail">Подробно о командах</a>
      <ol>
        <li><a href="#diff">Diff</a></li>
        <li><a href="#commit">Commit</a></li>
        <li><a href="#checkout">Checkout</a></li>
        <li><a href="#detached">Коммит из состояния «Detached HEAD»</a></li>
        <li><a href="#reset">Reset</a></li>
        <li><a href="#merge">Merge</a></li>
        <li><a href="#cherry-pick">Cherry Pick</a></li>
        <li><a href="#rebase">Rebase</a></li>
      </ol>
    </li>
    <li><a href="#technical-notes">Технические заметки</a></li>
  </ol>

  <h2 id="basic-usage">Основные команды</h2>

  <div class="center"><img src='{{ site.baseurl }}images/basic-usage.svg.png'></div>

  <p>Следующие четыре команды предназначены для копирования файлов между
  рабочей директорией, сценой (так же известной как «индекс») и историей
  (представленной в форме коммитов).</p>

  <ul>

    <li><code>git add <em>файлы</em></code> копирует <em>файлы</em> (в их
    текущем состоянии) на сцену.</li>

    <li><code>git commit</code> сохраняет снимок сцены в виде коммита.</li>

    <li><code>git reset -- <em>файлы</em></code> восстанавливает файлы на
    сцене, а именно копирует <em>файлы</em> из последнего коммита на сцену.
    Используйте эту команду для отмены изменений, внесенных командой
    <code>git add <em>файлы</em></code>.  Вы также можете выполнить
    <code>git reset</code> чтобы восстановить все файлы на сцене.</li>

    <li><code>git checkout -- <em>файлы</em></code> копирует <em>файлы</em> со
    сцены в рабочую директорию.  Эту команду удобно использовать чтобы
    сбросить нежелательные изменения в рабочей директории.</li>

  </ul>

  <p>Вы можете использовать <code>git reset -p</code>,
  <code>git checkout -p</code>, и <code>git add -p</code> вместо
  (или вместе с) именами файлов, чтобы в интерактивном режиме выбирать,
  какие именно изменения будут скопированы.</p>

  <p>Также можно перепрыгнуть через сцену и сразу же получить файлы из истории
  прямо в рабочую директорию; или сделать коммит, минуя сцену.</p>

  <div class="center"><img src='{{ site.baseurl }}images/basic-usage-2.svg.png'></div>

  <ul>

    <li><code>git commit -a </code> аналогичен запуску двух команд:
    <tt>git add</tt> для всех файлов, которые существовали в предыдущем
    коммите, и <tt>git commit</tt>.</li>

    <li><code>git commit <em>файлы</em></code> создает новый коммит, в основе
    которого лежат уже существующие файлы, добавляя изменения только для
    указанных <em>файлов</em>.  Одновременно, указанные <em>файлы</em> будут
    скопированы на сцену.</li>

    <li><code>git checkout HEAD -- <em>файлы</em></code> копирует
    <em>файлы</em> из текущего коммита и на сцену, и в рабочую
    директорию.</li>

  </ul>

  <h2 id="conventions">Соглашения</h2>

  <p>Иллюстрации в этой справке выдержаны в единой цветовой схеме.</p>

  <div class="center"><img src='{{ site.baseurl }}images/conventions.svg.png'></div>

  <p>Коммиты раскрашены зеленым цветом и подписаны 5-ти буквенными
  идентификаторами.  Каждый коммит указывает на своего родителя зеленой
  стрелочкой.  Ветки раскрашены оранжевым цветом; ветки указывают на коммиты.
  Специальная ссылка <em>HEAD</em> указывает на текущую ветку.  На иллюстрации
  вы можете увидеть последние пять коммитов. Самый последний коммит имеет хеш
  <em>ed489</em>. <em>master</em> (текущая ветка) указывает на этот коммит,
  <em>maint</em> (другая ветка) указывает на предка <em>master</em>-ового
  коммита.</p>

  <h2 id="commands-in-detail">Подробно о командах</h2>

  <h3 id="diff">Diff</h3>

  <p>Есть много способов посмотреть изменения между коммитами.  Ниже вы
  увидите несколько простых примеров.  К каждой из этих команд можно добавить
  имена файлов в качестве дополнительного аргумента.  Так мы выведем
  информацию об изменениях только для перечисленных файлов.</p>

  <div class="center"><img src='{{ site.baseurl }}images/diff.svg.png'></div>

  <h3 id="commit">Commit</h3>

  <p>Когда вы делаете коммит, git создает новый объект коммита, используя файлы
  со сцены, а текущей коммит становится родителем для нового. После этого
  указатель текущей ветки перемещается на новый коммит.  Вы это видите на
  картинке, где <em>master</em> — это текущая ветка.  До совершения коммита
  <em>master</em> указывал на коммит <em>ed489</em>.  После добавления нового
  коммита <em>f0cec</em>, родителем которого стал <em>ed489</em>, указатель
  ветки <em>master</em> был перемещен на новый коммит.</p>

  <div class="center"><img src='{{ site.baseurl }}images/commit-master.svg.png'></div>

  <p>То же самое происходит, если одна ветка является предком другой ветки.
  Ниже показан пример нового коммита <em>1800b</em> в ветке <em>maint</em>,
  которая является предком ветки <em>master</em>.  После этого ветка
  <em>maint</em> уже больше не является предком ветки <em>master</em>. И в
  случае необходимости объединения работы, проделанной в этих разделенных
  ветках, вам следует воспользоваться командой <a href='#merge'>merge</a> (что
  более предпочтительно) или <a href='#rebase'>rebase</a>.</p>

  <div class="center"><img src='{{ site.baseurl }}images/commit-maint.svg.png'></div>

  <p>Если вы сделали ошибку в последнем коммите, её легко исправить с помощью
  команды <code>git commit --amend</code>.  Эта команда создает новый коммит,
  родителем которого будет родитель ошибочного коммита. Старый ошибочный
  коммит будет отброшен, конечно же если только на него не будет ещё
  каких-либо других ссылок, что маловероятно.</p>

  <div class="center"><img src='{{ site.baseurl }}images/commit-amend.svg.png'></div>

  <p>Четвертый случай коммита из состояния «<a href="#detached">detached
    HEAD</a>» будет рассмотрен далее.</p>

  <h3 id="checkout">Checkout</h3>

  <p>Команда checkout используется для копирования файлов из истории или сцены
  в рабочую директорию. Также она может использоваться для переключения между
  ветками.</p>

  <p>Когда вы указываете имя файла (и/или ключ <code>-p</code>), git копирует
  эти файлы из указанного коммита на сцену и в рабочую директорию.  Например,
  <code>git checkout HEAD~ foo.c</code> копирует файл <code>foo.c</code>
  из коммита <em>HEAD~</em> (предка текущего коммита) в рабочу директорию и на
  сцену.  Если имя коммита не указано, то файл будет скопирован со сцены в
  рабочую директорию.  Обратите внимание на то что при выполнении команды
  <code>checkout</code> позиция указателя текущей ветки (HEAD) остаётся
  прежней, указатель никуда не перемещается.</p>

  <div class="center"><img src='{{ site.baseurl }}images/checkout-files.svg.png'></div>

  <p>В том случае если мы <em>не указываем</em> имя файла, но указываем имя
  (локальной) ветки, то указатель <em>HEAD</em> будет перемещен на эту ветку
  (мы переключимся на эту ветку). При этом сцена и рабочая директория будут
  приведены в соответствие с этим коммитом. Любой файл, который присутствует в
  новом коммите (<em>a47c3</em> ниже) будет скопирован из истории; любой файл,
  который был в старом коммите (<em>ed489</em>), но отсутствует в новом, будет
  удален; любой файл, который не записан ни в одном коммите, будет
  проигнорирован.</p>

  <div class="center"><img src='{{ site.baseurl }}images/checkout-branch.svg.png'></div>

  <p>В том случае, если мы <em>не указываем</em> имя файла, и
  <em>не указываем</em> имя (локальной) ветки, а указываем тег, дистанционную
  (remote) ветку, SHA-1 хеш коммита или что-то вроде <em>master~3</em>, то мы
  получаем безымянную ветку, называемую «<em>Detached HEAD</em>» (оторванная
  голова). Это очень полезная штука, если нам надо осмотреться в истории
  коммитов. К примеру, вам захочется скомпилировать git версии 1.6.6.1. Вы
  можете набрать <code>git checkout v1.6.6.1</code> (это тег, не ветка),
  скомпилировать, установить, а затем вернуться в другую ветку, скажем
  <code>git checkout master</code>. Тем не менее, коммиты из состояния
  «Detached HEAD» происходят по своим особым важным правилам, и мы рассмотрим
  их <a href="#detached">ниже</a>.</p>

  <div class="center"><img src='{{ site.baseurl }}images/checkout-detached.svg.png'></div>

  <h3 id="detached">Коммит из состояния «Detached HEAD»</h3>

  <p>Когда мы находимся в состоянии оторванной головы
  (<em>Detached HEAD</em>), коммит совершается по тем же правилам, что и
  обычно за исключением одной маленькой особенности: ни один указатель ветки
  не будет изменен или добавлен к новому коммиту.  Вы можете представить эту
  ситуацию как работу с анонимной веткой.</p>

  <div class="center"><img src='{{ site.baseurl }}images/commit-detached.svg.png'></div>

  <p>Если после такого коммита вы переключитесь в ветку <em>master</em>,
  то коммит <em>2eecb</em>, совершенный из состояния «Detached HEAD»
  потеряется и попросту будет уничтожен очередной сборкой мусора только потому
  нет ни одного объекта, который бы на него ссылался: ни ветки, ни тега.</p>

  <div class="center"><img src='{{ site.baseurl }}images/checkout-after-detached.svg.png'></div>

  <p>В том случае, если вы хотите сохранить этот коммит на будущее, вы можете
  создать на основе его новую ветку командой
  <code>git checkout -b <em>new</em></code>.</p>

  <div class="center"><img src='{{ site.baseurl }}images/checkout-b-detached.svg.png'></div>

  <h3 id="reset">Reset</h3>

  <p>Команда reset перемещает указатель текущей ветки в другую позицию, и
  дополнительно может обновить сцену и рабочую директорию. Эту команду можно
  также использовать для того чтобы скопировать файл из истории на сцену, не
  задевая рабочую директорию.</p>

  <p>Если коммит указан без имен файлов, указатель ветки будет перемещен на
  этот коммит, а затем сцена приведется в соответствие с этим коммитом. Если
  мы используем ключ <code>--soft</code>, то сцена не будет изменена. Если мы
  используем ключ <code>--hard</code>, то будет обновлена и сцена, и рабочая
  директория.</p>

  <div class="center"><img src='{{ site.baseurl }}images/reset-commit.svg.png'></div>

  <p>Если имя коммита не будет указано, по умолчанию оно будет <em>HEAD</em>.
  В этом случае указатель ветки не будет перемещен, но сцена (а также и
  рабочая директория, если был использован ключ <code>--hard</code>) будут
  приведены к состоянию последнего коммита.</p>

  <div class="center"><img src='{{ site.baseurl }}images/reset.svg.png'></div>

  <p>Если в команде указано имя файла (и/или ключ <code>-p</code>), то команда
  работает также как <a href='#checkout'>checkout</a> с именем файла, за
  исключением того, что только сцена (но не рабочая директория) будет
  изменена. Если вы подставите имя коммита на место двойной черты, вы сможете
  получить состояние файла из этого коммита, тогда как в случае с двойной
  чертой вы получите состояние файла из коммита, на который указывает
  <em>HEAD</em>.</p>

  <div class="center"><img src='{{ site.baseurl }}images/reset-files.svg.png'></div>

  <h3 id="merge">Merge</h3>

  <p>Команда merge (слияние) создает новый коммит на основе текущего коммита,
  применяя изменения других коммитов. Перед слиянием сцена должна быть
  приведена в соответствие с текущим коммитом. Самый простой случай слияния —
  это когда другой коммит является предком текущего коммита: в этом случае
  ничего не происходит. Другой простой случай слияния — когда текущий коммит
  является предком другого коммита: в этом случае происходит <em>быстрая
  перемотка</em> (<em>fast-forward</em>). Ссылка текущей ветки будет просто
  перемещена на новый коммит, а сцена и рабочая директория будут приведены в
  соответствие с новым коммитом.</p>

  <div class="center"><img src='{{ site.baseurl }}images/merge-ff.svg.png'></div>

  <p>Во всех других случаях выполняется «настоящее» слияние. Вы можете
  изменить стратегию слияния, но по умолчанию будет выполнено «рекурсивное»
  слияние, для которого будет взят текущий коммит (<em>ed489</em> ниже на
  схеме), другой коммит (<em>33104</em>) и их общий предок (<em>b325c</em>); и
  для этих трех коммитов будет выполнено
  <a href='http://en.wikipedia.org/wiki/Three-way_merge'>трехстороннее
  слияние</a>. Результат этого слияние будет записан в рабочую директорию и на
  сцену, и будет добавлен результирующий коммит со вторым родителем
  (<em>33104</em>).</p>

  <div class="center"><img src='{{ site.baseurl }}images/merge.svg.png'></div>

  <h3 id="cherry-pick">Cherry Pick</h3>

  <p>Команда cherry-pick («вишенка в тортике») создает новый коммит на основе
  только одного сладкого «коммита-вишенки», применив все его изменения
  и сообщение.</p>

  <div class="center"><img src='{{ site.baseurl }}images/cherry-pick.svg.png'></div>

  <h3 id="rebase">Rebase</h3>

  <p>Перебазирование (rebase) — это альтернатива <a href='#merge'>слиянию</a>
  для задач объединения нескольких веток. Если слияние создает новый коммит с
  двумя родителями, оставляя нелинейную историю, то перебазирование применяет
  все коммиты один за одним из одной ветки в другую, оставляя за собой
  линейную историю коммитов. По сути это автоматическое выполнение нескольких
  команд <a href='#cherry-pick'>cherry-pick</a> подряд.</p>

  <div class="center"><img src='{{ site.baseurl }}images/rebase.svg.png'></div>

  <p>На схеме выше вы видите как команда берет все коммиты, которые есть в
  ветке <em>topic</em>, но отсутствуют в ветке <em>master</em> (коммиты
  <em>169a6</em> and <em>2c33a</em>), и воспроизводит их в ветке
  <em>master</em>. Затем указатель ветки перемещается на новое место. Следует
  заметить, что старые коммиты будут уничтожены сборщиком мусора, если на них
  уже ничего не будет ссылаться.</p>

  <p>Используйте ключ <code>--onto</code> чтобы ограничить глубину захвата
  объединяемой ветки. На следующей схеме вы можете увидеть как в ветку
  <em>master</em> приходят лишь последние коммиты из текущей ветки, а именно
  коммиты после (но не включая) <em>169a6</em>, т. е. <em>2c33a</em>.</p>

  <div class="center"><img src='{{ site.baseurl }}images/rebase-onto.svg.png'></div>

  <p>Есть также интерактивный режим перебазирования <code>git rebase
  --interactive</code>, с помощью которого вы сможете сделать вещи похитрее
  простого линейного применения коммитов, а именно сбрасывание (dropping),
  изменение порядка (reordering), правка (modifying) и выдавливание
  (squashing) коммитов. Нет наглядной схемы, чтобы показать эти возможности;
  за описанием лучше обратиться к справке по <a
    href='http://www.kernel.org/pub/software/scm/git/docs/git-rebase.html#_interactive_mode'>git-rebase(1)</a>.</p>

  <h2 id="technical-notes">Технические заметки</h2>

  <p>Содержание файлов не хранится в индексе (<em>.git/index</em>) или в
  объектах коммитов. Правильнее было бы сказать, что каждый файл хранится в
  базе данных объектов (<em>.git/objects</em>) в <em>двоичном
  представлении</em>; найти этот файл можно по его SHA-1 хешу. В файле индекса
  записаны имена файлов, их хеши и дополнительная информация. В информации о
  коммитах вы встретите тип данных <em>дерево</em>, для идентификации которого
  также используется SHA-1 хеш. Деревья описывают директории в рабочей
  директории, а также содержат информацию о других деревьях и файлах в
  принадлежащей к обозначенному дереву. Каждый коммит хранит идентификатор
  своего верхнего дерева, которое содержит все файлы и другие деревья,
  измененные в этом коммите.</p>

  <p>Если вы делаете коммит из состояния «оторванной головы» (detached HEAD),
  то на этот коммит будет ссылаться ссылка истории HEAD. Но рано или поздно
  время хранения этой ссылки истечет, и этот коммит будет уничтожен сборщиком
  мусора точно также, как это делается при выполнении команд
  <code>git commit --amend</code> и <code>git rebase</code>.</p>

  <hr>

  <p>Copyright &copy; 2010,
    <a href='mailto:lodatom@gmail.com'>Mark Lodato</a>.
  Russian translation &copy; 2012,
    <a href='mailto:alex@sychev.com'>Alex Sychev</a>.
  </p>

  <p><a rel="license"
    href="http://creativecommons.org/licenses/by-nc-sa/3.0/us/deed.ru"><img
    alt="" src="http://i.creativecommons.org/l/by-nc-sa/3.0/us/80x15.png"></a>
  Это произведение доступно по <a rel="license"
    href="http://creativecommons.org/licenses/by-nc-sa/3.0/us/deed.ru">лицензии
    Creative Commons Attribution-NonCommercial-ShareAlike (Атрибуция —
    Некоммерческое использование — С сохранением условий) 3.0 США</a>.</p>