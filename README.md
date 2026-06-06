# ⚽ FIFA World Cup 2026 — Fan Page

> Página interactiva del Mundial 2026 con calendario completo, grupos, simulador de cuadro eliminatorio y guía informativa. Un solo archivo HTML, sin dependencias, sin backend.

![HTML](https://img.shields.io/badge/HTML-single--file-orange?style=flat-square&logo=html5)
![CSS](https://img.shields.io/badge/CSS-vanilla-blue?style=flat-square&logo=css3)
![JS](https://img.shields.io/badge/JavaScript-vanilla-yellow?style=flat-square&logo=javascript)
![License](https://img.shields.io/badge/license-MIT-green?style=flat-square)

---

## 📸 Vista general

La página está dividida en cuatro pestañas accesibles desde el header sticky:

| Pestaña | Descripción |
|---|---|
| 🏠 Inicio | Cuenta atrás en tiempo real hasta el partido inaugural |
| 📅 Calendario | Todos los 104 partidos con filtros por texto, sede y fecha |
| 🏆 Grupos | Los 12 grupos con modal de partidos por grupo |
| 🎮 Simulador | Simulador interactivo del cuadro oficial FIFA |
| ℹ️ Info | Datos del torneo y guía de uso del simulador |

---

## 🚀 Cómo usar

No hay instalación. Descarga el archivo y ábrelo en cualquier navegador moderno:

```bash
git clone https://github.com/tu-usuario/wc2026-fanpage.git
cd wc2026-fanpage
# Abre mundial2026.html en tu navegador
```

O simplemente descarga `mundial2026.html` y haz doble clic.

> Las banderas se cargan desde la API pública de FIFA. Necesitas conexión a internet para verlas.

---

## 📅 Calendario

Muestra los 104 partidos del torneo ordenados cronológicamente con hora en horario de Madrid (CEST).

### Filtros disponibles

- **Búsqueda de texto** — filtra por nombre de equipo, estadio o cualquier texto visible en la tarjeta.
- **Sede** — desplegable con todos los estadios del torneo, poblado automáticamente.
- **Fecha** — desplegable con cada jornada en formato corto (ej. "jue. 11 jun.").
- **Botón ✕ Limpiar** — resetea los tres filtros a la vez.

Los filtros son combinables. Al aplicar cualquiera de ellos, los bloques de fecha sin partidos visibles se ocultan automáticamente.

---

## 🏆 Grupos

Muestra los 12 grupos (A–L) como tarjetas con los 4 equipos de cada uno. Al hacer clic en una tarjeta se abre un modal con todos los partidos de ese grupo, con hora y estadio.

---

## 🎮 Simulador

Implementa el cuadro oficial FIFA 2026 según el **Appendix A** del reglamento de competición. El cuadro tiene 16 partidos de ronda de 32 divididos en dos caminos que convergen en la final.

### Paso 1 — Fase de grupos

En la vista **Fase de grupos** puedes establecer la clasificación final de cada uno de los 12 grupos. Dos métodos:

- **Arrastrar y soltar** — agarra una fila de equipo y suéltala en la posición deseada dentro del mismo grupo.
- **Selector de posición** — desplegable (1º / 2º / 3º / 4º) en cada fila para mover el equipo directamente a esa posición.

El color de cada posición indica el estado del equipo:

| Color | Significado |
|---|---|
| 🟢 Verde | Clasificado directo (1º y 2º de grupo) |
| 🟡 Ámbar | Candidato a mejor tercero |
| ⚫ Gris | Eliminado (4º de grupo) |

### Paso 2 — Mejores terceros

Al pie de la cuadrícula de grupos aparece el panel de **terceros**. Muestra los 12 equipos actualmente en 3ª posición de cada grupo (se actualiza en tiempo real al reordenar). Haz clic en los **8** que quieras que clasifiquen.

- El contador muestra cuántos llevas seleccionados.
- Si seleccionas más de 8, el primero que elegiste se deselecciona automáticamente.
- Cada tercero se asigna al partido correcto del cuadro según las reglas FIFA (tabla de asignación de terceros por combinación de grupos).

### Paso 3 — Cuadro eliminatorio

Cambia a la pestaña **Cuadro Final**. El bracket se dibuja con SVG y tarjetas posicionadas absolutamente:

```
P1_R32 → P1_R16 → P1_QF → P1_SF ─┐
                                   FINAL
P2_R32 → P2_R16 → P2_QF → P2_SF ─┘
                                   3er PUESTO
```

- **Haz clic en un equipo** de cualquier tarjeta para designarlo ganador. Avanzará automáticamente a la siguiente ronda.
- Los **perdedores de las semifinales** se sitúan en el partido por el tercer puesto.
- Si cambias el orden de grupos después de haber elegido ganadores, el cuadro se recalcula y solo conserva los resultados que siguen siendo posibles.
- Las líneas de conexión entre rondas se dibujan con SVG y se regeneran en cada actualización.

### Emparejamientos R32 oficiales

| Partido | Enfrentamiento |
|---|---|
| M73 | 2A vs 2B |
| M74 | 1E vs 3ABCDF |
| M76 | 1F vs 2C |
| M77 | 1I vs 3CDFGH |
| M79 | 1A vs 3CEFHI |
| M80 | 1L vs 3EHIJK |
| M81 | 1D vs 3BEFIJ |
| M82 | 1G vs 3AEHIJ |
| M83 | 2K vs 2L |
| M84 | 1H vs 2J |
| M85 | 1B vs 3EFGIJ |
| M86 | 1J vs 2H |
| M87 | 1K vs 3DEIJL |
| M88 | 2D vs 2G |
| — | 1C vs 2F |
| — | 2E vs 2I |

---

## 🗂️ Estructura del proyecto

```
mundial2026.html        # Todo el proyecto en un único archivo
│
├── <style>             # CSS completo (tema oscuro, responsive)
│   ├── Layout general (header sticky, nav, container)
│   ├── Countdown
│   ├── Filtros y tarjetas de partido
│   ├── Modal de grupos
│   ├── Simulador (grupos, terceros, bracket)
│   └── Footer
│
└── <script>            # JavaScript vanilla
    ├── Datos
    │   ├── teams{}         Códigos FIFA → nombres en español
    │   ├── groups{}        12 grupos con sus 4 selecciones
    │   └── allMatches[]    104 partidos (fecha, hora, equipos, sede, fase)
    │
    ├── Navegación          switchTab()
    ├── Countdown           updateCountdown() — tick cada 1s
    ├── Calendario          loadMatches(), filterMatches(), resetFilters()
    ├── Grupos              loadGroups(), openGroup(), closeModal()
    └── Simulador
        ├── Estado          groupOrdersState, bracketState, selectedThirds
        ├── Grupos          renderSimGroup(), setupSimDragDrop()
        ├── Terceros        renderThirdsPanel()
        ├── Lógica          updateBracketLogic(), resolveTeam()
        └── Bracket         renderBracket(), pickWinner()
```

---

## 🎨 Diseño

- **Tema:** oscuro (`#0a1929` base), sin dependencias de frameworks CSS.
- **Tipografía:** Inter (Google Fonts).
- **Paleta de acento:** cyan `#00d9ff` + dorado `#ffd700` + verde `#4ade80` + ámbar `#fbbf24`.
- **Responsive:** diseñado con `clamp()` y `grid auto-fill` para funcionar desde móvil hasta pantalla ancha.
- **Banderas:** cargadas desde `api.fifa.com` por código de selección.

---

## 📦 Datos incluidos

- ✅ 48 selecciones participantes con códigos FIFA
- ✅ 12 grupos completos (A–L)
- ✅ 104 partidos: fecha, hora Madrid (CEST), equipos, estadio y fase
- ✅ 16 sedes (11 EE. UU. + 3 México + 2 Canadá)
- ✅ Emparejamientos R32 según Appendix A oficial FIFA

---

## 🛠️ Tecnologías

| Tecnología | Uso |
|---|---|
| HTML5 | Estructura y semántica |
| CSS3 | Estilos, animaciones, layout responsive |
| JavaScript ES6+ | Lógica, DOM, drag & drop, SVG dinámico |
| SVG | Líneas conectoras del bracket |
| Google Fonts | Tipografía Inter |
| FIFA API (pública) | Imágenes de banderas |

---

## 🤝 Contribuir

1. Haz fork del repositorio
2. Crea una rama: `git checkout -b feature/mi-mejora`
3. Commitea tus cambios: `git commit -m 'feat: descripción'`
4. Haz push: `git push origin feature/mi-mejora`
5. Abre un Pull Request

---

## 📄 Licencia

MIT — libre para uso personal y comercial. Si lo usas en un proyecto público, se agradece una mención.

---

<p align="center">Hecho con 💙 e IA · <strong>GENBYTE</strong></p>
