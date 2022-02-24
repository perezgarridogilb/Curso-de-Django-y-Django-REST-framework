# Agenda
Django REST Framework Section

# REST services
Es como el puente para que se comuniquen dos software completamente independientes.

- Principalmente trabajan con archivos de formato JSON que luego va a tomar una aplicación para interpretarla, para transformarla en Objetos o Clases, que luego van a procesar para interactuar con nuestra Base de Datos.

Clave - valor
```
{
  "departamento":8,
  "nombredepto":"Ventas",
  "director": "Juan Rodríguez",
  "empleados":[
    {
      "nombre":"Pedro",
      "apellido":"Fernández"
    },{
      "nombre":"Jacinto",
      "apellido":"Benavente"
    } 
  ]
}
```
# REST Architecture
Es una interfaz para conectar sistemas basados en el protocolo HTTP
Recuperar información sobre el recurso

## Methods
GET: Recuperar información sobre el recurso API REST
POST: Crear un recurso de API REST
PUT: Actualizar un recurso de API REST
DELETE: Eliminar un recurso de la API REST o un componente relacionado

## Client - Server
Este nos obliga a tener separado lo que es del cliente y el servidor
- Todo proceso que interactúa con los datos tiene que ser un proceso exclusivamente del servidor.
- Toda la interfaz grácfica con la que interactúa el usuario es del lado del cliente.
- Pueden haber respuestas sin estado.

# Django REST framework

## Installation
```
pip install djangorestframework
```

## settings.py / base.py
```
THIRD_PARTY_APPS = (
    'rest_framework',
)
```

## Serializers
- Los serializadores en Django REST Framework son responsables de convertir objetos en tipos de datos comprensibles para JavaScript y marcos front-end.

- Convierten los datos de la vista a Objetos de JavaScript / JSON.

### views.py (templates)
```
from django.views.generic import ListView
from .models import Person

class ListaPersonas(ListView):
    template_name = "persona/personas.html"
    context_object_name = 'personas'

    def get_queryset(self):
        return Person.objects.all()
```

### views.py (serializer)
```

from rest_framework.generics import ListAPIView
from .models import Person
from .serializers import PersonSerializer

    
class PersonListApiView(ListAPIView):

    # Detallamos bajo qué serializador queremos que muestre el resultado
    serializer_class = PersonSerializer

    def get_queryset(self):
        return Person.objects.all()

```

### serializer.py
```
from rest_framework import serializers
from .models import Person

# Conectado a modelo (similar a los formularios)
class PersonSerializer(serializers.ModelSerializer):
    class Meta:
        model = Person
        # Específicamos los parámetros que queremos que serialize
        fields = ('__all__')

```

### urls.py
El que activa una vista es una url
```
from django.urls import path, re_path
from . import views

app_name = 'persona_app'

urlpatterns = [
    path(
        'api/persona/lista/',
        views.PersonListApiView.as_view(),
    ),
]
```