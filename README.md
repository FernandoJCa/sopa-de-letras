# Sopa de Letras (Word Search)

Este proyecto es una implementación de una sopa de letras interactiva utilizando Vue.js. Permite a los usuarios ingresar 5 palabras y generar un puzzle de 15x15 para jugar.

## Características

- Interfaz de usuario para ingresar 5 palabras
- Generación automática de una sopa de letras de 15x15
- Palabras colocadas en cualquier dirección (horizontal, vertical, diagonal, hacia adelante o hacia atrás)
- Selección interactiva de palabras en la cuadrícula
- Verificación de palabras encontradas
- Contenedorización con Docker para fácil despliegue

## Configuración del Proyecto

1. Clonar el repositorio:
   ```
   git clone <url-del-repositorio>
   cd sopa-de-letras
   ```

## Ejecutar el Proyecto

### Usando Docker Compose

Para iniciar la aplicación usando Docker Compose:

```
docker-compose up
```

## Uso

1. Al iniciar la aplicación, verás un campo de entrada para ingresar palabras.
2. Ingresa 5 palabras, una a la vez, presionando Enter o haciendo clic en "Agregar palabra" después de cada una.
3. Una vez ingresadas las 5 palabras, haz clic en "Generar sopa de letras".
4. Busca las palabras en la cuadrícula generada y selecciónalas haciendo clic en las letras.
5. Haz clic en "Verificar palabra" para comprobar si has encontrado una palabra correcta.
6. ¡Continúa hasta encontrar todas las palabras!

## Desarrollo

El componente principal `WordSearch.vue` contiene la lógica para:
- Manejar la entrada de palabras
- Generar la cuadrícula de la sopa de letras
- Colocar las palabras en la cuadrícula
- Manejar la selección de palabras por parte del usuario
- Verificar las palabras encontradas