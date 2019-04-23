---
title: Migración desde Vue 1.x
type: guide
order: 701
---

## Preguntas frecuentes

> Woah - ¡esta página es súper larga! ¿Eso significa que la versión 2.0 es completamente diferente y tendré que aprender los conceptos básicos una y otra vez y la migración será prácticamente imposible?

¡Me alegra que lo pregunte! La respuesta es no. Alrededor del 90% de la API es la misma y los conceptos básicos no han cambiado. Es largo porque nos gusta ofrecer explicaciones muy detalladas e incluir muchos ejemplos. Quédese tranquilo, __¡esto no es algo que tenga que leer de arriba abajo!__

> ¿Por dónde debería comenzar una migración?

1. Comience ejecutando el [asistente de migración](https://github.com/vuejs/vue-migration-helper) en un proyecto actual. Hemos minimizado y comprimido cuidadosamente un desarrollo de Vue superior en una interfaz de línea de comando simple. Así, cada vez que se reconoce una característica obsoleta, le informa, ofrece sugerencias y proporciona enlaces a más información.

2. Después de eso, navegué por la tabla de contenido de esta página en la barra lateral. Si ve un tema que puede afectarle, pero que el asistente de migración no captó, compruébelo.

3. Si usted tiene algunos tests, ejecútelos y vea lo que aún falla. Sino tiene test, solo abra la aplicación en su navegador y esté atento a las advertencias o errores mientras navega.

4. En este punto, su aplicación debería estar migrada por completo. Si aún tiene ganas de más, puede leer el resto de esta página [desde el principio](index.html). Muchas partes serán demasiado fáciles, ya que ya está familiarizado con los conceptos básicos.

> ¿Cuánto tiempo me llevará migrar una aplicación de Vue 1.x a 2.0?

Depende de algunos factores:

- El tamaño de su aplicación (en las aplicaciones pequeñas y medianas probablemente será menos de un día)

- Cuantas veces se distrae y comienza a jugar con una nueva función genial.😉 &nbsp;No le juzgamos, ¡A nosotros también nos pasó mientras construíamos 2.0!

- Las funciones obsoletas que esté usando. La mayoría se pueden actualizar con Buscar y Reemplazar, pero otras pueden tardar unos minutos. Si actualmente no está siguiendo las mejores prácticas, Vue 2.0 también intentará forzarle más a hacerlo. Esto es algo bueno a largo plazo, pero también podría significar una actualización (aunque posiblemente le retrase) significativa.

> Si actualizo a Vue 2, ¿también tendré que actualizar Vuex y Vue Router?

Solo Vue Router 2 es compatible con Vue 2, así que sí, también tendrá que seguir la [ruta de migración de Vue Router](migration-vue-router.html). Afortunadamente, la mayoría de aplicaciones no tienen mucho código de rutas, por lo que probablemente no le tome más de una hora.

En cuanto a Vuex, incluso la versión 0.8 es compatible con Vue 2, por lo que no está obligado a actualizar. La única razón por la que es posible que desee actualizar de inmediato es aprovechar las nuevas funciones de Vuex 2, como los módulos y el _boilerplate_ reducido.

## Templates (plantillas)

### Instancias de fragmento <sup>eliminadas</sup>

Cada componente ha de tener exactamente un elemento raíz. Las instancias de fragmento ya no son permitidas. Si tienes una plantilla como esta:

``` html
<p>foo</p>
<p>bar</p>
```

Se recomienda envolver todo el contenido edentro de un nuevo elemento, así:

``` html
<div>
  <p>foo</p>
  <p>bar</p>
</div>
```

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Ejecute su suite o aplicación de prueba end-to-end después de la actualización y busque: <strong>console warnings</strong> acerca de varios elementos raíz en una plantilla.</p>  
</div>
{% endraw %}

## _Lifecycle Hooks_

### `beforeCompile` <sup>eliminado</sup>

Use el _hook_ `created` en su lugar.

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Ejecute <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> en tu código base para encontar todos los ejemplos de este <i>hook</i>.</p>
</div>
{% endraw %}

### `compiled` <sup>reemplazado</sup>

Use en su lugar el nuevo _hook_ `mounted`

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Ejecute <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> en tu código base para encontar todos los ejemplos de este <i>hook</i>.</p>
</div>
{% endraw %}

### `attached` <sup>eliminado</sup>

Utilice una verificación _in-DOM_ personalizada en otros _hooks_. Por ejemplo, para reemplazar:

``` js
attached: function () {
  doSomething()
}
```

Se pudiera utilizar:

``` js
mounted: function () {
  this.$nextTick(function () {
    doSomething()
  })
}
```

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Ejecute <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> en tu código base para encontar todos los ejemplos de este <i>hook</i>.</p>
</div>
{% endraw %}

### `detached` <sup>eliminado</sup>

Utilice una verificación _in-DOM_ personalizada en otros _hooks_. Por ejemplo, para reemplazar:

``` js
detached: function () {
  doSomething()
}
```

Pudieras utilizar:

``` js
destroyed: function () {
  this.$nextTick(function () {
    doSomething()
  })
}
```

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Ejecute <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> en tu código base para encontar todos los ejemplos de este <i>hook</i>.</p>
</div>
{% endraw %}

### `init` <sup>renombrado</sup>

En su lugar utilice el nuevo _hook_ `beforeCreate`, que es esencialmente lo mismo. Fue renombrado por consistencia con otros métodos del _lifecycle_.

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Ejecute <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> en tu código base para encontar todos los ejemplos de este <i>hook</i>.</p>
</div>
{% endraw %}

### `ready` <sup>reemplazado</sup>

En su lugar utilice el nuevo _hook_ `mounted`. Debe tenerse en cuenta que con `mounted`, no hay garantía de estar _in-document_. Para ello, también incluya `Vue.nextTick`/`vm.$nextTick`. Por ejemplo:

``` js
mounted: function () {
  this.$nextTick(function () {
    // Se asume que this.$el está _in-document_
  })
}
```

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Ejecute <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> en tu código base para encontar todos los ejemplos de este <i>hook</i>.</p>
</div>
{% endraw %}

## `v-for`

### Orden de Argumentos para Arreglos `v-for` <sup>cambiado</sup>

Al incluir un `index`, el orden de los argumentos de las matrices solía ser `(index, value)`. Ahora es `(value, index)` para ser más coherente con los métodos de matriz nativos de JavaScript como `forEach` y `map`.

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Ejecute <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> en tu código base para encontrar el orden absoleto de los argumentos. Observa que si nombras tus argumentos de una forma inusual como <code>position</code> o <code>num</code>, el _helper_ no los marcará.</p>
</div>
{% endraw %}

### Orden de los Argumentos para los Objetos `v-for`<sup>cambiado</sup>

Cuando se incluye una `key`, el orden de argumento de los objetos solía ser `(key, value)`. Ahora es `(value, key)` para ser más consistente con iteradores de objetos comunes como _lodash_.

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Ejecute <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> en tu código base para encontrar todos los ejemplos del absoleto orden de los argumentos. Observa que si nombras tus argumentos como <code>name</code> o <code>property</code>, el _helper_ no los marcará.</p>
</div>
{% endraw %}

### `$index` y `$key` <sup>eliminados</sup>

Las variables `$index` y `$key` asignadas implícitamente se han eliminado a favor de definirlas explícitamente en `v-for`. Esto hace que el código sea más fácil de leer para los desarrolladores menos experimentados con Vue y también da como resultado un comportamiento mucho más claro cuando se trata de bucles anidados.

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Ejecute <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> en tu código base para encontrar ejemplos de these removed variables. Si te falta alguno, también verás <strong>console errors</strong> tales como: <code>Uncaught ReferenceError: $index is not defined</code></p>
</div>
{% endraw %}

### `track-by` <sup>reemplazado</sup>

`track-by` ha sido reemplazado por `key`, que funciona como cualquier otro atributo (sin `v-bind:` o el prefijo `:`), es tratado como una cadena. En la mayoría de los casos, se desearía _dynamic binding_ lo que espera una expresión. Por ejemplo, en lugar de:

``` html
<div v-for="item in items" track-by="id">
```

Ahora se ha de escribir:

``` html
<div v-for="item in items" v-bind:key="item.id">
```

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Ejecute <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> en tu código base para encontrar ejemplos de <code>track-by</code>.</p>
</div>
{% endraw %}

### Rango de Valores `v-for` <sup>cambiado</sup>

Anteriormente con `v-for="number in 10"` se tenía un `number` que empezaba en 0 y terminaba en 9. Ahora empieza en 1 y termina en 10.

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Busca en tu código por según la expresión regular <code>/\w+ in \d+/</code>. Donde quiera que aparezca dentro de un <code>v-for</code>, comprueba si tal vez te afecta.</p>
</div>
{% endraw %}

## Props

### Opción _Prop_ `coerce` <sup>eliminado</sup>

Si se desea forzar una _prop_, en su lugar configure un valor local calculado basado en él. Por ejemplo, en lugar de:

``` js
props: {
  username: {
    type: String,
    coerce: function (value) {
      return value
        .toLowerCase()
        .replace(/\s+/, '-')
    }
  }
}
```

Se pudiera escribir:

``` js
props: {
  username: String,
},
computed: {
  normalizedUsername: function () {
    return this.username
      .toLowerCase()
      .replace(/\s+/, '-')
  }
}
```

Esto trae consigo algunas ventajas:

- Todavía será posible tener acceso al valor original de la _prop_.
- Es obligatorio ser más explícito, dando a su valor coaccionado un nombre que lo diferencie del valor pasado en la _prop_.

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Ejecute <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> en tu código base para encontrar ejemplos de la opción <code>coerce</code>.</p>
</div>
{% endraw %}

### Opción _Prop_ `twoWay` <sup>eliminado</sup>

Las _props_ ahora son siempre hacia abajo (hacia los hijos). Para producir efectos secundarios en el _parent scope_, un componente debe emitir explícitamente un evento en lugar de depender del _binding_ implícito. Para obtener más información, consulte:

- [Custom component events](components.html#Custom-Events)
- [Custom input components](components.html#Form-Input-Components-using-Custom-Events) (using component events)
- [Global state management](state-management.html)

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Ejecute <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> en tu código base para encontrar ejemplos de la opción <code>twoWay</code>.</p>
</div>
{% endraw %}

### Modificadores `.once` y `.sync` en `v-bind` <sup>eliminado</sup>

Las _props_ ahora son siempre hacia abajo (hacia los hijos). Para producir efectos secundarios en el _parent scope_, un componente debe emitir explícitamente un evento en lugar de depender del _binding_ implícito. Para obtener más información, consulte:

- [Custom component events](components.html#Custom-Events)
- [Custom input components](components.html#Form-Input-Components-using-Custom-Events) (using component events)
- [Global state management](state-management.html)

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Ejecute <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> en tu código base para encontrar ejemplos de los modificadores <code>.once</code> y <code>.sync</code>.</p>
</div>
{% endraw %}

### _Prop Mutation_ <sup>absoleto</sup>

Ahora mutar una _prop_ es considerado un anti-patrón, por ejemplo: declarando una _prop_ y cambiando `this.myProp = 'someOtherValue'` en el componente. Debido a esto el nuevo mecanismo de renderización, donde quiera que el componente padre se vuleva a renderizar, los cambios locales del componente hijo serán sobreescritos .

la mayoría de los casos de mutar una _prop_ pueden ser reemplazados por una de estas opciones:

- a data property, with the prop used to set its default value
- a computed property

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Run your end-to-end test suite or app after upgrading and look for <strong>console warnings</strong> about prop mutations.</p>
  <p>Ejecuta tus pruebas end-to-end después de actualizar y busca <strong>console warnings</strong> sobre <i>prop mutations</i>.</p>
</div>
{% endraw %}

### _Props_ en una _Root Instance_ <sup>reemplazado</sup>

En las instancias raíces de Vue (por ejemplo instancias creadas así `new Vue({ ... })`), se debe usar `propsData` en lugar de `props`.

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Ejecuta tus pruebas end-to-end. Los <strong>failed tests</strong> te alertarán sobre el hecho de que las _props_ pasadas a las instancias raíces ya no estarán funcionando.</p>
</div>
{% endraw %}

## Propiedades Calculadas (_computed_)

### `cache: false` <sup>absoleto</sup>

La invalidación de almacenamiento en caché de las propiedades calculadas se eliminará en futuras versiones de Vue. Reemplace las propiedades calculadas con métodos que no estén _cacheadas_, lo que tendrá el mismo resultado.

Por ejemplo:

``` js
template: '<p>message: {{ timeMessage }}</p>',
computed: {
  timeMessage: {
    cache: false,
    get: function () {
      return Date.now() + this.message
    }
  }
}
```

O con métodos de componentes:

``` js
template: '<p>message: {{ getTimeMessage }}</p>',
methods: {
  getTimeMessage: function () {
    return Date.now() + this.message
  }
}
```

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Ejecute <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> en tu código base para encontrar ejemplos de the <code>cache: false</code> option.</p>
</div>
{% endraw %}

## _Built-In Directives_

### Veracidad/Falsedad con `v-bind` <sup>cambiado</sup>

Cuando se use `v-bind`, los únicos valores equivalentes a _false_ son: `null`, `undefined`, y `false`. Esto significa que el `0` y las cadenas vacías se renderizarán como valores _true_. Por ejemplo, `v-bind:draggable="''"` será renderizado como `draggable="true"`.

Para atributos enumerados, además de los valores de falsedad de arriba, la cadena `"false"` también será renderizada como `attr="false"`.

<p class="tip">Observe que para oras directivas (como `v-if` y `v-show`), la veracidad normal de JavaScript aún se mantiene.</p>

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
    <p>Ejecuta tus pruebas end-to-end, si es que tienes. Los <strong>failed tests</strong> te alertarán sobre cualquier parte de tu <i>app</i> que pueda ser afectada por este cambio.</p>
</div>
{% endraw %}

### Listening for Native Events on Components with `v-on` <sup>cambiado</sup>

When used on a component, `v-on` now only listens to custom events `$emit`ted by that component. To listen for a native DOM event on the root element, you can use the `.native` modifier. Por ejemplo:

``` html
<my-component v-on:click.native="doSomething"></my-component>
```

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Ejecuta tus pruebas end-to-end, si es que tienes. Los <strong>failed tests</strong> te alertarán sobre cualquier parte de tu <i>_app_</i> que pueda ser afectada por este cambio.</p> 
</div>
{% endraw %}

### Atributo Parámetro `debounce` para `v-model` <sup>eliminado</sup>

El _debouncing_ se usa para limitar la frecuencia con la que ejecutamos solicitudes Ajax y otras operaciones costosas. El parámetro atributo `debounce` de Vue para `v-model` hizo esto fácil para casos muy simples, pero esto realmente le quitó los rebotes a los __state updates__ en lugar de las propias operaciones costosas. Es una diferencia sutil, pero viene con limitaciones a medida que una aplicación crece.

Estas limitaciones se hacen evidentes al diseñar un indicador de búsqueda, como este por ejemplo:

{% raw %}
<script src="https://cdn.jsdelivr.net/lodash/4.13.1/lodash.js"></script>
<div id="debounce-search-demo" class="demo">
  <input v-model="searchQuery" placeholder="Escribe algo">
  <strong>{{ searchIndicator }}</strong>
</div>
<script>
new Vue({
  el: '#debounce-search-demo',
  data: {
    searchQuery: '',
    searchQueryIsDirty: false,
    isCalculating: false
  },
  computed: {
    searchIndicator: function () {
      if (this.isCalculating) {
        return '⟳ Obteniendo nuevos resultados'
      } else if (this.searchQueryIsDirty) {
        return '... Escribiendo'
      } else {
        return '✓ Hecho'
      }
    }
  },
  watch: {
    searchQuery: function () {
      this.searchQueryIsDirty = true
      this.expensiveOperation()
    }
  },
  methods: {
    expensiveOperation: _.debounce(function () {
      this.isCalculating = true
      setTimeout(function () {
        this.isCalculating = false
        this.searchQueryIsDirty = false
      }.bind(this), 1000)
    }, 500)
  }
})
</script>
{% endraw %}

Usando el atributo `debounce`, no habría forma de detectar el estado "Escribiendo", porque perdimos acceso al estado del input en tiempo real. Desacoplando de Vue la función _debounce_, somos capaces de _debounce_ sólo la operación que queremos limitar, eliminando los límites en las _features_ podemos desarrollar:

``` html
<!--
Mediante el uso de la función de ´debounce´ de lodash u otra
librería de utilidades, conocemos la implementación específica de ´debounce´.
Su uso será el mejor en su clase y podemos utilizarlo en cualquier lugar. No sólo
en nuestro template.
-->
<script src="https://cdn.jsdelivr.net/lodash/4.13.1/lodash.js"></script>
<div id="debounce-search-demo">
  <input v-model="searchQuery" placeholder="Escribe algo">
  <strong>{{ searchIndicator }}</strong>
</div>
```

``` js
new Vue({
  el: '#debounce-search-demo',
  data: {
    searchQuery: '',
    searchQueryIsDirty: false,
    isCalculating: false
  },
  computed: {
    searchIndicator: function () {
      if (this.isCalculating) {
        return '⟳ Obteniendo nuevos resultados'
      } else if (this.searchQueryIsDirty) {
        return '... Escribiendo'
      } else {
        return '✓ Done'
      }
    }
  },
  watch: {
    searchQuery: function () {
      this.searchQueryIsDirty = true
      this.expensiveOperation()
    }
  },
  methods: {
    // This is where the debounce actually belongs.
    expensiveOperation: _.debounce(function () {
      this.isCalculating = true
      setTimeout(function () {
        this.isCalculating = false
        this.searchQueryIsDirty = false
      }.bind(this), 1000)
    }, 500)
  }
})
```

Otra ventaja de este enfoque es que habrá momentos en que _debouncing_ no es la función contenedora correcta. Por ejemplo, al consultar una API para sugerencias de búsqueda, esperando a ofrecer sugerencias hasta después de que el usuario haya dejado de escribir durante un período de tiempo, no es una experiencia ideal. Lo que probablemente desea en su lugar es una función __throttling__. Ahora, ya que ya está utilizando una librería de utilidades como lodash, refactorizar para utilizar su función `throttle` en su lugar toma sólo unos pocos segundos.

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Ejecute <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> en tu código base para encontrar ejemplos del atributo <code>debounce</code>.</p>
</div>
{% endraw %}

### Atributos Parámetros `lazy` o `number` para `v-model` <sup>reemplazado</sup>

Los atributos parámetros `lazy` y `number` ahora son modificadores. Para hacerlo mas claro, en lugar de:

``` html
<input v-model="name" lazy>
<input v-model="age" type="number" number>
```

Se debería usar:

``` html
<input v-model.lazy="name">
<input v-model.number="age" type="number">
```

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Ejecute <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> en tu código base para encontrar ejemplos de estos atributos parámetros.</p>
</div>
{% endraw %}

### Atributo `value` con `v-model` <sup>eliminado</sup>

A `v-model` ya no le intresa sobre el valor inicial de un atributo `value`. Por prudencia, tratará los datos de la instancia de Vue como fuente confiable.

Esto significa que este elemento:

``` html
<input v-model="text" value="foo">
```

respaldado por estos datos:

``` js
data: {
  text: 'bar'
}
```

será renderizado con un valor de "bar" en lugar de "foo". Lo mismo para `<textarea>` con contenido. En lugar de:

``` html
<textarea v-model="text">
  hola mundo
</textarea>
```

Deberías asegurarte que tu valor inicial para `text` is "hola mundo".

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Run your end-to-end test suite or app after upgrading and look for <strong>console warnings</strong> about inline value attributes with <code>v-model</code>.</p>
  <p>Ejecuta tus pruebas end-to-end desde pués de actualiza busca <strong>console warnings</strong> sobre <i>inline value attributes</i> con <code>v-model</code>.</p>
</div>
{% endraw %}

### Valores Primitivos Iterados `v-model` con `v-for` <sup>eliminado</sup>

Casos como este ya no funcionan:

``` html
<input v-for="str in strings" v-model="str">
```

La razón es que este es el JavaScript equivalente al que el `<input>` es compilado: 

``` js
strings.map(function (str) {
  return createElement('input', ...)
})
```

Como se puede ver, el _binding_ bidireccional de `v-model` no tiene sentido aquí. Cambiando `str` a otro valor en la función iteradora no hará nada porque hay una sola variable en el _scope_ de la función.

En su lugar, se debería utilizar un arreglo de __objects__ de moodo que `v-model` pueda actualizar el campo en el objeto. Por ejemplo:

``` html
<input v-for="obj in objects" v-model="obj.str">
```

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Ejecuta tus pruebas si es que tienes alguna. Los <strong>failed tests</strong> te alertarán sobre algunas partes de tu <i>app</i> que se verán afectadas por este cambio.</p>
</div>
{% endraw %}

### `v-bind:style` con Sintaxis de Objeto e `!important` <sup>eliminado</sup>

Ya esto no funciona:

``` html
<p v-bind:style="{ color: myColor + ' !important' }">hola</p>
```

Si fuera necesario reeplazar otro `!important`, se debería usar la sintaxis de cadena:

``` html
<p v-bind:style="'color: ' + myColor + ' !important'">hola</p>
```

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Ejecute <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> en tu código base para encontrar ejemplos de <i>style bindings</i> con <code>!important</code> in objetos.</p>
</div>
{% endraw %}

### `v-el` y `v-ref` <sup>reemplazados</sup>

Para simplicidad, `v-el` y `v-ref` han sido unidos en el atributo `ref`, accesible en una instancia de componente a través de `$refs`. Esto significa que `v-el:my-element` se convertiría en `ref="myElement"` and `v-ref:my-component` se convertiría en `ref="myComponent"`. Cuando se usa en un elemento normal, `ref` será el elemento DOM, y cuando sea usado en un componente, `ref` será la instancia del componente.

desde que `v-ref` ya no es una directiva, sino un atributo especial, también puede ser definidi dinámicamente. Esto es especialmente útil en combinación con `v-for`. Por ejemplo:

``` html
<p v-for="item in items" v-bind:ref="'item' + item.id"></p>
```

Anteriormente, `v-el`/`v-ref` combinados con `v-for` producirían un arreglo de elementos/componentes, porque no era posible darle a cada elemento un nombre único. Aún se puede lograr este comportamiento si se le asigna a cada elmento el mismo `ref`:

``` html
<p v-for="item in items" ref="items"></p>
```

A diferencia de Vue 1.x, estos `$refs` no son _reactive_, porque son registrados/actualizados durante el proceso propio de renderizado. Hacerlos _reactive_ requeriría duplicar _renders_ por cada cambio. 

Por otro lado, `$refs` están inicialmente diseñados para ser accedidos programáticamente en JavaScript. No es recomendable fiarse de ellos en _templates_, porque significaría referisrse a  un estado que no pertenece a la instancia propia. Esto violaría el _data-driven view model_ de Vue.

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Ejecute <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> en tu código base para encontrar ejemplos de <code>v-el</code> y <code>v-ref</code>.</p>
</div>
{% endraw %}

### `v-else` con `v-show` <sup>eliminado</sup>

`v-else` ya no funcioa con `v-show`. Use en su lugar `v-if` con una expresión de negación. Por ejemplo, en lugar de:

``` html
<p v-if="foo">Foo</p>
<p v-else v-show="bar">Not foo, but bar</p>
```

Se puede usar:

``` html
<p v-if="foo">Foo</p>
<p v-if="!foo && bar">Not foo, but bar</p>
```

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Ejecute <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> en tu código base para encontrar ejemplos de <code>v-else</code> con <code>v-show</code>.</p>
</div>
{% endraw %}

## Directivas Personalizadas <sup>simplificado</sup>

Las directivas tienen altamente reducido el alcance de su responsabilidad: ahora sólo existen para aplicar manipulaciones a bajo nivel directamente al DOM. En la mayoría de los casos, es preferible usar compomentes como la mejor forma de reutilización de código.

Some of the most notable differences include:

- Las directivas ya no tienen instancias. Ya no más `this` dentro de los _hooks_ de directivas. En cambio, ellas reciben lo que puedan necesitar como argumentos. Si es necesario hacer persistir los estados a través de los _hooks_, se puede ser en`el`.
- Opciones como `acceptStatement`, `deep`, `priority`, etc, han sido eliminadas. Para sustituir las directivas `twoWay`, see [this example](#Two-Way-Filters-replaced).
- Algunos de los _hooks_ actuales tienen un comportamiento diferente y también hay un par de nuevos _ganchos_.

Afortunadamente, desde que las nuevas directivas son mucho más simples, pueden ser más fácilmente aprendidas. Lea [Custom Directives guide](custom-directive.html) para saber más.

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Ejecute <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> en tu código base para encontrar ejemplos dedirectivas definidas. El <i>helper</i> las marcará todas, como en la mayoría de los casos en que se desee refactorizar un componente.</p>
</div>
{% endraw %}

### Directive `.literal` Modifier <sup>eliminado</sup>

El modificador `.literal` ha sido eliminado, ya que el mismo puede se puede obtener fácilmente proporcionando una cadena literal como el valor.

Por ejemplo, se puede actualizar:

``` js
<p v-my-directive.literal="foo bar baz"></p>
```

a:

``` html
<p v-my-directive="'foo bar baz'"></p>
```

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Ejecute <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> en tu código base para encontrar ejemplos del modificador `.literal` en una directiva.</p>
</div>
{% endraw %}

## Transiciones

### Atributo `transition` <sup>reemplazado</sup>

El sistema de transiciones de Vue ha cambiado muy drásticamente y ahora usa elementos envolventes como `<transition>` y `<transition-group>`, en lugar del atributo `transition`. Se recomienda leer [Transitions guide](transitions.html) para saber más.

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Ejecute <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> en tu código base para encontrar ejemplos del atributo <code>transition</code>.</p>
</div>
{% endraw %}

### `Vue.transition` para Transiciones Reusables <sup>reemplazado</sup>

Con el nuevo sistema de transición, ahora se puede [usar componentss para transiciones reusables](transitions.html#Reusable-Transitions).

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Ejecute <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> en tu código base para encontrar ejemplos de <code>Vue.transition</code>.</p>
</div>
{% endraw %}

### Atributo de Transición `stagger` <sup>eliminado</sup>

Si se necesita escalonar las transiciones de lista, puede controlar la temporización estableciendo y accediendo a un `data-index` (o atributo similar) en un elemento. Vea [un ejemplo aquí](transitions.html#Staggering-List-Transitions).

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Ejecute <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> en tu código base para encontrar ejemplos del atributo <code>transition</code>. Durante tu actualización, puedes hacer una transición (juego de palabras muy pensado) a la nueva estrategia.</p>
</div>
{% endraw %}

## Eventos

### `events` option <sup>eliminado</sup>

La opción `events` ha sido eliminada. Los manipuladores de eventos han de ser registrados dentrode del _hook _ `created`. Revise [`$dispatch` and `$broadcast` migration guide](#dispatch-and-broadcast-replaced) para un ejemplo más detallado.

### `Vue.directive('on').keyCodes` <sup>reemplazado</sup>

La mejor forma de configurar `keyCodes` es a través de `Vue.config.keyCodes`. Por ejemplo:

``` js
// enable v-on:keyup.f1
Vue.config.keyCodes.f1 = 112
```
{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Ejecute <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> en tu código base para encontrar ejemplos de la vieja sintaxis de configuración de<code>keyCode</code> .</p>
</div>
{% endraw %}

### `$dispatch` y `$broadcast` <sup>reemplazados</sup>

`$dispatch` y `$broadcast` han sido eliminados en aras de una comunicación _cross-component_ más explícita y un manejo de estados más, como [Vuex](https://github.com/vuejs/vuex).

El problema es que los flujos de eventos que dependen de la estructura de árbol de un componente, pueden ser difíciles de razonar y muy frágiles cuando el árbol se vuelve grande. No se escala bien y no queremos que se convierta en un dolor más tarde.`$dispatch` and `$broadcast` tampoco resuelven la comunicación entre componentes hermanos.

Uno de los usos más comunes de estos métodos es la comunicación entre un padre y sus hijos directos. En estos casos, en realidad se puede [escuchar un `$emit` de un hijo con `v-on`](components.html#Form-Input-Components-using-Custom-Events). Esto permite mantener la conveniencia de los eventos con mayor explicitud.

Sin embargo, cuando se comunica entre descendientes/antepasados lejanos, `$emit` no ayuda. En su lugar, la actualización más simple posible sería usar un centro de eventos centralizado (_hub_). Esto tiene la ventaja añadida de permitir lacomunicación entre los componentes sin importar dónde se encuentran en el árbol componente, incluso entre hermanos! Dado que las instancias de Vue implementan una interfaz de emisor de eventos, en realidad puede utilizar una instancia de Vue vacía para este propósito.

Por ejemplo, digamos que tenemos una aplicación de tareas como esta:

```
Todos
|-- NewTodoInput
|-- Todo
    |-- DeleteTodoButton
```

We could manage communication between components with this single event hub:
Podríamos gestionar la comunicación entre los componentes con este único centro de eventos (_event hub_):

``` js
// Este es el event hub que usaremos en
// cada componente para la comunicación entre ellos.
var eventHub = new Vue()
```

Entonces entre nuestros componentes, podemos usar `$emit`, `$on`, `$off` para emitir eventos, escuchar eventos, y limpiar los _listeners_, respectivamente:

``` js
// NewTodoInput
// ...
methods: {
  addTodo: function () {
    eventHub.$emit('add-todo', { text: this.newTodoText })
    this.newTodoText = ''
  }
}
```

``` js
// DeleteTodoButton
// ...
methods: {
  deleteTodo: function (id) {
    eventHub.$emit('delete-todo', id)
  }
}
```

``` js
// Todos
// ...
created: function () {
  eventHub.$on('add-todo', this.addTodo)
  eventHub.$on('delete-todo', this.deleteTodo)
},
// It's good to clean up event listeners before
// a component is destroyed.
beforeDestroy: function () {
  eventHub.$off('add-todo', this.addTodo)
  eventHub.$off('delete-todo', this.deleteTodo)
},
methods: {
  addTodo: function (newTodo) {
    this.todos.push(newTodo)
  },
  deleteTodo: function (todoId) {
    this.todos = this.todos.filter(function (todo) {
      return todo.id !== todoId
    })
  }
}
```

Este patrón puede servir como un reemplazo para `$dispatch` y `$broadcast` en escenarios simples, pero para casos más complejos, se recomienda usar una capa de administración de estado dedicada como [Vuex](https://github.com/vuejs/vuex).

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Ejecute <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> en tu código base para encontrar ejemplos de <code>$dispatch</code> y <code>$broadcast</code>.</p>
</div>
{% endraw %}

## Filtros

### Filtros Fuera de las Interpolaciones de Texto <sup>eliminado</sup>

Ahora los filtros solo se pueden usar dentro de las interpolaciones de texto (`{% raw %}{{ }}{% endraw %}` tags). En el pasado hemos estado usando filtros dentro de directivas como `v-model`, `v-on`, etc. Llevó a más complejidad que conveniencia. Para el filtrado de listas en `v-for`, también es mejor mover esa lógica a JavaScript como propiedades calculadas, de modo que se pueda reutilizar en todo el componente.

En general, cuando algo se puede lograr en JavaScript puro, queremos evitar la introducción de una sintaxis especial, como filtros, para mantenernos en el mismo entorno. Así es como se pueden reemplazar los filtros de directiva incorporados de Vue:

#### Reemplazando el Filtro `debounce`

En lugar de:

``` html
<input v-on:keyup="doStuff | debounce 500">
```

``` js
methods: {
  doStuff: function () {
    // ...
  }
}
```

Use [lodash's `debounce`](https://lodash.com/docs/4.15.0#debounce) (o posiblemente [`throttle`](https://lodash.com/docs/4.15.0#throttle)) para limitar directa la llamada del costoso método. Se puede lograr lo mismo de arriba así:

``` html
<input v-on:keyup="doStuff">
```

``` js
methods: {
  doStuff: _.debounce(function () {
    // ...
  }, 500)
}
```

Para más información sobre las ventajas de esta estrategia, vea [el ejemplo con `v-model`](#debounce-Param-Attribute-for-v-model-removed).

#### Reemplazando el Filtro `limitBy`

En lugar de:

``` html
<p v-for="item in items | limitBy 10">{{ item }}</p>
```

Use JavaScript [método `.slice`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/slice#Examples) en una propiedad calculada:

``` html
<p v-for="item in filteredItems">{{ item }}</p>
```

``` js
computed: {
  filteredItems: function () {
    return this.items.slice(0, 10)
  }
}
```

#### Replacing the `filterBy` Filter

En lugar de:

``` html
<p v-for="user in users | filterBy searchQuery in 'name'">{{ user.name }}</p>
```

Use JavaScript [método `.filter`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/filter#Examples) en una propiedad calculada:

``` html
<p v-for="user in filteredUsers">{{ user.name }}</p>
```

``` js
computed: {
  filteredUsers: function () {
    var self = this
    return self.users.filter(function (user) {
      return user.name.indexOf(self.searchQuery) !== -1
    })
  }
}
```

El `.filter` nativo de JavaScript también puede administrar operaciones de filtrado mucho más complejas, ya que tiene acceso a toda la potencia de JavaScript dentro de las propiedades calculadas. Por ejemplo, si desea encontrar todos los usuarios activos y coinciden con su nombre (no sensible a mayúsculas) y correo electrónico:

``` js
var self = this
self.users.filter(function (user) {
  var searchRegex = new RegExp(self.searchQuery, 'i')
  return user.isActive && (
    searchRegex.test(user.name) ||
    searchRegex.test(user.email)
  )
})
```

#### Reemplazando el Filtro`orderBy`

En lugar de:

``` html
<p v-for="user in users | orderBy 'name'">{{ user.name }}</p>
```

Use [lodash's `orderBy`](https://lodash.com/docs/4.15.0#orderBy) (o probablemente [`sortBy`](https://lodash.com/docs/4.15.0#sortBy)) en una propiedad calculada:

``` html
<p v-for="user in orderedUsers">{{ user.name }}</p>
```

``` js
computed: {
  orderedUsers: function () {
    return _.orderBy(this.users, 'name')
  }
}
```

Incluso se puede ordenar por varias columnas:

``` js
_.orderBy(this.users, ['name', 'last_login'], ['asc', 'desc'])
```

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Ejecute <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> en tu código base para encontrar ejemplos de filtros usados dentro de directivas. Si te pierdes alguno, también verás <strong>console errors</strong>.</p>
</div>
{% endraw %}

### Filter Argument Syntax <sup>cambiado</sup>

Filters' syntax for arguments now better aligns with JavaScript function invocation. So instead of taking space-delimited arguments:

``` html
<p>{{ date | formatDate 'YY-MM-DD' timeZone }}</p>
```

We surround the arguments with parentheses and delimit the arguments with commas:

``` html
<p>{{ date | formatDate('YY-MM-DD', timeZone) }}</p>
```

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Ejecute <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> en tu código base para encontrar ejemplos de the old filter syntax. If you miss any, you should also see <strong>console errors</strong>.</p>
</div>
{% endraw %}

### Filtros de Texto _Built-In_ <sup>removed</sup>

Aunque los filtros dentro de las interpolaciones de texto todavía están permitidos, todos los filtros se han quitado. En su lugar, se recomienda utilizar librerías más especializadas para resolver problemas en cada dominio (por ejemplo, [`date-fns`](https://date-fns.org/) para formatear fechas y [`accounting`](http://openexchangerates.github.io/accounting.js/) para monedas).

Para cada uno de los filtros de texto incorporados de Vue, vamos a través de cómo pueden reemplazarse a continuación. El código de ejemplo podría existir en funciones auxiliares personalizadas, métodos o propiedades calculadas.

#### Reemplazando el Filtro `json`

En realidad, ya no es necesario para depurar, ya que Vue le dará formato a la salida automáticamente, ya sea una cadena, un número, un arreglo o un objeto sin formato. Si desea la misma funcionalidad `JSON.stringify` de JavaScript, se puede utilizar en un método o propiedad calculada (_computed_).

#### Reemplazando el Filtro `capitalize`

``` js
text[0].toUpperCase() + text.slice(1)
```

#### Reemplazando el Filtro `uppercase`

``` js
text.toUpperCase()
```

#### Reemplazando el Filtro `lowercase`

``` js
text.toLowerCase()
```

#### Reemplazando el Filtro `pluralize`

El paquete [pluralize](https://www.npmjs.com/package/pluralize) de NPM sirve muy bien para este propósito, pero si tan solo deseas pluralizar una palabra específica word o tener salidas especiales para casos como `0`, entonces podrías definir tus propias funciones para pluralizar. Por ejemplo:

``` js
function pluralizeKnife (count) {
  if (count === 0) {
    return 'no knives'
  } else if (count === 1) {
    return '1 knife'
  } else {
    return count + 'knives'
  }
}
```

#### Reemplazando el Filtro `currency`

Para una implementación muy modesta, usted podría hacer algo como esto:

``` js
'$' + price.toFixed(2)
```

Sin embargo, en muchos casos, seguirás teniendo un comportamiento extraño (por ejemplo, `0.035.toFixed(2)` redondea hasta `0.04`, pero `0.045` redondea a `0.04`). Para solucionar estos problemas, puede utilizar la biblioteca [`accounting`](http://openexchangerates.github.IO/Accounting.js/) para formatear monedas de forma más fiable.

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Ejecute <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> en tu código base para encontrar ejemplos de filtros de texto obsoletos. Si te pierdes alguno deberás ver <strong>console errors</strong>.</p>
</div>
{% endraw %}

### Filtros Bidireccionales <sup>reemplazado</sup>

Algunos usuarios han disfrutado usando filtros bidireccionales con `v-model` para crear entradas interesantes con muy poco código. Aunque _aparentemente_ simple, sin embargo, los filtros bidireccionales también pueden ocultar una gran complejidad e incluso alentar la mala _UX_ retrasando las actualizaciones de estado. En su lugar, los componentes que encapsulan una entrada se recomiendan como una forma más explícita y rica en características de crear entradas personalizadas.

A modo de ejemplo, ahora vamos a recorrer la migración de un filtro de moneda bidireccional:

<iframe width="100%" height="300" src="https://jsfiddle.net/chrisvfritz/6744xnjk/embedded/js,html,result" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

En su mayoría funciona bien, pero las actualizaciones de estado retrasados pueden causar un comportamiento extraño. Por ejemplo, haga clic en la pestaña `Resultado` e intente introducir `9.999` en una de esas entradas. Cuando la entrada pierde el foco, su valor se actualizará a `$10.00`. Sin embargo, al mirar el total calculado, verás que `9.999` es lo que se almacena en nuestros datos. La versión de la realidad que el usuario ve está fuera de sincronía!

Para comenzar la transición hacia una solución más robusta usando Vue 2,0, primero vamos a envolver este filtro en un nuevo componente `<currency-input>`:

<iframe width="100%" height="300" src="https://jsfiddle.net/chrisvfritz/943zfbsh/embedded/js,html,result" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

Esto nos permite agregar un comportamiento que un filtro por sí solo no podría encapsular, como seleccionar el contenido de una entrada en el foco. Ahora el siguiente paso será extraer la lógica de negocios del filtro. A continuación, extraemos todo en un objeto externo [objeto `currencyValidator`](https://GIST.github.com/chrisvfritz/5f0a639590d6e648933416f90ba7ae4e):

<iframe width="100%" height="300" src="https://jsfiddle.net/chrisvfritz/9c32kev2/embedded/js,html,result" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

Este aumento de la modularidad no solo hace que sea más fácil migrar a Vue 2, sino que también permite el análisis y el formateo de divisas:

- unidad probada aisladamente de su código Vue
- utilizado por otras partes de la aplicación, tal como validar la carga a un punto de conexión API

Teniendo este validador extraído, también hemos construido más cómodamente una solución más robusta. Las peculiaridades del estado han sido eliminadas y en realidad es imposible que los usuarios introduzcan algo incorrecto, similar a lo que intenta hacer la entrada de números nativos del navegador.


Sin embargo aún estamos limitados, por filtros y por Vue 1.0 en general, acabemos el _upgrade_ a Vue 2.0:

<iframe width="100%" height="300" src="https://jsfiddle.net/chrisvfritz/1oqjojjx/embedded/js,html,result" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

Has de notar que:

- Cada aspecto de nuestra entrada es más explícito, utilizando _hooks_ del ciclo de vida y eventos DOM en lugar del comportamiento oculto de los filtros bidireccionales.
- Ahora podemos usar el `v-model` directamente en nuestras entradas personalizadas, que no solo es más consistente con las entradas normales, sino que también significa que nuestro componente es amigable con Vuex.
- Puesto que ya no estamos usando las opciones de filtro que requieren un valor para retornar, nuestro trabajo de monedas podría ser hecho de forma asincrónica. Eso significa que si tuviéramos muchas aplicaciones que tuvieran que trabajar con divisas, podríamos refactorizar fácilmente esta lógica en un microservicio compartido.

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Ejecute <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> en tu código base para encontrar ejemplos de filtros usados en directivas como <code>v-model</code>. Si te pierdes alguna deberás ver <strong>console errors</strong>.</p>
</div>
{% endraw %}

## _Slots_

### _Slots_ Duplicados <sup>eliminado</sup>

Ya no está permitida la duplicación de `<slot>` dentro de una misma _template_. Cuando un _slot_ es dibujado estará: "_usado_" y no podrá ser dibujado otra vez dentro del mismo árbol. Si es necesario dibujar el mismo contenidoen varios lugares, pase ese contenido como una _prop_.

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Ejecuta tus pruebas <i>end-to-end</i> después de la actualización y buscar <strong>console warnings</strong> acerca de los <i>slots</i> duplicados <code>v-model</code>.</p>
</div>
{% endraw %}

### Atributo de Estilo `slot` <sup>eliminado</sup>

El contenido insertado con la etiqueta `<slot>` ya no preservan el atributo `slot`. Use un elemento que lo envuelva para asignarle estilo, o para uso avanzado, modifique el contenido insertado programáticamente usando [render functions](render-function.html).

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Ejecute <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> en tu código base para encontrar selectotes CSS apuntando a <i>slots</i> existentes(e.g. <code>[slot="my-slot-name"]</code>).</p>
</div>
{% endraw %}

### Atributo `keep-alive` <sup>reemplazado</sup>

`keep-alive` ya no es un atributo especial, sino más bien un componente contenedor, similar a `<transition>`. Por ejemplo:

``` html
<keep-alive>
  <component v-bind:is="view"></component>
</keep-alive>
```

Esto hace posible utilizar `<keep-alive>` en múltiples hijos condicionales:

``` html
<keep-alive>
  <todo-list v-if="todos.length > 0"></todo-list>
  <no-todos-gif v-else></no-todos-gif>
</keep-alive>
```

<p class="tip">When `<keep-alive>` has multiple children, they should eventually evaluate to a single child. Any child other than the first one will be ignored.</p>

Cuando se usa junto con `<transition>`, asegúrate de anidarlo adentro:

``` html
<transition>
  <keep-alive>
    <component v-bind:is="view"></component>
  </keep-alive>
</transition>
```

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Ejecute <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> en tu código base para encontrar <code>keep-alive</code> attributes.</p>
</div>
{% endraw %}

## Interpolación

### Interpolación con Atributos <sup>eliminado</sup>

La interpolación con atributos ya no es válida. Por ejemplo:

``` html
<button class="btn btn-{{ size }}"></button>
```

Debe ser sustituida por: 

``` html
<button v-bind:class="'btn btn-' + size"></button>
```

O por una propiedad existente en _computed_ o en _data_:

``` html
<button v-bind:class="buttonClasses"></button>
```

``` js
computed: {
  buttonClasses: function () {
    return 'btn btn-' + size
  }
}
```

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Ejecute <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> en tu código base para encontrar ejemplos de interpolación con atributos.</p>
</div>
{% endraw %}

### HTML Interpolation <sup>eliminado</sup>

Interpolaciones de HTML (`{% raw %}{{{ foo }}}{% endraw %}`) han sido eliminadas a favor de [`v-html` directive](../api/#v-html).

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Ejecute <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> en tu código base para encontrar interpolaciones de HTML.</p>
</div>
{% endraw %}

### One-Time Bindings <sup>reemplazado</sup>

One time bindings (`{% raw %}{{* foo }}{% endraw %}`) han sido remplazados por [`v-once` directive](../api/#v-once).

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Ejecute <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> en tu código base para encontrar los <u>One time bindings.</u> </p>
</div>
{% endraw %}

## Reactividad

### `vm.$watch` <sup>cambiado</sup>

Los _watchers_ creados con `vm.$watch` ahora se activan antes de que se vuelvan a ejecutar los componentes asociados. Esto permite actualizar el estado antes de que el componente se vuelva a dibujar, evitando así actualizaciones innecesarias. Por ejemplo, puede ver una _prop_ de componente y actualizar los datos propios del componente cuando cambia la _prop_.
Si anteriormente dependías de `vm.$watch` para hacer algo con el DOM después de que un componente se actualice, puedes hacerlo en el _hook_ del _lifecycle_ `updated`.
{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Ejecute sus pruebas, si tiene alguna. Las <strong>failed tests</strong> deben alertarte sobre el hecho de que un <i>watcher</i> confiaba en el comportamiento anterior.</p>
</div>
{% endraw %}

### `vm.$set` <sup>cambiado</sup>

`vm.$set` ahora es un alias de [`Vue.set`](../api/#Vue-set).

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Ejecute <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> en tu código base para encontrar ejemplos del uso absoleto.</p>
</div>
{% endraw %}

### `vm.$delete` <sup>cambiado</sup>

`vm.$delete` ahora es un alias de [`Vue.delete`](../api/#Vue-delete).

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Ejecute <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> en tu código base para encontrar ejemplos del uso absoleto.</p>
</div>
{% endraw %}

### `Array.prototype.$set` <sup>eliminado</sup>

Use `Vue.set` en su lugar.

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Ejecute <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> en tu código base para encontrar ejemplos de <code>.$set</code> en un arreglo. Si te pierdes alguno, deberías ver <strong> errores de consola </strong> del método que falta.</p>
</div>
{% endraw %}

### `Array.prototype.$remove` <sup>eliminado</sup>

Utilice `Array.prototype.splice` en su lugar. Por ejemplo:

``` js
methods: {
  removeTodo: function (todo) {
    var index = this.todos.indexOf(todo)
    this.todos.splice(index, 1)
  }
}
```

O mejor aún, pasar métodos de eliminación de un índice:

``` js
methods: {
  removeTodo: function (index) {
    this.todos.splice(index, 1)
  }
}
```

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Ejecute <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> en tu código base para encontrar ejemplos de <code>.$remove</code> en un arreglo. Si te pierdes alguno, <strong>console warnings</strong> serán emitidas.</p>
</div>
{% endraw %}

### `Vue.set` y `Vue.delete` in instancias Vue <sup>eliminados</sup>

`Vue.set` y `Vue.delete` ya no funcionan en instancias de Vue. Ahora es obligatorio declarar correctamente todas las propiedades reactivas de nivel superior en la opción de _data_. Si desea eliminar propiedades en una instancia de Vue o en su `$data`, asígnele null.

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Ejecute <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> en tu código base para encontrar ejemplos de <code>Vue.set</code> o <code>Vue.delete</code> en una instancia de Vue. Si te pierdes alguno, <strong>console warnings</strong> serán emitidas.</p>
</div>
{% endraw %}

### Replacing `vm.$data` <sup>eliminado</sup>

Ahora está prohibido reemplazar el $data raíz de una instancia de componente. Esto evita algunos casos importantes en el sistema de reactividad y hace que el estado del componente sea más predecible (especialmente con los sistemas de comprobación de tipos).

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Ejecute <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> en tu código base para encontrar ejemplos de <i>overwriting</i> <code>vm.$data</code>. Si te pierdes alguno, <strong>console warnings</strong> serán emitidas.</p>
</div>
{% endraw %}

### `vm.$get` <sup>eliminado</sup>

En su lugar, obtenga datos reactivos directamente.

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Ejecute <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> en tu código base para encontrar ejemplos de <code>vm.$get</code>. Si te pierdes alguno, verás <strong>console errors</strong>.</p>
</div>
{% endraw %}

## DOM-Focused Instance Methods

### `vm.$appendTo` <sup>eliminado</sup>

Utilice el _DOM API_ nativo:

``` js
myElement.appendChild(vm.$el)
```

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Ejecute <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> en tu código base para encontrar ejemplos de <code>vm.$appendTo</code>. Si te pierdes alguno, verás <strong>console errors</strong>.</p>
</div>
{% endraw %}

### `vm.$before` <sup>eliminado</sup>

Utilice el _DOM API_ nativo:

``` js
myElement.parentNode.insertBefore(vm.$el, myElement)
```

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Ejecute <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> en tu código base para encontrar ejemplos de <code>vm.$before</code>. Si te pierdes alguno, verás <strong>console errors</strong>. </p>
</div>
{% endraw %}

### `vm.$after` <sup>eliminado</sup>

Use the native DOM API:

``` js
myElement.parentNode.insertBefore(vm.$el, myElement.nextSibling)
```

Or if `myElement` is the last child:

``` js
myElement.parentNode.appendChild(vm.$el)
```

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Ejecute <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> en tu código base para encontrar ejemplos de <code>vm.$after</code>. Si te pierdes alguno, verás <strong>console errors</strong>. </p>
</div>
{% endraw %}

### `vm.$remove` <sup>eliminado</sup>

Use the native DOM API:

``` js
vm.$el.remove()
```

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Ejecute <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> en tu código base para encontrar ejemplos de <code>vm.$remove</code>. Si te pierdes alguno, verás <strong>console errors</strong>. </p>
</div>
{% endraw %}

## Meta Instance Methods

### `vm.$eval` <sup>eliminado</sup>

No real use. If you do happen to rely on this feature somehow and aren't sure how to work around it, post on [the forum](https://forum.vuejs.org/) for ideas.

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Ejecute <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> en tu código base para encontrar ejemplos de <code>vm.$eval</code>. Si te pierdes alguno, verás <strong>console errors</strong>. </p>
</div>
{% endraw %}

### `vm.$interpolate` <sup>eliminado</sup>

No real use. If you do happen to rely on this feature somehow and aren't sure how to work around it, post on [the forum](https://forum.vuejs.org/) for ideas.

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Ejecute <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> en tu código base para encontrar ejemplos de <code>vm.$interpolate</code>. Si te pierdes alguno, verás <strong>console errors</strong>. </p>
</div>
{% endraw %}

### `vm.$log` <sup>eliminado</sup>

Utilice [Vue Devtools](https://github.com/vuejs/vue-devtools) para una óptima experiencia de _debugging_.

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Ejecute <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> en tu código base para encontrar ejemplos de <code>vm.$log</code>. Si te pierdes alguno, verás <strong>console errors</strong>. </p>
</div>
{% endraw %}

## Instance DOM Options

### `replace: false` <sup>eliminado</sup>

Los componentes ahora reemplazan siempre el elemento al que están enlazados. Para simular el comportamiento de `replace: false`, puedes envolver el componente raíz con un elemento similar al que estás reemplazando. Por ejemplo:

``` js
new Vue({
  el: '#app',
  template: '<div id="app"> ... </div>'
})
```

Or with a render function:

``` js
new Vue({
  el: '#app',
  render: function (h) {
    h('div', {
      attrs: {
        id: 'app',
      }
    }, /* ... */)
  }
})
```

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Ejecute <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> en tu código base para encontrar ejemplos de <code>replace: false</code>.</p>
</div>
{% endraw %}

## Global Config

### `Vue.config.debug` <sup>eliminado</sup>

Ya no es necesario, ya que ahora las advertencias vienen con trazas de pila por defecto.

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Ejecute <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> en tu código base para encontrar ejemplos de <code>Vue.config.debug</code>.</p>
</div>
{% endraw %}

### `Vue.config.async` <sup>eliminado</sup>

_Async_ ahora es necesario para el rendimiento de renderización.

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Ejecute <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> en tu código base para encontrar ejemplos de <code>Vue.config.async</code>.</p>
</div>
{% endraw %}

### `Vue.config.delimiters` <sup>reemplazado</sup>

Esto se ha reelaborado como una [component-level option](../api/#delimiters). Esto le permite usar delimitadores alternativos dentro de la aplicación sin romper componentes _3rd-party_.


{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Ejecute <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> en tu código base para encontrar ejemplos de <code>Vue.config.delimiters</code>.</p>
</div>
{% endraw %}

### `Vue.config.unsafeDelimiters` <sup>eliminado</sup>

La interpolación de HTML ha sido [eliminada en favor de `v-html`](#HTML-Interpolation-removed).

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Ejecute <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> en tu código base para encontrar ejemplos de <code>Vue.config.unsafeDelimiters</code>. Después de esto, el _helper_ también encontrará instancias de interpolation de HTML de modo que puedes reemplazarlas con `v-html`.</p>
</div>
{% endraw %}

## Global API

### `Vue.extend` con `el` <sup>eliminado</sup>

La opción _el_ ya no se puede utilizar en `Vue.extend`. Solamente es válido como una opción en la creación de la instancia.

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Ejecute su suite o aplicación de prueba end-to-end después de actualizar y busque <strong>console warnings</strong> sobre la opción <code>el</code> con <code>Vue.extend</code>.</p>
</div>
{% endraw %}

### `Vue.elementDirective` <sup>eliminado</sup>

Utilice en su lugar: _components_

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Ejecute <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> en tu código base para encontrar ejemplos de <code>Vue.elementDirective</code>.</p>
</div>
{% endraw %}

### `Vue.partial` <sup>eliminado</sup>

Los _partials_ se han eliminado en favor de un flujo de datos más explícito entre los componentes, utilizando _props_. A menos que estés utilizando un _partial_ en un área de rendimiento crítico, la recomendación es utilizar un [normal component](components.html) en su lugar. Si enlazó dinámicamente el `name` de un _partial_, puede utilizar un [dynamic component](components.html#Dynamic-Components).

Si estás utilizando _partials_ en una parte de rendimiento crítico de la aplicación, a continuación, debe actualizar a [functional components](render-function.html#Functional-Components). Deben estar en un archivo JS/JSX (en lugar de en un archivo `.vue`) y son _sin estado_ y _sin instancia_, como los _partials_. Esto hace que el renderizado sea extremadamente rápido.

Una ventaja de _components_ funcionales sobre _partials_ es que pueden ser mucho más dinámicos, ya que le conceden acceso a toda la potencia de JavaScript. Sin embargo, hay un costo para este poder. Si nunca has usado un _framework_ de _components_ con funciones de renderización antes, puede tardar un poco más en aprenderse.

{% raw %}
<div class="upgrade-path">
  <h4>Upgrade Path</h4>
  <p>Ejecute <a href="https://github.com/vuejs/vue-migration-helper">migration helper</a> en tu código base para encontrar ejemplos de <code>Vue.partial</code>.</p>
</div>
{% endraw %}
