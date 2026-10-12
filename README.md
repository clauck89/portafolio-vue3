# Portafolio Personal — Vue 3 + Vite

Este proyecto representa la migración completa y construcción desde cero de mi portafolio web (originalmente en HTML/JS tradicional) a una moderna Aplicación de Página Única (SPA) utilizando **Vue 3**, **Vite** y **pnpm** como gestor de paquetes.

Esta nueva versión elimina dependencias antiguas como jQuery, aprovechando el sistema de reactividad de Vue 3 para gestionar el estado, los filtros, los modales y el tema visual.

## Características Principales

- **Arquitectura Reactiva:** Filtros, modales de casos de estudio y navegación controlados nativamente por Vue 3.
- **Diseño Responsive:** Adaptable a dispositivos móviles con tema claro/oscuro dinámico.
- **Gestor de Paquetes Optimizado:** Uso de `pnpm` para instalaciones más rápidas y eficientes.
- **Secciones Modulares:** Presentación, perfil, habilidades, proyectos y contacto.
- **Formulario de Contacto:** Prepara correos mediante `mailto:` (seguro, sin almacenar ni enviar datos a un backend).
- **Accesibilidad y SEO:** Metadatos iniciales y soporte para preferencias del sistema como `prefers-reduced-motion`.

## Requisitos Previos

- Node.js (versión compatible con Vite 6).
- [pnpm](https://pnpm.io/es/installation) instalado de forma global.

## Ejecutar en Desarrollo

Clona el repositorio e instala las dependencias usando pnpm:

```bash
pnpm install