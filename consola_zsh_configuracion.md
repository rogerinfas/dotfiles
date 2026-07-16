# 💻 Configuración de la Consola (Zsh + Autocompletado + Diseño)

La terminal en tu sistema está configurada para tener un aspecto moderno (prompt personalizado) y sugerencias inteligentes de comandos (autocompletado según el historial y resaltado de sintaxis). A continuación se documenta cómo está construido este setup.

---

## 1. Terminal Base y Shell (Kitty + Zsh)
El emulador de terminal que estás usando es **Kitty**, el cual está configurado para no usar el shell por defecto (Bash), sino arrancar directamente con **Zsh**, que es el motor donde funcionan todos estos plugins.

**Archivo:** `~/.config/kitty/kitty.conf`
```conf
shell /usr/bin/zsh
```

---

## 2. El motor: Oh-My-Zsh y Plugins de Sugerencias
Toda la magia del autocompletado y colores al escribir viene del framework **Oh-My-Zsh** y dos plugins específicos. 

**Archivo de configuración:** `~/.config/zshrc/20-customization`

```bash
# Plugins habilitados de Oh-My-Zsh
plugins=(
    git
    sudo
    web-search
    archlinux
    zsh-autosuggestions       # <- SUGERENCIAS (sombra gris al escribir)
    zsh-syntax-highlighting   # <- COLORES (verde si el comando existe, rojo si no)
    fast-syntax-highlighting
    copyfile
    copybuffer
    dirhistory
)

# Iniciar Oh-My-Zsh
source $ZSH/oh-my-zsh.sh
```

### ¿Cómo funcionan los plugins clave?
- **`zsh-autosuggestions`**: Analiza tu historial de comandos (`~/.zsh_history`). Cuando empiezas a escribir, busca el comando más parecido que ya hayas usado antes y te lo muestra en una sombra gris tenue. **Si presionas `Flecha Derecha` (o `Tab` según el setup), el comando se autocompleta.**
- **`zsh-syntax-highlighting`**: Pinta los comandos de verde si están bien escritos y existen en el sistema, o en rojo si has cometido un error de tipeo.

---

## 3. El Diseño Visual (Prompt) con Oh-My-Posh
El diseño moderno del texto antes del cursor (donde dice tu usuario, la ruta de la carpeta, la flecha y el estado de Git) no viene de Zsh, sino de **Oh-My-Posh**, un motor de diseño de prompts multiplataforma.

**Archivo de configuración:** `~/.config/zshrc/20-customization`
```bash
# Prompt inicializado por Oh-My-Posh usando el tema "zen"
eval "$(oh-my-posh init zsh --config $HOME/.config/ohmyposh/zen.toml)"
```

El archivo que define los colores y forma de las flechas del prompt es:
`~/.config/ohmyposh/zen.toml`

---

## 🚀 ¿Cómo replicarlo en otra PC (desde cero)?

Si quisieras instalar todo esto en otra computadora o distribución Linux, estos serían los pasos lógicos:

1. Instalar Zsh y Kitty: `sudo pacman -S zsh kitty`
2. Instalar Oh-My-Zsh usando su script oficial:
   ```bash
   sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
   ```
3. Clonar los plugins de autocompletado y sintaxis en la carpeta de Oh-My-Zsh:
   ```bash
   git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
   git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
   ```
4. Editar `~/.zshrc` y añadir los plugins `(git zsh-autosuggestions zsh-syntax-highlighting)`.
5. Instalar [Oh-My-Posh](https://ohmyposh.dev/docs/installation/linux) para tener el diseño del prompt de colores y flechas.
