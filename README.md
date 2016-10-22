                                           DJANGO CODING STYLE

#IMPORTS

future, standard library, third-party libraries, diğer Django bileşenleri, local Django bileşenleri,

try/except'ler önem sıralarına göre aşağıdaki gibi kullanılmalıdır. Mümkün olduğunca alfabetik sıraya

göre yazmaya dikkat etmelisiniz.


# future

    from __future__ import unicode_literals
    

# standard library

    import json
    
    from itertools import chain
    

# third-party

    import bcrypt
    

# Django

    from django.http import Http404
    
    from django.http.response import (
    
        Http404, HttpResponse, HttpResponseNotAllowed, StreamingHttpResponse,
       
        cookie,
       
    )
    

# local Django

    from .models import LogEntry
    

# try/except

    try:
    
        import pytz
        
    except ImportError:
    
        pytz = None
        

    CONSTANT = 'foo'


    class Example(object):
    
       # ...


#TEMPLATE STYLE


Template-lerde, kıvırcık parantez ve etiket içerikleri arasında bir boşluk bırakmalıyız.


#Doğru kullanım:

    {{ foo }}
    

#Yanlış kullanım:

    {{foo}}
    

#VİEW STYLE


View-lerde view fonksiyonunun ilk parametresi request ile çağırılmalıdır.


#Doğru kullanım:

    def my_view(request, foo):
    
        # ...
        

#Yanlış kullanım:

    def my_view(req, foo):
    
       # ...


#MODEL STYLE


Field nameler kullanılırken küçük harf ve '_' kullanmalıyız. Büyük harf kullanmamaya
dikkat etmeliyiz.


#Doğru kullanım:

    class Person(models.Model):
    
        first_name = models.CharField(max_length=20)
        
        last_name = models.CharField(max_length=40)
        

#Yanlış kullanım:

    class Person(models.Model):
    
        FirstName = models.CharField(max_length=20)
        
        Last_Name = models.CharField(max_length=40)
       

Field-ları tanımladıktan sonra bir boşluk bırakıp class Meta sınıfını yazmalıyız.


#Doğru kullanım:

    class Person(models.Model):
    
        first_name = models.CharField(max_length=20)
       
        last_name = models.CharField(max_length=40)


        class Meta:
        
           verbose_name_plural = 'people'
           

#Yanlış kullanım

    class Person(models.Model):
    
        first_name = models.CharField(max_length=20)
        
        last_name = models.CharField(max_length=40)
        
        class Meta:
        
           verbose_name_plural = 'people'
           

#Aşağıdaki kullanım şeklide yanlıştır.

    class Person(models.Model):
    
        class Meta:
        
           verbose_name_plural = 'people'
           

       first_name = models.CharField(max_length=20)
       
       last_name = models.CharField(max_length=40)
       

Eğer choices belirli bir model alanı için tanımlanmışsa, model üzerinde bir sınıf
özelliği olarak büyük harfle tanımlayıp choice içinde tuple olarak açıklayabiliriz.


    class MyModel(models.Model):
    
        DIRECTION_UP = 'U'
        
        DIRECTION_DOWN = 'D'
        
        DIRECTION_CHOICES = (
        
             (DIRECTION_UP, 'Up'),
             
             (DIRECTION_DOWN, 'Down'),
             
         )
         

#django.conf.settings Kullanımı


Modülleri django.conf.settings-in üstüne yazmamalıyız.


Ayarlarımızı aşağıdaki gibi elle yapabiliriz.

    from django.conf import settings

    settings.configure({}, SOME_SETTING='foo')
    

Eğer bir ayara settings.conf satırından önce erişilirse bu ayar işe yaramaz.

    from django.conf import settings
    
    from django.urls import get_callable
    

    default_foo_view = get_callable(settings.FOO_VIEW)
    

Yukarıdaki karmaşıklığı önlemek için django.utils.functional.LazyObject,
django.utils.functional.lazy() ya da lambda kullanmalıyız.
