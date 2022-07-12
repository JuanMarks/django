# Iniciando Com Django

## Criando ambiente virtual


```bash
# Crie a pasta onde voce quer fazer o ambiente virtual depois faça
# Dentro da pasta pelo o bash faça
python3 -m venv ./venv
# Carregando ambiente virtual dentro do vs code com o local da pasta aberto no vs code
source "caminho"/venv/bin/activate
# ou
"caminho"/venv/bin/activate
# ou em windows 
"caminho"/venv/Scripts/activate
# com o ambiente virtual ativado no vs code faça
pip install django
```

## Iniciando Projeto
No vs code no terminal bash com o ambiente virtual ativado faça
```bash
django-admin startproject "nome do projeto"
```

<p>No arquivo settings.py mudar o timezone para America/Fortaleza e a linguagem para pt-br<br></p>

```python
LANGUAGE_CODE = 'pt-br'

TIME_ZONE = 'America/Fortaleza'
```

<p><br>Subindo servidor django sempre verificando se o ambiente virtual esta ativado</p>

```bash
python manage.py runserver
```

Criando app com 
```bash
python manage.py startapp "nome do app"
```
<p>Registre o app no arquivo settings.py na linha 33</p>

```python
INSTALLED_APPS = [
    'nome do app',
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
]
```

<p>Crie um arquivo com nome de urls.py na pasta do app e faça</p>

```python
from path import django.urls
from . import views

urlpatterns = [
path('', views.index, name='index'),
]
```

<p>No arquivo views faça</p>

```python
#EXEMPLO
def index(request):
     	receitas = Receita.objects.all()

     	dados = {
          'receitas' : receitas
     	}
     	return render(request, 'index.html', dados)
```

<p>Adcionar include no arquivo urls.py do projeto </p>

```python
from django.contrib import admin
from django.urls import path, include
#EXEMPLO
urlpatterns = [
    path('', include('receitas.urls')),
    path('admin/', admin.site.urls),
]
```

<p>Criar uma pasta com nome de template no app e criar ou colocar o arquivo index.html nele</p>
<p>Gerenciando arquivos estaticos no django</p>
<p>Adcionar o caminho da templates no arquico settings.py linha 58</p>

```python
'DIRS': [os.path.join(BASE_DIR, 'nome da pasta do app/templates')]
```

<p>Configurando arquivos estaticos</p>

```python
STATIC_ROOT = os.path.join(BASE_DIR, 'static')
STATIC_URL = '/static/'
STATICFILES_DIRS = [
    os.path.join(BASE_DIR, 'nome do projeto/static')
]
```
<p>Na pasta do projeto criar uma pasta com nome de static coloque todos os arquivos sem ser o index ou qualquer outro html na pasta static</p>
<p>No terminal com o ambiente virtual ligado digite</p>

```bash
python manage.py collectstatic
```

<p>Depois coloque os arquivos html dentro da pasta templates</p>
<p>No arquivo html bem no inicio coloque {% load static %} para carregar os arquivos estaticos do arquivo, depois coloque {% static '' %} em tudo que fizer parte da pasta static dentro do html </p>

```html
#EXEMPLO
{% load static %}
 <!-- Preloader -->
    <div id="preloader">
        <i class="circle-preloader"></i>
        <img src="{% static 'img/core-img/pizza.png' %}" alt="">
    </div>
```
<p>Crie outra função no arquivo views.py que renderize outra pagina se precisar</p>

```python
#EXEMPLO
def receita(request):
    return render(request, 'receita.html')
```
<p>Adcionar path da outra pagina no arquivo urls.py</p>

```python
#EXEMPLO
path('', views.receita, name='receita'),
```
<p>Se precisar e se tiver algum codigo repitido entre as paginas fazer um arquivo base html e usar o {% block content%} e {% endblock %}</p>

```html
#EXEMPLO
{% load static %}

<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="description" content="">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">
    <!-- The above 4 meta tags *must* come first in the head; any other head content must come *after* these tags -->

    <!-- Título -->
    <title>Alura Receitas</title>

    <!-- Favicon -->
    <link rel="icon" href="{% static 'img/core-img/favicon.ico' %}">

    <!-- Stylesheet -->
    <link rel="stylesheet" href="{% static 'site.css' %}">

</head>

<body>
    {% block content %} {% endblock %}
</body>
```

<p>E extender onde fica o codigo repetido {% extends 'base.html'%} nao esquecer de botar o block e o endblock no inicio e no final 	da pagina onde voce vai extender o arquivo base</p>

```html
#EXEMPLO
{% extends 'base.html'%}
{% load static %}
{% block content%}
    <!-- Preloader -->
    <div id="preloader">
        <i class="circle-preloader"></i>
        <img src="{% static 'img/core-img/pizza.png' %}" alt="">
    </div>

    <!-- Search Wrapper -->
    <div class="search-wrapper">
        <!-- Close Btn -->
        <div class="close-btn"><i class="fa fa-times" aria-hidden="true"></i></div>

        <div class="container">
            <div class="row">
                <div class="col-12">
                    <form action="#" method="post">
                        <input type="search" name="search" placeholder="O que está procurando...">
                        <button type="submit"><i class="fa fa-search" aria-hidden="true"></i></button>
                    </form>
                </div>
            </div>
        </div>
    </div>
{% endblock %}
```
