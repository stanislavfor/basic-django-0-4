![](assets/_logo_django_.jpg)
# Фреймворк Django (семинары)
# Урок 4. Работа с формами
## Описание
На этом семинаре мы:
- научимся создавать классы форм;
- изучим поля и виджеты форм;
- узнаем об обработке данных из формы;
- разберёмся в сохранении файлов.

<br><hr>
## Домашнее задание
Уважаемые студенты! Обращаем ваше внимание, что сдавать домашнее задание необходимо через Git. 

Задание: <br>
- Измените модель продукта, добавьте поле для хранения фотографии продукта.
- Создайте форму, которая позволит сохранять фото.
<br><hr>
## Решение задания

<br><br>
**Для запуска проекта**:
- Скачать архив с проектом;
- Перейти в директорию проекта 'shop_project';
- Запустить команду ```python manage.py runserver```;
- Открыть в браузере страницу с адресом http://127.0.0.1:8000/;
- По окончанию работы с проектом, отключить комбинацией клавиш 'Ctrl+C'.


<br><br>
### 1. Действия для запуска проекта (начало)

- Перейти в папку проекта и активировать виртуальное окружение:

```bash
   cd E:\shop_project
   myvenv\Scripts\activate.bat
```

### 2. Добавление возможность загрузки фотографий в базу данных

Чтобы добавлять фотографии продуктов, необходимо выполнить изменения в коде проекта: <br>
- Добавить поле для хранения фотографии продукта в модель Product в models.py:
```
  photo = models.ImageField(upload_to='product_photos/', null=True, blank=True) 
```
- Обновить форму ProductForm в forms.py, чтобы она включала поле для загрузки фотографии 'photo'.
- Обновить шаблон add_product.html, чтобы он включал поле для загрузки фотографии:
```
  <form class="form bottom" action="" method="post" enctype="multipart/form-data">
    {% csrf_token %}
    {{ form.as_p }}
    <input class="button" type="submit" value="Добавить продукт" />
  </form>
```
- Обновить представление add_product в views.py, добавить запись 'request.FILES', <br>
  чтобы представление add_product корректно обрабатывало загрузку фотографии:
```
  form = ProductForm(request.POST, request.FILES)
```
- Обновить настройки MEDIA_URL и MEDIA_ROOT в settings.py, <br>
  чтобы настройки MEDIA_URL и MEDIA_ROOT в файле settings.py работали для media ресурса:
  ```
     MEDIA_URL = '/media/'
     MEDIA_ROOT = os.path.join(BASE_DIR, 'media')
  ```
- Обновить URL-маршруты в urls.py для обработки медиа-файлов в файл urls.py:
  ```
    urlpatterns = [
    # все другие URL-маршруты проекта
    ] + static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)
  ```

### 3. Создать миграции

- Создать и запустить миграцию для обновления структуры базы данных, <br> 
  чтобы убедиться, что база данных синхронизирована с последней версией:

```sh
python manage.py makemigrations
```
 
### 4. Применить миграции

Применить миграции для обновления структуры базы данных.

```sh
python manage.py migrate
```

Теперь должен появиться столбец `photo` в таблице `shop_product`,
и можно загружать фотографии продуктов в базу данных.

### 5. Установить библиотеку Pillow

Потребуется установить библиотеку Pillow:
```sh
python -m pip install Pillow
```

### 6. Запустить сервер разработки

Запустить сервер разработки, чтобы проверить изменения.
- После внесения изменений перезапустить сервер, выполнив команду:
```sh
python manage.py runserver
```

- Так как, по умолчанию Django стартует на порту `8000`, следует открыть браузер<br> и перейти по адресу: [http://127.0.0.1:8000/](http://127.0.0.1:8000/).

### 7. Маршрутизация в браузере

- Главная страница проекта http://127.0.0.1:8000/
- Список всех клиентов http://127.0.0.1:8000/clients/
- Список всех продуктов http://127.0.0.1:8000/products/
- Список всех заказов http://127.0.0.1:8000/orders/
<br><br>
- Информация о конкретном клиенте http://127.0.0.1:8000/clients/1/, где ```http://127.0.0.1:8000/clients/{ID_клиента}/```
- Информация о конкретном товаре http://127.0.0.1:8000/products/1/, где ```http://127.0.0.1:8000/products/{ID_товара}/```
- Информация о конкретном заказе http://127.0.0.1:8000/orders/1/, где ```http://127.0.0.1:8000/orders/{ID_заказа}/```
<br><br>
- Страница редактирования конкретного заказа http://127.0.0.1:8000/edit-order/1/, где ```http://127.0.0.1:8000/edit-order/{ID_заказа}/```
- Страница добавления нового клиента http://127.0.0.1:8000/add-client/
- Страница добавления нового товара http://localhost:8000/add-product/
- Страница добавления нового заказа http://localhost:8000/add-order/
- Страница редактирования профиля http://localhost:8000/update-profile/
<br><br>
- Список всех заказов клиента http://127.0.0.1:8000/clients-orders/
- Список всех личных заказов http://127.0.0.1:8000/client-orders/

### 8. Остановка сервера

После окончания работы с сервером Django требуется остановка сервера или деактивация, выход из локального окружения. <br>
- Всегда останавливать сервер перед переходом к другим действиям.
- Использовать команду `deactivate` только после того, как вышли из запущенного сервера Django.
- Новый проект можно создавать сразу после освобождения консоли путём остановки текущего сервера.
- Если сервер не остановить, консоль останется заблокированной процессом Django, и никакие новые команды не смогут быть выполнены.
- Деактивация виртуального окружения возможна только тогда, когда оно было предварительно активировано.

Сервер Django остановится автоматически, если отправить сигнал прерывания процесса. Для этого можно воспользоваться комбинацией клавиш: <br>
- В Windows или Linux: **Ctrl+C**
- В macOS: **Command + C**

После нажатия комбинации клавиш сервер завершит свою работу, и можно вводить команды в консоли.
Ввести команду деактивации, которая вернет обратно в глобальную среду системы разработки:
```bash
myvenv\Scripts\deactivate.bat
```

<br><hr>
## Дополнительная информация

<br><br>
### Создание классов форм
Формы в Django создаются путем наследования от класса `forms.Form` или `forms.ModelForm`. <br>

Например, простой формы регистрации пользователя:

```
from django import forms

class RegistrationForm(forms.Form):
    username = forms.CharField(max_length=100)
    email = forms.EmailField()
    password = forms.CharField(widget=forms.PasswordInput())
```

, где класс формы с тремя полями: имя пользователя (`username`), электронная почта (`email`) и пароль (`password`). <br> 
Поле пароля специально отображается скрытым видом благодаря использованию специального виджета `PasswordInput`.


<br><br>
### Поля и виджеты форм
Django предлагает большое количество встроенных полей и виджетов, каждый из которых служит определенной цели. <br><br>

**Список наиболее часто используемых типов полей и виджетов**:

##### Поля форм:
- `CharField`- строковые данные.
- `EmailField`- проверка формата электронной почты.
- `IntegerField`, `FloatField`- числовые поля.
- `BooleanField`- булевые значения («галочка»).
- `DateField`, `TimeField`, `DateTimeField`- ввод дат и времени.
- `FileField`, `ImageField`- загрузка файлов и изображений.

##### Виджеты:
Виджет - это визуальное представление поля ввода в браузере. Основные виджеты включают:
- `TextInput`- стандартный однострочный текстовый ввод.
- `Textarea`- многострочное текстовое поле.
- `CheckboxInput`- чекбокс (галочка).
- `Select`- выпадающий список выбора.
- `RadioSelect`- набор радиокнопок.
- `PasswordInput`- специальный виджет для скрытия введенного текста.

Пример задания виджета вручную:
```
class MyForm(forms.Form):
    message = forms.CharField(
        widget=forms.Textarea(attrs={'rows': 5, 'cols': 40})
    )
```

<br><br>
### Обработка данных из формы
Для обработки данных из формы используется метод `is_valid()`, который проверяет правильность заполнения полей. Если форма валидная, её данные можно обработать и сохранить.

Пример простой обработки формы в представлении:

```
def register(request):
    if request.method == 'POST':
        form = RegistrationForm(request.POST)
        if form.is_valid():
            # Здесь обрабатываем полученные данные
            print(form.cleaned_data['username'])
            return HttpResponseRedirect('/thanks/')
    else:
        form = RegistrationForm()
    
    return render(request, 'register.html', {'form': form})

```

, где метод `cleaned_data` возвращает очищенные и проверенные данные формы, готовые к обработке.


<br><br>
### Сохранение файлов
При работе с файлами используются специальные классы полей `FileField` и `ImageField`. <br>
Для сохранения загружаемых файлов, нужно включить поддержку загрузки файлов в форме и обработку POST-запросов.

Пример сохранения файла в модели:

```
# models.py
from django.db import models

class Document(models.Model):
    title = models.CharField(max_length=255)
    file = models.FileField(upload_to='uploads/%Y/%m/%d')
```

Затем добавляем обработчик в форму:

```
# forms.py
from .models import Document
from django.forms import ModelForm

class UploadDocumentForm(ModelForm):
    class Meta:
        model = Document
        fields = ['title', 'file']
```

И используем в представлении:

```
def upload_document(request):
    if request.method == 'POST':
        form = UploadDocumentForm(request.POST, request.FILES)
        if form.is_valid():
            form.save()
            return redirect('document_list')
    else:
        form = UploadDocumentForm()
    
    return render(request, 'upload.html', {'form': form})
```

- **Обратить внимание, что важно передавать оба аргумента: `request.POST` и `request.FILES`, иначе файл не сохранится**.


<br><br><hr>
## Инструкция

<br><br>
### Добавление загрузки изображений в проект

1. Обновить модель `Product` в `models.py`

Добавить поле для хранения фотографии продукта в модель `Product`:

```
from django.db import models

class Product(models.Model):
    name = models.CharField(max_length=100)
    description = models.TextField()
    price = models.DecimalField(max_digits=10, decimal_places=2)
    quantity = models.PositiveIntegerField()
    photo = models.ImageField(upload_to='product_photos/', null=True, blank=True)  # Добавьте это поле

    def __str__(self):
        return self.name
```

2. Обновить форму `ProductForm` в `forms.py`

Обновить форму `ProductForm`, чтобы она включала поле для загрузки фотографии 'photo':

```
from django import forms
from .models import Product

class ProductForm(forms.ModelForm):
    class Meta:
        model = Product
        fields = ['name', 'description', 'price', 'quantity', 'photo']
```

3. Обновить представление `add_product` в `views.py`

Проверить, что представление `add_product` корректно обрабатывает загрузку фотографии:

```
from django.shortcuts import render, redirect
from .forms import ProductForm

def add_product(request):
    if request.method == 'POST':
        form = ProductForm(request.POST, request.FILES)
        if form.is_valid():
            form.save()
            return redirect('products_list')  # Перенаправление на страницу со списком товаров
    else:
        form = ProductForm()
    return render(request, 'add_product.html', {'form': form})
```

4. Обновить представление `EditProduct` в `views.py`

Проверить, что представление `EditProduct` корректно обрабатывает загрузку фотографии:

```
from django.views.generic.edit import UpdateView
from django.urls import reverse_lazy
from .models import Product
from .forms import ProductForm

class EditProduct(UpdateView):
    model = Product
    template_name = 'edit_product.html'
    success_url = reverse_lazy('products_list')
    form_class = ProductForm
```

5. Обновить настройки `MEDIA_URL` и `MEDIA_ROOT` в `settings.py`

Проверить, что настройки `MEDIA_URL` и `MEDIA_ROOT` определены в файле `settings.py`:

```
MEDIA_URL = '/media/'
MEDIA_ROOT = os.path.join(BASE_DIR, 'media')
```

6. Обновить URL-маршруты в `urls.py`

Добавить URL-маршрут для обработки медиа-файлов в файл `urls.py`:

```
from django.conf import settings
from django.conf.urls.static import static

urlpatterns = [
    # здесь другие URL-маршруты проекта
] + static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)
```

7. Установить библиотеку Pillow для работы с изображениями в среде Python

Библиотека с открытым исходным кодом Pillow предоставляет возможности для обработки изображений: изменение размера, кадрирование, преобразование в разные форматы и другие, поддерживает широкий диапазон форматов файлов изображений: JPEG, PNG, BMP и TIFF.

Потребуется установить:
```
python -m pip install Pillow
```

8. Запустить сервер разработки

Запустить сервер разработки, чтобы проверить изменения:

```
python manage.py runserver
```

<br><br>
### Добавление фавикона (`favicon`) в проект Django


1. Создать файл фавикона

Создать или скачать иконку размером **16x16** px формата `.ico` (например, `favicon.ico`).

2. Разместить файл в папке статики

Поместить файл `favicon.ico` в директорию проекта внутри каталога со статическими файлами, в каталог `static/`.

3. Настроить пути к статическим файлам

Проверить настройки в файле настроек `settings.py`. Убедится, что путь к статике указан верно:

```
STATIC_URL = '/static/'
```

Также добавить переменную `STATICFILES_DIRS`, если требуется хранить статику отдельно от приложения:

```
import os
BASE_DIR = os.path.dirname(os.path.dirname(os.path.abspath(__file__)))

# Путь к директории с файлом favicon.ico
STATICFILES_DIRS = [
    os.path.join(BASE_DIR, 'static'),
]
```

#### 4. Подключить фавикон в шаблоне HTML

Открыть базовый шаблон `base.html` и добавить ссылку на фавикон в тег `<head>`:

```
<!DOCTYPE html>
<html lang="ru">
<head>
    <!-- Другие метатеги -->
    {% load static %}
    <link rel="shortcut icon" href="{% static 'favicon.ico' %}" type="image/x-icon"/>
</head>
<body>
    <!-- Контент страницы -->
</body>
</html>
```




<br><br><br><br>
<hr><hr><hr><hr>