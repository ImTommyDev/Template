# 🚀 Template React + .NET con Configuración Avanzada

Este es un template de proyecto que combina **React TypeScript** con un backend en **.NET**. Está diseñado para ofrecer una configuración optimizada con formateo automático, pre-commits y scripts para facilitar el desarrollo.

---

## 📁 Estructura del Proyecto

```
Template/
│-- template.client/       # Frontend (React + TypeScript)
│-- Template.Server/       # Backend (ASP.NET Core)
│-- .editorconfig          # Configuración de formateo para C#
│-- .gitignore             # Archivos a ignorar en Git
│-- README.md              # Documentación del proyecto
```

---

## 🔧 Configuración y Características

### 📌 **Pre-commit con Husky**
Este template incluye un pre-commit hook con Husky que automatiza las siguientes tareas:

```sh
#!/bin/sh
. "$(dirname "$0")/_/husky.sh"

# Formateo automático de archivos en staging con Prettier
prettier $(git diff --cached --name-only --diff-filter=ACMR | sed 's| |\\ |g') --write --ignore-unknown

git update-index --again

echo "🔹 Ejecutando ESLint en frontend..."
cd template.client && npm run lint

echo "🔹 Ejecutando C# Formatter en backend..."
cd ../template.server && dotnet format
```

Esto garantiza que el código siga las normas de estilo antes de cada commit.

---

### 📦 **Scripts Disponibles**

El archivo `package.json` del frontend incluye scripts útiles:

```json
"scripts": {
  "dev": "vite",
  "build": "tsc -b && vite build",
  "lint": "eslint .",
  "preview": "vite preview",
  "start": "concurrently \"npm run frontend\" \"npm run backend\"",
  "frontend": "vite",
  "backend": "cd ../Template.Server && dotnet run",
  "prepare": "husky install"
}
```

- `npm run start`: Inicia **frontend y backend simultáneamente**.
- `npm run lint`: Ejecuta ESLint en el frontend.
- `npm run frontend`: Inicia el servidor de desarrollo de React.
- `npm run backend`: Inicia el backend de .NET.

---

### 🎨 **Formato Automático**

El proyecto usa:

- **Prettier** para formatear archivos en el frontend (`.tsx`, `.ts`, `.js`).
- **C# Formatter (dotnet format)** para formatear el código del backend.
- Un archivo `.editorconfig` con reglas de formato para C#:

```ini
[*.cs]
indent_style = space
indent_size = 4
end_of_line = crlf
charset = utf-8
trim_trailing_whitespace = true
insert_final_newline = true
```

---

### 📡 **Configuración del Backend**

El backend en **ASP.NET Core** ya está configurado con **Serilog** para logs y Swagger para documentación de la API:

```csharp
using Serilog;

var builder = WebApplication.CreateBuilder(args);

// Configuración de logs
Log.Logger = new LoggerConfiguration()
    .WriteTo.Console()
    .CreateLogger();

builder.Host.UseSerilog();

// Servicios y configuración de Swagger
builder.Services.AddControllers();
builder.Services.AddEndpointsApiExplorer();
builder.Services.AddSwaggerGen();

var app = builder.Build();

app.UseDefaultFiles();
app.UseStaticFiles();

if (app.Environment.IsDevelopment())
{
    app.UseSwagger();
    app.UseSwaggerUI();
}

app.UseHttpsRedirection();
app.UseAuthorization();
app.MapControllers();
app.MapFallbackToFile("/index.html");

app.Run();
```

---

## 🚀 **Cómo Usarlo**

1. **Clonar el repositorio**
   ```sh
   git clone https://github.com/ImTommyDev/Template.git
   cd Template
   ```

2. **Instalar dependencias del frontend**
   ```sh
   cd template.client
   npm install
   ```

3. **Iniciar el proyecto**
   ```sh
   npm run start
   ```

---

## 🔥 **Conclusión**

Este template proporciona un punto de partida sólido para proyectos con **React + .NET**, con una configuración optimizada y herramientas que mejoran la productividad. 🚀

Si tienes ideas para mejorarlo, ¡las contribuciones son bienvenidas! 😃

