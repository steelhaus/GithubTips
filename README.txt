# GithubTips
Заметки для себя. Кривые, только начинаю работать с гитом :-)

//—ОСНОВНЫЕ ИНСТРУМЕНТЫ, НЕ ОТНОСЯЩИЕСЯ НЕПОСРЕДСТВЕННО К ГИТУ
cd <directory> - переместиться к соответствующей директории
ls <directory> - вывести содержимое выбранной директории
	ls - вывести содержимое текущей директории
		-C - выводит кэшированные файлы (нужно, если исследуем .git)
touch <file_name> - создать файл
cat <file_name> - вывести содержимое файла
vi <file_name> - редактор изменения содержимого
	внутри если нажать ‘i’, то можно будет редактировать файл в обычном режиме.
	выйти из редактирования в обычном режиме - ESC
	сохранить и выйти из редактора vi - ввести ‘:wq’, нажать ENTER
clip < <file_name> - скопировать содержимое файла в буфер обмена 

//— НАСТРОЙКА ГИТА
добавить ssh ключ - https://help.github.com/articles/generating-ssh-keys/#platform-windows
убрать проверку символов переносов строк (актуально для windows)
	git config —global core.autocrlf false 
	git config — global core.safecrlf false 
добавить псевдоним - git config —global alias.<alias_name> <alias_value>
Примеры:
	git config —global alias.co checkout - вместо «checkout» теперь можно писать «co» 
	git config —global alias.hist 'log —pretty=format:»%h %ad | %s%d [%an]» —graph —date=short’ - команда «hist» теперь выводит историю в красивом формате

//— ОСНОВНЫЕ КОМАНДЫ ГИТА
git add <file_name> - индексирует выбранный файл. 
	—all - важно писать, когда некоторые файлы были удалены
git add . - индексирует все файлы в данной категории
git commit - коммитит текущую версию в историю версий. После нажатия enter откроет редактор vim и предложит ввести комментарий коммита
	-m ‘<comment>’ - сразу добавляет комментарий без перехода в vim
	—amend - переписывает последний коммит
git reset - отменяет текущее индексирование (git add) файлов
git status - показывает текущий статус
git mv <filename> <directory> - перемещает выбранный файл в выбранную директорию
git rm <filename> - удаляет выбранный файл из репозитория

//— ИСТОРИЯ КОММИТОВ (ЛОГИРОВАНИЕ)
git log - показывает всю историю коммитов
	git log —pretty=oneline (в одну строку)
		—max-count=2 (указывается максимальное количество последних коммитов)
		—since=‘5 minutes ago’ (с какого времени)
		—until=‘5 minutes ago’ (по какое время)
		—author=<your name> (выбрать только коммиты указанного автора)
		—all (все коммиты (включая все ветви и слияния) )
Примеры форматирования git log:
	git log —all —pretty=format:"%h %cd %s (%an)"  —since=‘7 days ago’
	git log —pretty=format:»%h %ad | %s%d [%an]» —graph —date=short
		%h - hash
		%ad - added date
		%s - comment
		%d - comment decorations
		%an = author name
		—graph - выводит дерево коммитов в формате символов ASCII (нужно, если были слияния)
		—date=short - короткий формат даты
gitk - просмотр истории коммитов в GUI

//—ТЕГИ И ОТКАТЫ
git tag <tag_name> - устанавливает тег текущему коммиту
git tag v1 - устанавливает текущему коммиту тег с названием «v1». Теперь можно обращаться к нему через v1, а не через хэш
	—d - удаляет тег
	git tag —d v1 - удаляет тег v1
git tag - просмотр всех тегов
git hist master —all - показывает теги в истории (hist - это псевдоним log-а с форматированием. Указан в пункте «настройки гита»)
cat .git/refs/tags/<tag name> - выведет хэш коммита с данным тегом

git checkout <hash/tag_name> - возврат к коммиту с указанным хэшем/тегом
	git checkout v1 - возврат к коммиту с тегом «v1»
	git checkout v1^ - возврат к коммиту, который шел перед коммитом с тегом «v1»
		^ - родитель в ветке
git checkout master - возврат к последней версии в ветке master

git revert <hash> - отмена коммита
git revert HEAD - отмена текущего коммита (на котором находимся в текущий момент)

git reset —hard <hash/tag_name> - полностью удаляет сведения о выбранном коммите
	—hard - рабочий каталог обновляется в соответствии с новым HEAD

//—ВЕТВИ
git branch <branch_name> - создать новую ветвь
git checkout <branch_name> - переключиться на ветвь
Предыдущие две команды можно заменить одной: 
git checkout -b <branch_name> - создает и переключается на ветвь
git merge <branch_name> - слить текущую ветвь с выбранной ветвью
git rebase <branch_name> - перебазирует текущую ветвь с выбранной ветвью. Похоже на слияние, но дерево ветви имеет иную структуру
cat .git/head - содержит ссылку на текущую ветку
git cat-file -p <hash> - выведет данные об объекте

//—УДАЛЕННЫЙ РЕПОЗИТОРИЙ
git clone <repository_to_clone> <cloned_repository> - клонирование репозитория
	—bare - клонирует чистый репозиторий (без рабочей директории, только сведения)
git remote - показывает удаленный репозиторий клонированного репозитория
	обычно покажет, что он называется origin
git remote show origin - выведет все сведения об оригинальном репозитории, от которого клонировали
git branch - вывести все ветки
	—a - выводит еще и ветки origin

git fetch - извлекает все коммиты из удаленного репозитория, но не сливает их с локальным
git merge origin/master - так же слияет удаленный репо с текущим
	git pull - заменяет обе команды fetch и merge.
git push origin master - передает все коммиты из текущего репозитория в origin репозиторий и слияет их.