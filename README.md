# 🌌 Space Hub UI Library (v2.1.0)

**Space Hub** es una biblioteca de interfaz de usuario (UI Library) moderna, ligera y multiplataforma escrita en Lua para Roblox. Está optimizada para funcionar tanto en **PC** como en **dispositivos móviles** (Android/iOS con ejecutores como Delta, Codex, Arceus X, etc.), ofreciendo soporte táctil nativo, respuesta háptica, guardado automático de configuraciones y módulos extensibles.

---

## ✨ Características Principales

* **Diseño Multiplataforma:** Soporte nativo para pantallas táctiles, botón flotante móvil (`Show Menu`), redimensionamiento mediante agarre (`grip`) y modo píldora minimizada.
* **10 Temas Integrados:** `Space`, `Titanium`, `Graphite`, `Cobalt`, `Emerald`, `Crimson`, `Violet`, `Steel`, `Obsidian` y `Amber` con cálculo automático de contraste.
* **Sistema de Guardado y Auto-Execute:** Guardado y carga automática de configuraciones en formato `.json` mediante `Flags`, además de gestión integrada de carpetas `autoexec`.
* **Buscador en Tiempo Real:** Barra de búsqueda que filtra pestañas, botones y opciones ignorando acentos y mayúsculas.
* **Componentes Avanzados:** Incluye bloques de código copiables, entradas multilínea, botones flotantes arrastrables y visor de jugadores (`Spectate`).
* **Sistema de Extensiones (`Blocks`):** Permite modularizar el menú y añadir nuevos componentes o ganchos (`OnWindow`) dinámicamente.

---

## 🚀 Inicio Rápido

Carga la biblioteca directamente en tu script utilizando `loadstring`:

```lua
local SpaceHub = loadstring(game:HttpGet("https://githubusercontent.com"))()
```

---

## 🪟 Crear una Ventana (`CreateWindow`)

```lua
local Window = SpaceHub:CreateWindow({
    Name = "Space Hub",
    Theme = "Space", -- Opciones: Space, Titanium, Graphite, Cobalt, Emerald, Crimson, Violet, Steel, Obsidian, Amber
    Width = 640,
    Height = 400,
    ToggleKey = Enum.KeyCode.RightShift,
    MobileButton = true, -- Muestra el botón "Show Space Hub" al ocultar el menú en móvil
    LoadingScreen = true,
    LoadingDuration = 3,
    ConfigurationSaving = {
        Enabled = true,
        FolderName = "SpaceHub",
        FileName = "config"
    },
    Discord = {
        Enabled = false,
        Invite = "tu_invitacion", -- Solo el código, sin discord.gg/
        RememberJoins = true
    },
    KeySystem = false,
    KeySettings = {
        Title = "Sistema de Llaves",
        Subtitle = "Verificación requerida",
        Note = "Obtén tu llave en nuestro servidor.",
        FileName = "Key",
        SaveKey = true,
        GrabKeyFromSite = false, -- Si es true, 'Key' debe contener URLs raw con la clave
        Key = { "12345", "SPACE-KEY" },
        KeyLink = "https://tu-enlace-de-key.com"
    }
})
```

---

## 📑 Pestañas y Secciones

```lua
-- Crear una pestaña (el icono es opcional: puede ser rbxassetid://... o un número)
local MainTab = Window:CreateTab("Principal", 125106043223530)

-- Crear un separador de sección
local Section = MainTab:CreateSection("Controles Básicos")
Section:Set("Nuevo nombre de sección") -- Actualizar texto
```

---

## 🧩 Elementos de Interfaz

### 1. Etiqueta y Párrafo (`CreateLabel` / `CreateParagraph`)
```lua
local Label = MainTab:CreateLabel("Estado: Conectado", 125106043223530)
Label:Set("Estado: En espera")

local Paragraph = MainTab:CreateParagraph({
    Title = "Información del Script",
    Content = "Bienvenido a la interfaz oficial de Space Hub."
})
Paragraph:Set({ Title = "Actualización", Content = "Versión 2.1.0 cargada." })
```

### 2. Botón (`CreateButton`)
```lua
local Button = MainTab:CreateButton({
    Name = "Ejecutar Acción",
    Callback = function()
        print("¡Botón presionado!")
    end
})
Button:Set("Nuevo Texto")
```

### 3. Interruptor (`CreateToggle`)
```lua
local Toggle = MainTab:CreateToggle({
    Name = "Modo Turbo",
    CurrentValue = false,
    Flag = "Toggle_Turbo", -- Necesario para ConfigurationSaving
    Callback = function(Value)
        print("Estado del toggle:", Value)
    end
})
Toggle:Set(true)
```

### 4. Deslizador (`CreateSlider`)
```lua
local Slider = MainTab:CreateSlider({
    Name = "Velocidad de Caminata",
    Range = { 16, 200 },
    Increment = 1,
    Suffix = "WS",
    CurrentValue = 16,
    Flag = "Slider_WalkSpeed",
    Callback = function(Value)
        local char = game.Players.LocalPlayer.Character
        if char and char:FindFirstChildOfClass("Humanoid") then
            char:FindFirstChildOfClass("Humanoid").WalkSpeed = Value
        end
    end
})
Slider:Set(50)
```

### 5. Lista Desplegable (`CreateDropdown`)
Soporta selección única o múltiple (`MultipleOptions = true`):
```lua
local Dropdown = MainTab:CreateDropdown({
    Name = "Seleccionar Objetivo",
    Options = { "Opción A", "Opción B", "Opción C" },
    CurrentOption = { "Opción A" },
    MultipleOptions = false,
    Flag = "Dropdown_Target",
    Callback = function(Options)
        -- 'Options' siempre devuelve una tabla con las opciones seleccionadas
        print("Seleccionado:", table.concat(Options, ", "))
    end
})
Dropdown:Refresh({ "Nueva 1", "Nueva 2" })
Dropdown:Set({ "Nueva 2" })
```

### 6. Entradas de Texto y Números
```lua
-- Input estándar
local Input = MainTab:CreateInput({
    Name = "Nombre de jugador",
    PlaceholderText = "Escribe aquí...",
    CurrentValue = "",
    RemoveTextAfterFocusLost = false,
    Flag = "Input_Player",
    Callback = function(Text)
        print("Texto ingresado:", Text)
    end
})

-- Input numérico con límites (PowerInput)
local PowerInput = MainTab:CreatePowerInput({
    Name = "Potencia de Salto",
    CurrentValue = 50,
    Min = 0,
    Max = 500,
    AllowDecimal = true,
    AllowNegative = false,
    Flag = "Power_Jump",
    Callback = function(Number)
        print("Valor numérico:", Number)
    end
})

-- Entrada rápida con botón de envío (MessageInput)
MainTab:CreateMessageInput({
    Name = "Enviar Mensaje",
    ButtonText = "Enviar",
    PlaceholderText = "Escribe un mensaje...",
    ClearOnSend = true,
    NumbersOnly = false,
    Callback = function(Text, AsNumber)
        print("Enviado:", Text)
    end
})
```

### 7. Editor y Bloque de Código (`CreateCodeInput` / `CreateCodeBlock`)
```lua
-- Editor multilínea para ejecutar scripts
local CodeEditor = MainTab:CreateCodeInput({
    Name = "Ejecutor Rápido",
    Height = 160,
    PlaceholderText = "Pega tu código Lua aquí...",
    Callback = function(Code)
        print("Código guardado:", #Code, "caracteres")
    end
})

-- Bloque de solo lectura con botón "Copy"
local CodeViewer = MainTab:CreateCodeBlock({
    Title = "Loadstring Oficial",
    Height = 120,
    Code = 'loadstring(game:HttpGet("https://ejemplo.com"))()'
})
```

### 8. Selector de Tecla (`CreateKeybind`) y Color (`CreateColorPicker`)
```lua
local Keybind = MainTab:CreateKeybind({
    Name = "Tecla de Vuelo",
    CurrentKeybind = "F",
    HoldToInteract = false,
    Flag = "Key_Fly",
    Callback = function(State)
        print("Keybind activado:", State)
    end
})

local ColorPicker = MainTab:CreateColorPicker({
    Name = "Color ESP",
    Color = Color3.fromRGB(135, 148, 255),
    Flag = "Color_ESP",
    Callback = function(Color)
        print("Nuevo color RGB:", Color)
    end
})
```

---

## 🛠️ Componentes Especiales

### Botones Flotantes Arrastrables (`CreateFloatingButtons`)
Crea botones en pantalla ideales para ejecutar acciones rápidas en dispositivos móviles:
```lua
local FloatBtns = MainTab:CreateFloatingButtons({
    Name = "Botones Flotantes",
    AmountName = "Cantidad de botones",
    Count = 3,
    Max = 10,
    Size = 56,
    Names = { "TP", "Hit", "Fly" },
    Flag = "Float_Controls",
    Callback = function(Index, ButtonName)
        print("Botón flotante tocado:", Index, ButtonName)
    end
})
```

### Espectador de Jugadores (`CreateSpectate`)
Genera una tarjeta interactiva con avatar y botones `<` / `>` para observar a otros jugadores del servidor:
```lua
local Spectate = MainTab:CreateSpectate({
    Name = "Observar Jugadores",
    Callback = function(TargetPlayer, IsSpectating)
        if IsSpectating then
            print("Observando a:", TargetPlayer.Name)
        else
            print("Cámara restaurada")
        end
    end
})
```

---

## 🔔 Notificaciones y Control de Ventana

```lua
-- Enviar notificación
SpaceHub:Notify({
    Title = "Aviso del Sistema",
    Content = "La configuración se ha aplicado correctamente.",
    Image = 125106043223530,
    Duration = 5
})

-- Métodos de control de ventana
Window:SetTheme("Obsidian")
Window:SetMenuType("horizontal") -- "vertical" o "horizontal"
Window:Minimize(true)
Window:Hide()
Window:Show()
Window:Destroy()
```

---

## 🔌 Sistema de Extensiones (`Blocks` y `Extend`)

Puedes registrar nuevos elementos personalizados en la API o enganchar código cuando se crea la ventana:

```lua
-- Añadir un nuevo método a todas las pestañas
SpaceHub:Extend("CreateCustomDivider", function(ctx, options)
    return ctx.Api:CreateSection(options.Text or "---")
end)

-- Ejecutar código automáticamente al crear la ventana
SpaceHub:OnWindow(function(Window, Util)
    local ExtraTab = Window:CreateTab("Extras")
