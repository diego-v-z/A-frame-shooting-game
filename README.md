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
Para más detalles del uso de este componente, visitar el [siguiente enlace.](https://www.npmjs.com/package/aframe-environment-component)

- Finalmente, para la configuración de la función 'Shooting' se utilizó la librería de [CodeChangers.](https://codechangers.com/lessons/vr/adding_shooting/), con el fin de ahorrarnos tiempo en la elaboración del puntero, la cámara y las variables predeterminadas ('myScene', 'myCamera', etc.).

**2. ELABORACIÓN DE LA COLISIÓN ENTRE BALA Y EL OBJETO**

- Lo primero fue configurar la colisión entre bala y objeto, es decir, que en nuestro juego se reconozca cuándo la bala impacta al objeto, cuando ambos objetos interactúan. Lo primero, entonces, será elaborar una función para eso:
```
const shoot = () => {
  const bullet = document.createElement("a-sphere"); //Creamos la bala.
  let pos = myCamera.getAttribute("position"); //Asociamos la posición de la bala a nuestra cámara.
  bullet.setAttribute("position", pos);
  bullet.setAttribute("velocity", getDirection(myCamera, 30));//Definimos la velocidad de la bala y desde dónde parte, en este caso desde la cámara.
  bullet.setAttribute("dynamic-body", true);
  bullet.setAttribute("radius", 0.5);
  bullet.setAttribute("src", "https://i.imgur.com/H8e3Vnu.png");
  myScene.appendChild(bullet);
  bullet.addEventListener('collide', shootCollided); /**Aquí utilizamos los conceptos de 'Events' y 'Event Listeners' de A-frame. En términos simples definimos un evento("collide") + una función que llamaremos para ejecutar dicho evento ("shootCollided").**/
};
```
- Ahora definiremos la función "shootCollided":
```
const shootCollided = event => {
  console.log("Hit something...");
}
```
Esta es una funcióna a modo de prueba para ver si se ejecuta correctamente en la consola. Nuestra función responde correctamente, así que ahora la definimos:

```
const shootCollided = event => {
  if (event.detail.body.el.id === 'floor') {
    console.log("Dispara al suelo!");
    event.detail.target.el.removeEventListener('collide', shootCollided);
    myScene.removeChild(event.detail.target.el);
  }
}
```
Lo que hicimos arriba fue definir qué sucederá si la bala colisiona contra nuestro "suelo". Esto se hace por dos motivos: 1)porque nuestro objetivo no es el suelo, sino el objeto geométrico, así que la bala simplemente desaparecerá si llega al suelo; 2) Delimitamos la cantidad de balas que van saliendo. Removemos tanto la acción como el elemento 'bala', evitando que nuestro juego VR no se ralentice.
