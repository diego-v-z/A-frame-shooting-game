# A-frame juego de realidad virtual.
## Un juego the shooter efectuado con A-frame .

- Este juego de tipo shooting consiste en una interfaz sencilla en que se utiliza un puntero para disparar a un objetivo ("target").
A medida que se apunta al objetivo, van surgiendo más niveles. A continuación se detallarán los pasos efectuados para crear el juego.

**1. MONTAJE DE LA ESCENA Y LIBRERÍA PARA LA ACCIÓN "SHOOT"**

- Para el montaje de la escena se utilizó una plantilla HTML de A-frame obtenida del [Sitio Oficial](https://aframe.io/docs/1.1.0/introduction/). En ella van todas las primitivas y componentes que se usarán en la interacción del juego.
- Para el bosque 3D se utilizó un componente de [A-frame Registry](https://aframe.io/aframe-registry), el cual se inserta en nuestro `<head>`:
```JavaScript
  <script src="https://unpkg.com/aframe-environment-component@1.2.0/dist/aframe-environment-component.min.js"></script>
```
Para más detalles del uso de este componente, visitar el [siguiente enlace.](https://www.npmjs.com/package/aframe-environment-component)

- Finalmente, para la configuración de la función 'Shooting' se utilizó la librería de [CodeChangers.](https://codechangers.com/lessons/vr/adding_shooting/), con el fin de ahorrarnos tiempo en la elaboración del puntero, la cámara y las variables predeterminadas ('myScene', 'myCamera', etc.).

**2. Configuración de tecla para ejecutar "Shoot"**

- Para que podamos disparar la bala, efectuaremos una función para configurar una tecla. Nos basaremos en la lista `KeyboardEvent.KeyCode` de la [API de MDN Web Docs](https://developer.mozilla.org/en-US/docs/Web/API/KeyboardEvent/keyCode). La función será la siguiente:
```JavaScript
document.onkeydown = event => {
  if (event.which == 32) {
    shoot();
  } else if (event.which == 67)
};
```

**2. ELABORACIÓN DE LA COLISIÓN ENTRE BALA Y EL OBJETO**

- Lo primero fue configurar la colisión entre bala y objeto, es decir, que en nuestro juego se reconozca cuándo la bala impacta al objeto, cuando ambos objetos interactúan. Lo primero, entonces, será elaborar una función para eso:
```JavaScript
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
```JavaScript
const shootCollided = event => {
  console.log("Hit something...");
}
```
Esta es una funcióna a modo de prueba para ver si se ejecuta correctamente en la consola. Nuestra función responde correctamente, así que ahora la definimos:

**a)** Lo primero es desginar qué sucederá si la bala colisiona contra nuestro "suelo". Esto se hace por dos motivos: 1)porque nuestro objetivo no es el suelo, sino el objeto geométrico; 2) Las balas desaparecen y no se irán "al infinito", lo que ayudará a que nuestro juego no se ralentice. Removemos tanto la acción como el elemento 'bala', evitando que nuestro juego VR no se ralentice. Nuestra función entonces, en primera instancia, quedará así:
```JavaScript
const shootCollided = event => {
  if (event.detail.body.el.id === 'floor') {
    console.log("Dispara al suelo!");
    event.detail.target.el.removeEventListener('collide', shootCollided);
    myScene.removeChild(event.detail.target.el);
  }
}
```
**b)** Ahora efectuaremos una detección para el obejtivo que nos interesa dentro de la misma función:
```JavaScript
else if (event.detail.body.el.className === 'target') {
    console.log("Dispara al obejtivo!");
```
Con lo cual el 'collision detection' para el target principal está listo.

**3. Configuración de los siguientes niveles y objetivos.**

- Ahora queremos que el objetivo y la bala desaparezcan una vez disparemos. para eso basta con añadir a nuestra función lo siguiente:
```JavaScript
    event.detail.target.el.removeEventListener('collide', shootCollided); //Para quitar el EventListener.
    myScene.removeChild(event.detail.target.el); //Para remover el objetivo.
    myScene.removeChild(event.detail.body.el); //Para remover la bala.
```
- Luego, que comienzen a aparecer los nuevos objetivos que representarán los siguientes niveles. Para eso, en primer lugar, usaremos `document.querySelectorAll`. Este selector Query nos permitirá usar el selector de nuestro elemento en el HTML, como si se tratara de un selector CSS:
```JavaScript
//'.length === 0' será el selector Query a utilizar para definir cuando destruyamos todos los objetivos y ganemos la partida para pasar al siguiente nivel.
if (document.querySelectorAll('.target').length === 0) { 
    console.log("Has ganado!");
```
- Para la elaboración de los siguientes niveles simplemente se efectuaron más copias de los objetos geométricos y se crearon otras hojas HTML. Así, al completar un nivel (cuando se hayan destruido todos los objetivos), se pasa al siguiente nivel. Esto se logra de la siguiente manera:

**a)** Se agrega la página del siguiente nivel al final de nuestro HTML. Para el primer nivel, por ejemplo, añadiremos `<script>nextLevel = 'level2.html';</script>` en index.html. Esto se debe repetir para las hojas que iremos añadiendo (level2.html, level3.html, etc.).

**b)** Se agrega `let nextLevel = 'index.html';` al inicio de nuestro `script.js`. Esta variable permitirá ejecutar el siguiente nivel una vez se complete. `index.html` es el primer nivel por defecto. 
