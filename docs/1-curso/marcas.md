# Lenguajes de Marcas y Sistemas de Gestión de Información (LMSGI)

Estructuración de información con XML, esquemas de validación y desarrollo web básico con HTML5 y CSS3.

---

## 🏷️ XML y XPath

```xml
<?xml version="1.0" encoding="UTF-8"?>
<instituto>
    <ciclo nombre="ASIR">
        <modulo horas="160">Implantación de Aplicaciones Web</modulo>
        <modulo horas="140">Servicios de Red e Internet</modulo>
    </ciclo>
</instituto>
```

### Consultas XPath
- `/instituto/ciclo[@nombre='ASIR']/modulo`: Selecciona todos los módulos de ASIR.
- `//modulo[@horas > 150]/text()`: Devuelve los nombres de módulos con más de 150 horas.
