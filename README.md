# polymer-card-test

bases de components
1 -> template nativo, mas rapido que jade por que no se interpreta
2 -> Shadow dom igual que en react, se hacen ahi las modificaciones antes
3 -> custom elements, lo que nos permite crear las etiquetas. Document.createElement para crear tags
4 -> html imports, nos permite importar piezas creadas en punto 3
 
Aún no está soportado en todos los navegadores al 100%. Safari está un poco ahí esperando

Problemas que resuelve
 - reutilización de código
 - ejemplos de reutilización, todo tira con js
 - los web componetnes nos permiten hacerlo en el lado de los navegadores sin tener que usar JS para reutilizar código, con menos código a simple vista, una sintaxis más declarativa.

especificaciones: 
  Custom elements
    Elementos declarativos, más mantenibles en html

  Templates
    Estando del lado del navegador, se parsean pero no se renderizan hasta que se instancian

  Shadow Dom
    Video ya usa shadow Dom, donde se ve todo delcarado y como funciona

  Imports
    Se puede definir un import para combinar todas las dependencias, por ejemplo bootstrap y que nos cargue todas las librerías que dependen de este


Uno por uno

  Web components.

    1 - declararlo con document.registerElement. Se registra en el dom pero no tiene funcionalidad. Es obligatorio al menos tener un guión en los componentes

    2 - Con Object.create(HTMLElement.prototype) creamos el objecto html para añadirle propiedades

    3 - Definimos Ciclo de vida: 
       1 - CreatedCallback - al registrarse el elemento
       2 - AttachedCallback - cuando el elemento se añade al dom
       3 - detachedCallback - cuando el elemento desaparece del dom
       4 - x - cuando los atributos cambian de valor se ejecuta esa función

    4 - Definimos las propertys con Object.defineProperty(element, nombre de la propiedad), lo mismo con funciones: elemento.funcionPrueba = function()

    5 - se puede usar el atributo is para definir que tipo de custom element es un tag nativo <button is="sngular-button">
      - el is se utiliza para custom elements, lo otro lo usas cuando es tuyo
      - quizás es mejor usar un is si quieres dejar claro el "role" del elemento.
      - después se puede usar la propiedad extend en el registerElement { extends: 'button' }. extendemos todo, es genial para la accesibilidad!!!! 

  Templates
    sencillo, utilizar <template> <p>template</p> </template> - estan en el dom pero no se renderizan hasta que se clonan 
    - con la template se clona: document.querySelector(template).content.cloneNode(true);

  Shadow-dom
    querySelector para coger un elemento.
    creamos el shadowDom con element.createShadowRoot(); (Geeks say: creado bajo la sombra)
    una cosa interesante es que, si tu encapsulas los estilos dentro del shadow Dom, sería un problema para reutilizarlo con otros estilos. Se puede usar una sintaxis como @apply, o var(--nombre-var, red), y definir cosas fueras del componente para que reemplaze las propiedades del css.

  Imports
    <link rel="import" href="sngular-date.html"> Nos sirve para importar componentes ya declarados y reutilizarlos en todas partes



POLYMER

  dom-module con un id, y la sintaxis para crear cel componente
  <dom-module id="id-de-elemento">
    <script>
      Polymer({ is: 'id-de-elemento'})
    </script>
  </dom-module>

  para temas de espera y sincronía HTMLImports.whenReady(function() {
    polymer...
  })

  definimos las funciones de los webComponentes con wrappers de polymer( created, attached, detached)
  En polymer se utiliza la función ready para asegurarse de que el local Dom y todas las properties creadas ya están ok
  
  definimos las propiedades dentro de un objeto properties: {}
  dentro de property podemos definir watchers con "observer" y el nombre de la función en string, útil para los cambios de valor realizados en el navegador.
  la definición del tipo solo hace que se serialize a ese tipo - number parseInt o lo que sea que haga
  Mete el shadowDom en el template automáticamente
  En funciones, se puede utilizar el selector this.$.data (siendo data el id) para acceder al elemento html

  para comunicarse, puedes usar this.fire('nombre-del-evento') se recoge el evento desde un eventListener. 
  con {{}} es two-way data binding
  hay una propiedad notify(boolean) que se puede definir dentro de cada property para temas de comunicación con el padre

  content select=".clase" para insertar el contenido exactamente en los sitios que quieras, mu interesante


  polyserve sirve para servir componentes por url en vez de tener que ira bower_componentes
  polybuild
  web-component-tester
