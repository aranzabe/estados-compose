# 2.- Estados en Jetpack Compose

## **1️⃣ Qué es un estado en Compose**

- Un **estado** es cualquier valor que puede cambiar a lo largo del tiempo y que **afecta la UI**.
- Compose **vuelve a recomponer** automáticamente los elementos que dependen de ese estado cuando este cambia.
- Los estados se usan con **`remember`** y `mutableStateOf` para que Compose los observe.

## **2️⃣ Ejemplo básico de estado**

```kotlin
@Composable
fun EjemploEstado() {
    var contador by remember { mutableStateOf(0) } // Estado observable
    Button(onClick = { contador++ }) {
        Text("Clicks: $contador")
    }
}
```

✅ Cada vez que `contador` cambia, **Compose vuelve a dibujar** la parte de la UI que depende de `contador`.

---

## **3️⃣ Tipos de estado más comunes**

### **3.1 Local (in-memory)**

- Usando `remember` y `mutableStateOf`.
- Se pierde al rotar la pantalla o recrear la actividad.

```kotlin
var texto by remember { mutableStateOf("") }
```

### **3.2 Persistente a recomposiciones y rotaciones**

- Usando `rememberSaveable`:

```kotlin
var texto by rememberSaveable { mutableStateOf("") }
```

Ideal para **guardar el estado tras rotaciones de pantalla**.

### **3.3 Contextual**

- `LocalContext.current` permite acceder al contexto de la Activity o Composable.

```kotlin
val contexto = LocalContext.current
```

## **4️⃣ Integración con Inputs y Buttons**

En el ejemplo:

```kotlin
var cont by remember { mutableStateOf(0) }
var caja by remember { mutableStateOf("") }
```

- `cont` se actualiza con el botón: `Button(onClick = { cont++ })`
- `caja` se actualiza con el `TextField`: `onValueChange = { caja = it }`
- Cada vez que uno de estos cambia, **la columna se recompone** y los `Text` muestran los valores actuales.

## **5️⃣ Ejemplo completo explicado**

```kotlin
@Composable
fun Estados() {
    val contexto = LocalContext.current

    // Estados observables
    var cont by remember { mutableStateOf(0) }
    var caja by remember { mutableStateOf("") }

    Column(modifier = Modifier.fillMaxSize()) {
        Spacer(modifier = Modifier.height(20.dp))

        Column(
            modifier = Modifier
                .fillMaxSize()
                .background(Color.Green),
            horizontalAlignment = Alignment.CenterHorizontally
        ) {
            // Input que actualiza el estado
            TextField(
                value = caja,
                label = { Text("Escribe algo") },
                onValueChange = { caja = it },
                modifier = Modifier.fillMaxWidth()
            )

            // Botón que actualiza el estado
            Button(onClick = { cont++ }) {
                Text("Púlsame")
            }

            // Los textos se recomponen automáticamente al cambiar los estados
            Text(text = "Valor de contador: $cont")
            Text(text = "Valor de la caja: $caja")
        }
    }
}
```

**🔹 Qué sucede paso a paso:**

1. El usuario escribe algo → `caja` cambia → Compose recompone la UI que depende de `caja`.
2. El usuario pulsa el botón → `cont` incrementa → Compose recompone los `Text` que muestran `cont`.

## **6️⃣ Resumen de buenas prácticas**

- Usar **`var estado by remember { mutableStateOf(valorInicial) }`** para valores internos de la UI.
- Usar **`rememberSaveable`** si quieres mantener el estado tras rotaciones de pantalla.
- Mantener los estados **lo más cerca posible de donde se usan**, pero no duplicarlos innecesariamente.
- Cada cambio en un estado **dispara recomposición solo de los elementos que dependen de él**, no de toda la pantalla.