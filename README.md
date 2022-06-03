# A-frame-shooting-game
## Un juego the shooter efectuado con A-frame .

- Este juego de tipo shooting consiste en una interfaz sencilla en que se utiliza un puntero para disparar a un objetivo ("target").
A medida que se apunta al objetivo, van surgiendo más niveles. A continuación se detallarán los pasos efectuados para crear el juego.

**1. MONTAJE DE LA ESCENA Y LIBRERÍA PARA LA ACCIÓN "SHOOT"**

- Para el montaje de la escena se utilizó una plantilla HTML de A-frame obtenida del [Sitio Oficial](https://aframe.io/docs/1.1.0/introduction/). En ella van todas las primitivas y componentes que se usarán en la interacción del juego.
- Para el bosque 3D se utilizó un componente de [A-frame Registry](https://aframe.io/aframe-registry), el cual se inserta en nuestro `<head>`:
```
  <script src="https://unpkg.com/aframe-environment-component@1.2.0/dist/aframe-environment-component.min.js"></script>
```
- Para más detalles del uso de este componente, visitar el [siguiente enlace.](https://www.npmjs.com/package/aframe-environment-component)

- Finalmente, para la configuración de la función 'Shooting' se utilizó la librería de [CodeChangers.](https://codechangers.com/lessons/vr/adding_shooting/)

**2. ELABORACIÓN DE LA COLISIÓN ENTRE BALA Y EL OBJETO**

- Lo primero fue configurar la colisión entre bala y objeto, es decir, que en nuestro juego se reconozca 
