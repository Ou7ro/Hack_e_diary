# Взлом школьного дневника

В данной сборке скриптов, представленно 3 скрипта, что отправляет запросы в базу данных электроного журнала и изменяет его. Используя их, вы можете исправить все плохие оценки, убрать замечания и добавить похвал.

## Зависимости

- Какую версию python использовать, зависит от используемой вами версии [Djangо](https://docs.djangoproject.com/en/4.0/faq/install/#what-python-version-can-i-use-with-django)

- При разработке был использован Python3.10, то есть для использования он точно подойдет.

## Скрипты и их запуск
### Запуск через import

Для данного метода требуется:

- Скачать файл `scripts.py` и поместить его в папке, где находится `manage.py`.

- 

### Запуск через shell

Для запуска через shell, потребуется:

- Запустить `shell`, командой:
```
python manage.py shell
```
- Вставить скопированный текст из файла `scripts.py`.

- После чего вы можете использовать ниже описанные скрипты.

#### fix_marks

В качестве аргумента, принимат ФИО ученика.

В результате его работы, все плохие оценки (2 и 3) изменят свое значение на 5-ки.

Для работы, требуется прописать в `shell`:
```Python
fix_marks('Иванов Иван Иванович')
```

### 



Сам запуск будем производить путем копирования одних из скриптов в зависимости от цели. 
Необходимы будут только корректировки в наименовании ученика.



***
3. Полное удаление замечаний на ученика.
```Python
from datacenter.models import Schoolkid, Mark, Chastisement, Lesson, Commendation
def remove_chastisements(schoolkid_name):
    schoolkid = checks_pupil(schoolkid_name)
    if schoolkid:
        chastisement = Chastisement.objects.filter(schoolkid=schoolkid)
        chastisement.delete()
```
    Запускается посредством копирования в командную строку `shell` и запускается: 
```
remove_chastisements("ФИО школьника")
```
***
4. Данный скрипт отмечает похвалу в базе данных ученика.
```Python
import random
from datacenter.models import Schoolkid, Mark, Chastisement, Lesson
def create_commendation(schoolkid_name, subject):
    schoolkid = checks_pupil(schoolkid_name)
    if schoolkid:
        lesson = Lesson.objects.filter(subject__title=subject, year_of_study=schoolkid.year_of_study,
                                       group_letter=schoolkid.group_letter).order_by("subject").first()
        if lesson is None:
            print('Данного урока не найдено')
        else:
            random_commendations = random.choice(COMMENDATIONS)
            Commendation.objects.create(text=random_commendations, created=lesson.date, schoolkid=schoolkid,
                                        subject=lesson.subject, teacher=lesson.teacher)
```
    Запускается посредством копирования в командную строку `shell` и запускается: 
```
create_commendation("ФИО школьника", "Предмет")
```