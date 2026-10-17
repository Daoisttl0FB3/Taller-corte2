# Taller 2: React

**Estudiante:** Gabriela Morales Cancino.

**Programa:** Ingeniería de Sistemas y Computación.

**Asignatura:** Desarrollo Frontend  
**Docente:** Luis Giovanny Carreño Ortiz  
**Fecha:** 16 de octubre de 2026

---

## Punto 1 · ¿Qué es React?

### ¿Qué es React y para qué sirve dentro del desarrollo web?

React es una biblioteca de JavaScript que sirve para construir interfaces de usuario, es decir, la parte de una aplicación web que la persona ve y con la que interactúa: botones, formularios, listas, menús, etc. Su idea central es que la interfaz se arma a partir de piezas pequeñas e independientes llamadas componentes, y que cada pieza sabe cómo debe verse según los datos que recibe. Sirve para hacer desde páginas sencillas hasta aplicaciones grandes, y también para apps móviles con React Native.

### ¿Quién lo creó y en qué año apareció?

React lo creó Jordan Walke, un ingeniero de Facebook. Empezó como una herramienta interna que se usó en 2011 en el News Feed de Facebook y luego en Instagram. En 2013 se publicó como código abierto, y desde ahí cualquiera pudo usarlo.

### ¿Es una librería o un framework? Explica la diferencia y justifica tu respuesta.

React es una librería, no un framework. Aunque en ocasiones se usan ambos términos como si fueran lo mismo, hay una diferencia y es que una librería ofrece herramientas para resolver una parte específica de un problema, mientras que un framework suele dar una estructura más amplia y establece cómo organizar distintas partes de una aplicación. React se encarga de la interfaz y nada más.

### ¿Qué significa que React sea declarativo?

Significa que uno describe qué quiere ver en pantalla y no cómo lograrlo paso a paso. En vez de decirle al navegador "busca este elemento, cámbiale el texto, agrégale esta clase", uno dice "cuando el contador valga 5, la pantalla muestra un 5", y React se encarga de hacer los cambios necesarios.

```jsx
function Saludo({ nombre }) {
  return <h1>Hola, {nombre}</h1>;
}
```

En este ejemplo, el componente dice que debe aparecer un título con el nombre recibido. Si `nombre` cambia, React se encarga de actualizar la interfaz.

## Punto 2. ¿Qué problema resuelve React?

### ¿Cuál era la dificultad principal de construir interfaces con JavaScript “a mano” (manipulando el DOM)?

El DOM (Document Object Model) es la representación que el navegador construye de los elementos de una página, como títulos, botones, imágenes y campos de texto. Con JavaScript tradicional es posible modificar directamente esos elementos; el problema aparece cuando la interfaz crece y muchas cosas dependen de los mismos datos.

### Explica la diferencia entre el enfoque imperativo y el enfoque declarativo.

En el enfoque imperativo uno da las instrucciones paso a paso. En el declarativo uno describe el resultado final. Un ejemplo con el mismo contador:

```javascript
// Imperativo (JavaScript tradicional)
let cuenta = 0;
const texto = document.getElementById("contador");
const boton = document.getElementById("sumar");

boton.addEventListener("click", () => {
  cuenta = cuenta + 1;
  texto.textContent = "Clics: " + cuenta; // hay que acordarse de actualizar la pantalla
});
```

```jsx
// Declarativo (React)
function Contador() {
  const [cuenta, setCuenta] = useState(0);

  return (
    <button onClick={() => setCuenta(cuenta + 1)}>
      Clics: {cuenta}
    </button>
  );
}
```

En el primero tengo que acordarme de actualizar el texto cada vez que cambia el número. En el segundo solo digo que el botón muestra `cuenta`, y si `cuenta` cambia, la pantalla se actualiza sola.

### ¿Cómo ayuda React a mantener la interfaz sincronizada con los datos (el estado)?

React mantiene la interfaz sincronizada con el estado, cuando el estado cambia, React vuelve a calcular cómo debería verse el componente, lo compara con lo que ya había en pantalla y modifica en el DOM únicamente lo que cambió. Así uno no tiene que preocuparse por qué parte de la pantalla actualizar. Solo se encarga de mantener bien los datos.

### ¿Qué rol juegan los componentes reutilizables en la solución de ese problema?

Los componentes ayudan a dividir una interfaz grande en partes pequeñas con una función concreta. Un botón, una tarjeta o un formulario pueden escribirse una vez y reutilizarse en varias partes de la aplicación con distintos datos. Esto evita repetir el mismo código y permite hacer una modificación en un solo componente para que se refleje en los sitios donde se utiliza.

## Punto 3. Principales componentes de React

### 3.1 Componentes

Un componente es una función de JavaScript que devuelve la parte de interfaz que debe mostrarse en pantalla. Los componentes son la base de React, porque permiten dividir una aplicación en piezas independientes y reutilizables.

**Características principales.**

- Se crean como funciones, y su nombre empieza con mayúscula (así React los distingue de las etiquetas normales de HTML).
- Pueden usarse varias veces y combinarse unos dentro de otros.
- Cada uno puede recibir datos y manejar su propia información.

```jsx
function Tarjeta({ titulo, descripcion }) {
  return (
    <div className="tarjeta">
      <h2>{titulo}</h2>
      <p>{descripcion}</p>
    </div>
  );
}

function App() {
  return (
    <Tarjeta
      titulo="React"
      descripcion="Librería para construir interfaces de usuario."
    />
  );
}
```

El componente `Tarjeta` recibe un título y una descripción, y muestra esa información dentro de una tarjeta. Luego, App lo utiliza pasándole los datos necesarios.

### 3.2 JSX

JSX es una sintaxis que permite escribir algo parecido a HTML dentro del código de JavaScript. No es HTML de verdad porque antes de ejecutarse, una herramienta lo convierte en JavaScript normal. Su ventaja es que la estructura de la interfaz y la lógica quedan juntas en el mismo lugar.

**Características y reglas básicas.**

- Un componente debe devolver un solo elemento raíz. Si hay varios, se envuelven en un `<div>` o en un fragmento (`<>...</>`).
- Todas las etiquetas se cierran, incluso las que en HTML no lo requieren (`<img />`, `<input />`).
- Los atributos van en camelCase y algunos cambian de nombre: `className` en vez de class.
- Para usar JavaScript dentro del JSX se escriben llaves { }.

```jsx
function Perfil() {
  const nombre = "Ana";
  const foto = "https://example.com/ana.jpg";

  return (
    <>
      <img className="avatar" src={foto} alt={"Foto de " + nombre} />
      <p>Hola, {nombre}</p>
    </>
  );
}
```

Aquí se usan llaves para insertar variables, `className` en lugar de `class`, la etiqueta `img` cerrada con `/>` y un fragmento que agrupa los dos elementos.

### 3.3 Props

Las props son los datos que un componente recibe desde afuera. Funcionan como los parámetros de una función y permiten que el mismo componente se vea distinto según lo que le pasemos.

**Características principales.**

- Se envían desde el componente padre, con una sintaxis similar a los atributos de HTML.
- Son de solo lectura: el componente que las recibe no debe modificarlas.
- Pueden ser textos, números, objetos, listas e incluso funciones.

```jsx
function Usuario({ nombre, edad }) {
  return (
    <p>
      {nombre} tiene {edad} años.
    </p>
  );
}

function App() {
  return (
    <>
      <Usuario nombre="Laura" edad={20} />
      <Usuario nombre="Andrés" edad={22} />
    </>
  );
}
```

Aquí `Usuario` es un componente reutilizable. El mismo diseño se usa para mostrar información de Laura y de Andrés, pero cada uno recibe sus propios datos mediante props.

### 3.4. Estado (`useState`)

El estado es información que un componente necesita recordar y que puede cambiar mientras la aplicación está funcionando, como lo que escribe el usuario, un contador o si un menú está abierto. El Hook useState permite crear una variable de estado y una función para actualizarla.

**Características principales.**

- `useState` devuelve dos cosas: el valor actual y una función para actualizarlo.
- Cuando se llama a esa función, React vuelve a ejecutar el componente y la interfaz se actualiza con el nuevo valor.
- No se debe modificar el estado directamente (cuenta = 5 no funciona); siempre se usa la función.

```jsx
import { useState } from 'react';

function Contador() {
  const [contador, setContador] = useState(0);

  return (
    <div>
      <p>Has hecho clic {contador} veces.</p>
      <button onClick={() => setContador(contador + 1)}>
        Aumentar
      </button>
    </div>
  );
}
```

En este ejemplo, `contador` guarda el número actual y `setContador` permite cambiarlo. Cada vez que se presiona el botón, el estado aumenta en uno y React actualiza automáticamente el texto mostrado.

### 3.5. Eventos (`onClick`, `onChange`)

Los eventos permiten que la interfaz responda a las acciones del usuario, como hacer clic, escribir en un campo, enviar un formulario o mover el mouse. En React se escriben en formato camelCase, por ejemplo onClick y onChange, y reciben una función que indica qué hacer cuando ocurre el evento.

**Características principales.**

- Los nombres van en camelCase (onClick, no onclick).
- Se pasa la función, no su resultado: onClick={manejarClic} y no onClick={manejarClic()}.
- Normalmente el evento termina cambiando el estado, y por eso la pantalla se actualiza.

```jsx
import { useState } from 'react';

function Formulario() {
  const [nombre, setNombre] = useState('');

  return (
    <div>
      <input
        type="text"
        value={nombre}
        onChange={(evento) => setNombre(evento.target.value)}
        placeholder="Escribe tu nombre"
      />

      <button onClick={() => alert(`Hola, ${nombre}`)}>
        Saludar
      </button>
    </div>
  );
}
```

En este formulario, onChange se activa cada vez que la persona escribe en el campo de texto. El valor que escribió se guarda en el estado mediante setNombre. Luego, cuando se presiona el botón, onClick ejecuta una función que muestra un saludo con el nombre guardado.

## Punto 4. Importancia de React en la industria

### ¿Por qué tantas empresas eligen React para sus productos web?

React es importante en la industria porque resuelve un problema que sucede mucho y es construir interfaces complejas que deben actualizarse constantemente sin que el código se vuelva difícil de mantener. Permite que equipos grandes trabajen sobre partes separadas de la aplicación, reutilicen código y mantengan una experiencia visual consistente debido a que está enfocado en los componentes.

### ¿Qué aporta React al ecosistema de desarrollo (herramientas que lo rodean: empaquetadores, frameworks basados en React, etc.)?

React no se quedó solo como librería, sino que alrededor de él se construyó un entorno completo de herramientas:

- Empaquetadores y herramientas de desarrollo, como Vite, que sirven para crear y ejecutar un proyecto rápidamente.
- Frameworks basados en React, como Next.js o React Router, que agregan rutas, renderizado en el servidor y otras funciones. De hecho, la documentación oficial de React hoy recomienda empezar proyectos nuevos con alguno de estos frameworks.
- React Native, que lleva las mismas ideas a las aplicaciones móviles, y Expo, que facilita trabajar con él.
- Muchas librerías de componentes, de manejo de estado y de formularios hechas por la comunidad.

### ¿Qué papel juegan su comunidad y su mantenimiento (quién lo mantiene hoy)?

React tiene una comunidad muy grande de desarrolladores, empresas, creadores de contenido y proyectos de código abierto. Esa comunidad produce documentación, librerías, cursos, ejemplos y soluciones a problemas comunes, lo que facilita aprender y encontrar apoyo.

En cuanto al mantenimiento, React fue creado y mantenido por Meta durante muchos años. En febrero de 2026, Meta lanzó oficialmente la React Foundation, dentro de la Linux Foundation, para dar un enfoque más independiente al proyecto. Meta sigue participando con financiamiento e ingenieros dedicados, mientras que la fundación apoya la infraestructura, la comunidad y el ecosistema de React.

### ¿Por qué es una habilidad relevante para un desarrollador front-end hoy en día?

Porque es una de las tecnologías que más piden las empresas para el desarrollo de interfaces, y aprenderlo abre puertas tanto en web como en móvil. Además, sus conceptos (componentes, estado, props) se parecen a los de otras herramientas modernas, así que quien entiende React tiene más facilidad para aprender otras. En resumen, saber React ayuda a conseguir trabajo y también a entender cómo se construye hoy la mayoría del desarrollo front-end.

## Punto 5. Grandes empresas que utilizan React

### 1. Meta / Facebook

Meta es la empresa creadora de React y lo utiliza en sus propios productos, especialmente en Facebook. La interfaz de Facebook maneja grandes cantidades de información que cambian constantemente: publicaciones, comentarios, reacciones, notificaciones y mensajes. React le permite actualizar esas partes de la pantalla sin recargar toda la página.

Es un caso relevante porque Facebook fue el origen de la librería: React nació para resolver los problemas de mantenimiento y rendimiento de interfaces muy grandes. Además, Meta sigue usando React como una de sus herramientas principales para construir interfaces web y también mantiene React Native para el desarrollo móvil.

Fuente: [Engineering at Meta](https://engineering.fb.com/2025/10/07/open-source/introducing-the-react-foundation-the-new-home-for-react-react-native/)

### 2. Discord: aplicación web y cliente de escritorio

Discord confirmó en su blog de ingeniería que sus clientes de escritorio están construidos con React, mientras que sus aplicaciones móviles utilizan React Native. En el caso de escritorio, React permite desarrollar una interfaz con distintas áreas que deben actualizarse continuamente, como canales, conversaciones, mensajes y controles de usuario.

Es un caso interesante porque Discord necesita responder con rapidez a muchas acciones, por ejemplo, cambiar de canal, enviar mensajes o actualizar una conversación. La empresa señala que el uso de React en escritorio y React Native en dispositivos móviles ayuda a sus equipos a desarrollar funciones para distintas plataformas con mayor rapidez, aunque también requiere prestar atención al rendimiento.

Fuente: Discord Engineering. [Supercharging Discord Mobile: Our Journey to a Faster App](https://discord.com/blog/supercharging-discord-mobile-our-journey-to-a-faster-app)

## Fuentes consultadas

1. React. *Documentación oficial*. [https://es.react.dev](https://es.react.dev)
2. React. *Tu primer componente*. [https://es.react.dev/learn/your-first-component](https://es.react.dev/learn/your-first-component)
3. React. *Escribir marcado con JSX*. [https://es.react.dev/learn/writing-markup-with-jsx](https://es.react.dev/learn/writing-markup-with-jsx)
4. React. *Pasando props a un componente*. [https://es.react.dev/learn/passing-props-to-a-component](https://es.react.dev/learn/passing-props-to-a-component)
5. React. *El estado: la memoria de un componente*. [https://es.react.dev/learn/state-a-components-memory](https://es.react.dev/learn/state-a-components-memory)
6. React. *Respondiendo a eventos*. [https://es.react.dev/learn/responding-to-events](https://es.react.dev/learn/responding-to-events)
7. Carroll, M. (24 de febrero de 2026). *The React Foundation: A New Home for React Hosted by the Linux Foundation*. [https://react.dev/blog/2026/02/24/the-react-foundation](https://react.dev/blog/2026/02/24/the-react-foundation)
8. Elliot, I. (9 de marzo de 2026). *React Foundation Officially Launched*. I Programmer. [https://www.i-programmer.info/news/87-web-development/18714-react-foundation-officially-launched.html](https://www.i-programmer.info/news/87-web-development/18714-react-foundation-officially-launched.html)
9. The New Stack. (3 de marzo de 2026). *Meta gave React its own foundation*. [https://thenewstack.io/react-foundation-open-source-governance/](https://thenewstack.io/react-foundation-open-source-governance/)
10. Wikipedia. *React (JavaScript library)*. [https://en.wikipedia.org/wiki/React_(JavaScript_library)](https://en.wikipedia.org/wiki/React_(JavaScript_library))
11. Engineering at Meta. *Introducing the React Foundation*. [https://engineering.fb.com/2025/10/07/open-source/introducing-the-react-foundation-the-new-home-for-react-react-native/](https://engineering.fb.com/2025/10/07/open-source/introducing-the-react-foundation-the-new-home-for-react-react-native/)
12. Discord Engineering. (5 de marzo de 2025). *Supercharging Discord Mobile: Our Journey to a Faster App*. [https://discord.com/blog/supercharging-discord-mobile-our-journey-to-a-faster-app](https://discord.com/blog/supercharging-discord-mobile-our-journey-to-a-faster-app)
