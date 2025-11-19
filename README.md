# Sistema de Registro de Requerimientos - Empresa de Seguros

Frontend desarrollado con Vue.js 3 y Tailwind CSS para el registro y gestión de requerimientos de una empresa de seguros.

## Características

- Registro de diferentes tipos de requerimientos:
  - Reclamos
  - Solicitudes
  - Consultas
  - Quejas
  - Sugerencias

- Campos específicos según el tipo de requerimiento
- Sistema de prioridades (Baja, Media, Alta, Urgente)
- Interfaz responsive y moderna
- Lista de requerimientos registrados con información detallada

## Instalación

1. Instalar dependencias:
```bash
npm install
```

2. Ejecutar en modo desarrollo:
```bash
npm run dev
```

3. Compilar para producción:
```bash
npm run build
```

## Tecnologías Utilizadas

- Vue.js 3 (Composition API)
- Tailwind CSS
- Vite

## Estructura del Proyecto

```
├── src/
│   ├── components/
│   │   ├── RegistroRequerimiento.vue
│   │   └── ListaRequerimientos.vue
│   ├── App.vue
│   ├── main.js
│   └── style.css
├── index.html
├── package.json
├── vite.config.js
├── tailwind.config.js
└── postcss.config.js
```

fnm env --use-on-cd | Out-String | Invoke-Expression
fnm use 20