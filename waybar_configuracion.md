# 📊 Configuración y Estilos de Waybar

Esta es la configuración extraída de la Waybar (tema `ml4w-glass-center`), documentando los márgenes, bordes y estilos actuales para tener un respaldo y facilitar futuras modificaciones.

---

## 1. Márgenes exteriores y Ubicación
Controla la distancia de la barra respecto a los bordes de la pantalla.
**Archivo:** `~/.config/waybar/themes/ml4w-glass-center/config`

```json
{
    // ...
    "margin-top": 1,
    "margin-bottom": 0,
    "margin-left": 1,
    "margin-right": 1,
    "spacing": 0,
    // ...
}
```
> **Nota:** La barra está configurada para pegarse casi por completo a los bordes de la pantalla (1 píxel de separación). Si quieres que la barra sea "flotante", habría que aumentar estos márgenes (por ejemplo, a `5` o `10`).

---

## 2. Bordes, Márgenes internos y Estilos Visuales
Controla la estética de la barra "Glass", incluyendo los bloques de los módulos (izquierdo, central y derecho).
**Archivo:** `~/.config/waybar/themes/ml4w-glass/style.css`

```css
.modules-left, .modules-center, .modules-right {
    /* Esquinas redondeadas (Suaves) */
    border-radius: 12px;
    
    /* Distancia entre bloques (separación) y contra el borde externo */
    margin: 5px;
    
    /* Espacio interno entre el borde y el contenido de los módulos */
    padding: 0px;
    
    /* Efecto Glass / Transparencia */
    opacity: 0.8;
    
    /* Fondo transparente-oscuro con borde dinámico de Matugen */
    background-color: @surface;
    background: radial-gradient(circle at 50% 250%, alpha(darker(@surface),0.9), alpha(@surface_dim,0.9)) padding-box,
        linear-gradient(@primary,@on_primary) border-box;
    border: 2px solid transparent;	
}
```

### Explicación del Estilo:
- **Esquinas (`border-radius`):** `12px` le da un aspecto moderno y curvado sin ser un óvalo completo.
- **Separación (`margin`):** El `margin: 5px` crea espacios vacíos entre el módulo izquierdo, el central y el derecho, dando el efecto de "múltiples píldoras" en lugar de una barra continua.
- **Borde de Color (`border` y `background`):** El borde real tiene `2px` pero es transparente. El efecto de borde de color se logra usando el gradiente de Matugen (`@primary`, `@on_primary`) pintado en el `border-box`, lo que permite que el color cambie dinámicamente con el fondo de pantalla.

---

## ¿Cómo aplicar cambios futuros?
Si modificamos alguno de estos archivos en el futuro, podemos aplicar los cambios inmediatamente recargando la Waybar con:
```bash
killall waybar; ~/.config/waybar/launch.sh
```

---

## 3. Iconos de Aplicaciones Abiertas (Centro de la Barra)
En el centro de la barra se muestran los iconos de las aplicaciones que tienes abiertas en cada escritorio. Esto **no es un módulo de "barra de tareas" (taskbar) tradicional**, sino una función avanzada del propio módulo de escritorios de Hyprland.

**Archivo:** `~/.config/waybar/modules.json` (busca `"hyprland/workspaces"`)

### ¿Cómo funciona?
Se utiliza la variable `{windows}` en el formato del escritorio, combinada con la función `window-rewrite` de Waybar. Esta función lee la "clase" (class) de la ventana abierta y la reemplaza por un icono específico de una fuente de iconos (como FontAwesome).

```json
"hyprland/workspaces": {
    "format": "{id} {windows}", // Muestra el número del escritorio y luego los iconos
    "format-window-separator": " ", // Separación entre múltiples iconos
    "window-rewrite-default": "", // Icono por defecto si la app no está en la lista inferior
    
    // Diccionario de reemplazo: "class<NombreApp>": "Icono"
    "window-rewrite": {
      "class<kitty>": "",
      "class<Brave-browser>": "",
      "class<firefox>": "",
      "class<spotify>": "",
      "class<code-oss>": "󰨞",
      "class<discord>": "",
      "class<thunar>": "󰉋"
      // ... más aplicaciones
    }
}
```

### ¿Cómo añadir un icono nuevo para una aplicación?
Si instalas una aplicación nueva y su icono se muestra como el icono por defecto (``):
1. Abre la aplicación.
2. Abre una terminal y ejecuta `hyprctl clients`.
3. Busca tu aplicación en la lista y copia el valor exacto que dice `class:`.
4. Abre `~/.config/waybar/modules.json`.
5. Ve a `"window-rewrite"` y añade una nueva línea con ese nombre de clase y el icono que quieras usar (puedes buscar iconos en páginas como [NerdFonts Cheat Sheet](https://www.nerdfonts.com/cheat-sheet)).
   *Ejemplo:* `"class<tu_app>": "🚀",`
6. Reinicia Waybar (`killall waybar; ~/.config/waybar/launch.sh`).
