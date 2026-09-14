# CodeIgniter4Forge

> **Fork de CodeIgniter 4** con una modificación estructural clave: el front controller (`index.php`) vive en la **raíz del proyecto**, no dentro de `public/`.

[![PHPUnit](https://github.com/codeigniter4/CodeIgniter4/actions/workflows/test-phpunit.yml/badge.svg)](https://github.com/codeigniter4/CodeIgniter4/actions/workflows/test-phpunit.yml)
[![PHPStan](https://github.com/codeigniter4/CodeIgniter4/actions/workflows/test-phpstan.yml/badge.svg)](https://github.com/codeigniter4/CodeIgniter4/actions/workflows/test-phpstan.yml)
[![Psalm](https://github.com/codeigniter4/CodeIgniter4/actions/workflows/test-psalm.yml/badge.svg)](https://github.com/codeigniter4/CodeIgniter4/actions/workflows/test-psalm.yml)
[![Coverage Status](https://coveralls.io/repos/github/codeigniter4/CodeIgniter4/badge.svg?branch=develop)](https://coveralls.io/github/codeigniter4/CodeIgniter4?branch=develop)
[![Downloads](https://poser.pugx.org/codeigniter4/framework/downloads)](https://packagist.org/packages/codeigniter4/framework)
[![GitHub release (latest by date)](https://img.shields.io/github/v/release/codeigniter4/CodeIgniter4)](https://packagist.org/packages/codeigniter4/framework)
[![GitHub stars](https://img.shields.io/github/stars/codeigniter4/CodeIgniter4)](https://packagist.org/packages/codeigniter4/framework)
[![GitHub license](https://img.shields.io/github/license/codeigniter4/CodeIgniter4)](https://github.com/codeigniter4/CodeIgniter4/blob/develop/LICENSE)
[![contributions welcome](https://img.shields.io/badge/contributions-welcome-brightgreen.svg?style=flat)](https://github.com/codeigniter4/CodeIgniter4/pulls)
<br>

## ¿Qué es CodeIgniter?

CodeIgniter es un framework PHP full-stack ligero, rápido, flexible y seguro.
Más información en el [sitio oficial](https://codeigniter.com).

Este repositorio contiene el código fuente de CodeIgniter 4 con la modificación
**CodeIgniter4Forge** descrita abajo.

## Cambio fundamental: `index.php` en la raíz

A diferencia del CodeIgniter 4 oficial, donde el front controller vive dentro de
la carpeta `public/` y el servidor web debe configurarse para apuntar allí,
**CodeIgniter4Forge** coloca `index.php` directamente en la raíz del proyecto.

### ¿Por qué?

- **Drop-and-run**: puedes copiar todo el proyecto a cualquier directorio del
  servidor web y funciona de inmediato, sin configurar virtual hosts ni
  document roots personalizados.
- **Simplicidad en despliegues**: ideal para entornos compartidos, cPanel,
  Paneles de control, o cualquier situación donde no tengas control sobre la
  configuración del servidor web.
- **Experiencia tradicional**: recupera el modelo de trabajo de CodeIgniter 3 y
  otros frameworks donde el proyecto se sirve directamente desde su propia
  carpeta.

### ¿Qué se modificó?

| Archivo | Cambio |
|---|---|
| **`index.php`** (raíz) | Creado nuevo front controller. `FCPATH` apunta a la raíz. La ruta a `app/Config/Paths.php` se resuelve como `FCPATH . 'app/Config/Paths.php'` (sin `../`). |
| **`.htaccess`** (raíz) | Creado con reglas de reescritura que: (1) sirven archivos estáticos de la raíz; (2) si no existen en la raíz, intentan servirlos desde `public/`; (3) envían todo lo demás al front controller. |
| **`public/.htaccess`** | Sin cambios — se conserva intacto por compatibilidad. |
| **`public/index.php`** | Sin cambios — se conserva intacto por compatibilidad. |
| **`spark`** (CLI) | `FCPATH` actualizado de `public/` a la raíz. |
| **`preload.php`** | `FCPATH` actualizado de `public/` a la raíz. |
| **`favicon.ico`** | Copiado de `public/` a la raíz. |
| **`robots.txt`** | Copiado de `public/` a la raíz. |

### ¿Sigue funcionando la carpeta `public/`?

**Sí.** La carpeta `public/` y todo su contenido original se conservan intactos
por dos razones:

1. **Compatibilidad hacia atrás**: si alguien sigue usando `public/` como
   document root, el `index.php` y `.htaccess` originales aún funcionan.
2. **Assets legacy**: el `.htaccess` de la raíz tiene una regla de
   *fallback*: si un archivo estático no se encuentra en la raíz, busca
   automáticamente dentro de `public/`. Así, cualquier CSS, JS o imagen que
   estuviera en `public/assets/` se sigue sirviendo sin mover nada.

### Nota sobre seguridad

Aunque ahora el proyecto es "drop-and-run", la carpeta `public/` original de
CodeIgniter 4 tenía un propósito de seguridad: mantener archivos sensibles
(`app/`, `system/`, `writable/`, `.env`) fuera del alcance del servidor web.
Con CodeIgniter4Forge, esos directorios están en la misma raíz que `index.php`,
por lo que **es importante que el servidor web tenga configurado un bloqueo
adecuado** para evitar que se acceda a ellos directamente.

El `.htaccess` incluido ayuda, pero su efectividad depende de que Apache tenga
`AllowOverride` habilitado. En Nginx, debes agregar reglas explícitas para
denegar el acceso a `app/`, `system/`, `writable/`, `.env`, etc.

## Repository Management

CodeIgniter is developed completely on a volunteer basis. As such, please give up to 7 days
for your issues to be reviewed. If you haven't heard from one of the team in that time period,
feel free to leave a comment on the issue so that it gets brought back to our attention.

> [!IMPORTANT]
> We use GitHub issues to track **BUGS** and to track approved **DEVELOPMENT** work packages.
> We use our [forum](http://forum.codeigniter.com) to provide SUPPORT and to discuss
> FEATURE REQUESTS.

If you raise an issue here that pertains to support or a feature request, it will
be closed! If you are not sure if you have found a bug, raise a thread on the forum first -
someone else may have encountered the same thing.

Before raising a new GitHub issue, please check that your bug hasn't already
been reported or fixed.

We use pull requests (PRs) for CONTRIBUTIONS to the repository.
We are looking for contributions that address one of the reported bugs or
approved work packages.

Do not use a PR as a form of feature request.
Unsolicited contributions will only be considered if they fit nicely
into the framework roadmap.
Remember that some components that were part of CodeIgniter 3 are being moved
to optional packages, with their own repository.

## Contributing

We **are** accepting contributions from the community! It doesn't matter whether you can code, write documentation, or help find bugs,
all contributions are welcome.

Please read the [*Contributing to CodeIgniter*](https://github.com/codeigniter4/CodeIgniter4/blob/develop/contributing/README.md).

CodeIgniter has had thousands on contributions from people since its creation. This project would not be what it is without them.

<a href="https://github.com/codeigniter4/CodeIgniter4/graphs/contributors">
  <img src="https://contrib.rocks/image?repo=codeigniter4/CodeIgniter4" />
</a>

Made with [contrib.rocks](https://contrib.rocks).

## Server Requirements

PHP version 8.2 or higher is required, with the following extensions installed:

- [intl](http://php.net/manual/en/intl.requirements.php)
- [mbstring](http://php.net/manual/en/mbstring.installation.php)

> [!WARNING]
> - The end of life date for PHP 7.4 was November 28, 2022.
> - The end of life date for PHP 8.0 was November 26, 2023.
> - The end of life date for PHP 8.1 was December 31, 2025.
> - If you are still using a PHP version below 8.2, you should upgrade immediately.
> - The end of life date for PHP 8.2 will be December 31, 2026.

Additionally, make sure that the following extensions are enabled in your PHP:

- json (enabled by default - don't turn it off)
- [mysqlnd](http://php.net/manual/en/mysqlnd.install.php) if you plan to use MySQL
- [libcurl](http://php.net/manual/en/curl.requirements.php) if you plan to use the HTTP\CURLRequest library

## Running CodeIgniter Tests

Information on running the CodeIgniter test suite can be found in the [README.md](tests/README.md) file in the tests directory.

---

## 🤝 Contribución

Este proyecto es **Open Source** y vive gracias a la comunidad. ¡Tus contribuciones son bienvenidas!

### Cómo Contribuir

1. **Fork** del repositorio
2. **Crea tu rama** de característica
   ```bash
   git checkout -b feature/nueva-funcionalidad
   ```
3. **Asegúrate de ejecutar los tests**
   ```bash
   composer test
   ```
4. **Haz commit de tus cambios**
   ```bash
   git commit -m 'Add: Nueva funcionalidad increíble'
   ```
5. **Push a tu rama**
   ```bash
   git push origin feature/nueva-funcionalidad
   ```
6. **Abre un Pull Request**

### Directrices de Contribución

- ✅ Sigue los estándares PSR-12
- ✅ Mantén el tipado estricto (`declare(strict_types=1)`)
- ✅ Documenta todas las funciones públicas
- ✅ Agrega tests para nuevas funcionalidades
- ✅ Actualiza la documentación relevante

### Áreas que Necesitan Ayuda

- 📝 Mejoras en documentación
- 🧪 Tests unitarios y de integración
- 🎨 Nuevos componentes de Bootstrap
- 🔧 Implementación de nuevos frameworks (Tailwind, Material)
- 🌍 Traducciones de documentación
- 🐛 Reportes de bugs

---

## 🤝 Soporte y Comunidad

### ¿Necesitas Ayuda?

- 📖 **Documentación**: Lee el [README completo](README.md) y [ARCHITECTURE.md](ARCHITECTURE.md)
- 🐛 **Reportar bugs**: Abre un [issue en GitHub](https://github.com/jalexiscv/Html/issues)
- 💡 **Solicitar funcionalidades**: Usa las [GitHub Discussions](https://github.com/jalexiscv/Html/discussions)
- 📧 **Contacto directo**: jalexiscv@gmail.com

### Comunidad

- **Discusiones**: Únete a las conversaciones en GitHub Discussions
- **Contribuciones**: Revisa los [issues etiquetados como "good first issue"](https://github.com/jalexiscv/Html/labels/good%20first%20issue)

---

## 📜 Licencia

Distribuido bajo la Licencia **MIT**. Ver [LICENSE](LICENSE) para más información.

> La licencia MIT te permite usar, copiar, modificar, fusionar, publicar, distribuir, sublicenciar y/o vender copias del software sin restricciones, siempre que se incluya el aviso de copyright.

---

## 👨‍💻 Autor

**Jose Alexis Correa Valencia**  
*Full Stack Developer & Software Architect*

Con más de 25 años de experiencia en desarrollo de software empresarial, especializado en arquitecturas escalables y soluciones PHP modernas.

- **GitHub**: [@jalexiscv](https://github.com/jalexiscv)
- **LinkedIn**: [Jose Alexis Correa Valencia](https://www.linkedin.com/in/jalexiscv/)
- **Email**: jalexiscv@gmail.com
- **Ubicación**: Colombia 🇨🇴

---

## ❤️ Donaciones

Si CodeIgniter4Forge te ha ayudado a ti o a tu negocio, considera apoyar su desarrollo y mantenimiento continuo.

| Método | Detalles |
|--------|----------|
| **PayPal** | [jalexiscv@gmail.com](https://www.paypal.com/paypalme/anssible) |
| **Nequi (Colombia)** | `3117977281` |

### Beneficios de tu Soporte

Tu donación ayuda a:
- ⚡ Acelerar el desarrollo de nuevas funcionalidades
- 📚 Crear más documentación y ejemplos
- 🧪 Mejorar la cobertura de tests
- 🎨 Implementar soporte para más frameworks
- 🌍 Mantener el proyecto activo y actualizado

*¡Gracias por tu apoyo!* 🙏

---

<div align="center">

**Desarrollado con ❤️ para la comunidad PHP**

[⬆ Volver arriba](#codeigniter4forge)

</div>
