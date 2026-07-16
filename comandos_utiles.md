# 🐧 Comandos Útiles - Guía de Configuración

Esta es una guía de referencia rápida con los comandos y ubicaciones de configuración para tu sistema con **Hyprland** (ML4W).

---

## 🖥️ Gestión del Dock (nwg-dock-hyprland)

El dock está configurado de manera optimizada (ejecución directa con `exec` para no consumir recursos innecesarios).

### Desactivar/Quitar el Dock
Para ocultarlo por completo y que no consuma recursos:
```bash
touch ~/.config/ml4w/settings/dock-disabled && killall nwg-dock-hyprland
```

### Activar el Dock con Ocultamiento Automático (Autohide)
Para volver a activarlo de manera que solo aparezca al pasar el mouse por el borde inferior:
```bash
rm -f ~/.config/ml4w/settings/dock-disabled && touch ~/.config/ml4w/settings/dock-autohide && nohup ~/.config/nwg-dock-hyprland/launch.sh >/dev/null 2>&1 &
```

### Activar el Dock Fijo (Siempre Visible)
Para dejarlo siempre visible en pantalla:
```bash
rm -f ~/.config/ml4w/settings/dock-disabled ~/.config/ml4w/settings/dock-autohide && nohup ~/.config/nwg-dock-hyprland/launch.sh >/dev/null 2>&1 &
```

---

## 🔊 Bluetooth (BlueZ)

El servicio bluetooth del sistema.

* **Activar y habilitar al encender el PC:**
  ```bash
  sudo systemctl enable --now bluetooth
  ```
* **Ver estado del servicio bluetooth:**
  ```bash
  systemctl status bluetooth
  ```

---

## 📊 Barra de Estado (Waybar)

Configuración de la barra superior.

* **Archivo de configuración (para cambiar márgenes `margin-top/left/right` y módulos):**
  `~/.config/waybar/themes/ml4w-glass-center/config`
* **Archivo global de módulos (para iconos de escritorios, formato, etc.):**
  `~/.config/waybar/modules.json`
* **Archivo de estilos CSS (para cambiar el padding, márgenes internos y colores):**
  `~/.config/waybar/themes/ml4w-glass/style.css`
* **Comando para reiniciar y aplicar cambios de inmediato:**
  ```bash
  killall waybar; ~/.config/waybar/launch.sh
  ```

### 🖥️ Comportamiento de Escritorios en Waybar
* **Clic Izquierdo:** Activa y te mueve al monitor correspondiente del escritorio seleccionado (`"on-click": "activate"`, `"move-to-monitor": true`).
* **Navegación con rueda (Scroll):**
  * Rueda hacia abajo: Siguiente escritorio (`hyprctl dispatch workspace e+1`)
  * Rueda hacia arriba: Anterior escritorio (`hyprctl dispatch workspace e-1`)
* *Nota:* Requiere el paquete `waybar-git` para que la comunicación IPC con Hyprland v0.55+ funcione correctamente.

---

## 🖥️ Monitores y Entorno

* **Archivo de configuración de monitores:**
  `~/.config/hypr/monitors.lua`
* **Recargar configuración de Hyprland en tiempo real:**
  ```bash
  hyprctl reload
  ```
* **Ver información técnica de los monitores conectados:**
  ```bash
  hyprctl monitors
  ```

---

## ⌨️ Atajos de Teclado (Keybindings)

Los atajos del teclado ahora están adaptados al estilo de **JaKooLit**.

* **Ubicación de tus atajos activos:**
  `~/.config/hypr/conf/keybindings/default.lua`
* **Copia de seguridad original (ML4W original):**
  `~/.config/hypr/conf/keybindings/default.lua.bak`

---

## ⌨️ Distribución del Teclado (Keyboard Layout)

Para configurar el idioma y la distribución de tu teclado físico:

* **Archivo de configuración:**
  `~/.config/hypr/input.lua`
* **Cambiar la distribución:**
  Dentro de ese archivo, busca la línea `kb_layout` y edítala según corresponda:
  * Español Latinoamericano (Redragon Fizz Pro): `"latam"`
  * Español de España: `"es"`
  * Inglés de Estados Unidos: `"us"`

---

## 📸 Capturas de Pantalla (Screenshots)

Las capturas de pantalla se guardan automáticamente y se **copian al portapapeles** para que puedas pegarlas directamente en Discord, Telegram, el navegador, etc.

### Atajos de Teclado
| Atajo | Acción |
|-------|--------|
| `SUPER + SHIFT + S` | Captura de área (selecciona con el mouse) |
| `SUPER + SHIFT + PRINT` | Captura de área (igual que el anterior) |
| `SUPER + PRINT` | Captura de pantalla completa |

### Carpeta de guardado
Las capturas se guardan en:
```
~/Imágenes/screenshots/
```

### Cómo funciona el portapapeles
El script de captura fue modificado para:
1. Guardar la imagen en un archivo temporal en `/tmp/`.
2. Copiar la imagen al portapapeles de Wayland con el MIME type correcto (`image/png`).
3. Mover el archivo a `~/Imágenes/screenshots/` con el nombre `screenshot_YYYYMMDD_HHMMSS.png`.

> **Nota:** Si la captura no se pega en alguna aplicación, verifica que `wl-clipboard` esté instalado:
> ```bash
> which wl-copy
> ```

### Script de capturas
* **Ubicación del script:**
  `~/.config/hypr/scripts/screenshot.sh`

---

## 🎨 Iconos Dinámicos (Papirus + Matugen)

Los iconos de carpetas del explorador de archivos cambian automáticamente de color cada vez que cambias el fondo de pantalla, sincronizándose con la paleta de colores generada por Matugen.

### ¿Cómo funciona?

```
Cambio de fondo  →  Matugen genera colors.css  →  ml4w-papirus-color-sync
→  Lee @primary de colors.css  →  Encuentra color Papirus más cercano
→  papirus-folders aplica el color  →  Iconos actualizados
```

### Archivos clave
| Archivo | Propósito |
|---------|-----------|
| `~/.config/ml4w/scripts/ml4w-papirus-color-sync` | Script que sincroniza el color primario de Matugen con Papirus |
| `~/.config/ml4w/scripts/ml4w-wallpaper` | Script principal de wallpaper (tiene el hook integrado) |
| `/etc/sudoers.d/papirus-folders` | Regla que permite ejecutar papirus-folders sin contraseña |

### Tema de iconos activo
* **Tema instalado:** `Papirus-Dark`
* **Colores disponibles en Papirus:** adwaita, black, blue, bluegrey, breeze, brown, carmine, cyan, darkcyan, deeporange, green, grey, indigo, magenta, nordic, orange, palebrown, paleorange, pink, red, teal, violet, white, yaru, yellow

### Forzar sincronización manual
Si quieres re-sincronizar los iconos con el fondo actual sin cambiar el wallpaper:
```bash
bash ~/.config/ml4w/scripts/ml4w-papirus-color-sync
```

### Cambiar el tema de iconos manualmente a un color específico
```bash
sudo papirus-folders -C cyan --theme Papirus-Dark
sudo papirus-folders -C cyan --theme Papirus
```

### Restaurar color original de Papirus (azul por defecto)
```bash
sudo papirus-folders -C blue --theme Papirus-Dark
sudo papirus-folders -C blue --theme Papirus
```

> **Nota:** Los cambios se aplican al reabrir el explorador de archivos (Thunar/Nautilus). El sistema es completamente automático y no requiere contraseña.
