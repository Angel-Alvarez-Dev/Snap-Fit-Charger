# Snap Fit Charger

**Cargador USB-C Modular Ultra-Compacto con Topología Flyback Aislada**

![Estado](https://img.shields.io/badge/estado-Desarrollo%20MVP-yellow)
![Potencia](https://img.shields.io/badge/potencia-20W%20USB--PD-blue)
![Seguridad](https://img.shields.io/badge/seguridad-IEC%2062368--1-green)

---

## 📋 Descripción General del Proyecto

Snap Fit Charger es un adaptador de corriente AC-DC ultra-compacto e innovador diseñado para máxima portabilidad y compatibilidad universal. El proyecto combina electrónica de potencia con diseño mecánico modular snap-fit para crear una solución de carga verdaderamente versátil.

**Innovación Clave**: Sistema modular de plugs snap-fit que permite a los viajeros intercambiar adaptadores regionales sin necesidad de cargar múltiples cargadores.

### Especificaciones Objetivo

- **Entrada**: 90–264 V AC, 50/60 Hz (Universal)
- **Salida**: USB-C con USB-PD 3.0
  - 5V @ 3A (15W)
  - 9V @ 2.22A (20W)
  - 12V @ 1.67A (20W)
- **Potencia Máxima**: 20W
- **Topología**: Convertidor Flyback aislado de alta frecuencia
- **Dimensiones**: Objetivo < 40mm × 40mm × 25mm
- **Eficiencia**: >85% a carga nominal
- **Seguridad**: Cumplimiento IEC 62368-1

---

## 🎯 Planteamiento del Problema

**Puntos de Dolor Actuales:**
1. Los viajeros necesitan múltiples cargadores para diferentes regiones
2. Cargadores voluminosos ocupan espacio premium en el equipaje
3. Estándares de carga propietarios generan basura electrónica
4. Diseños no modulares imposibilitan reparaciones

**Nuestra Solución:**
Un cargador USB-C del tamaño de la palma de la mano, con adaptadores de enchufe intercambiables mediante snap-fit, voltaje de entrada universal y cumplimiento con estándares USB Power Delivery.

---

## 🏗️ Estructura del Repositorio

```
snap-fit-charger/
├── README.md                          # Este archivo
├── LICENSE                            # Licencia MIT
│
├── hardware/
│   ├── pcb/
│   │   ├── README.md                  # Descripción del diseño PCB
│   │   ├── schematic/
│   │   │   ├── snapfit_schematic.pdf
│   │   │   ├── snapfit_schematic.kicad_sch
│   │   │   └── diagrama_bloques.png
│   │   ├── layout/
│   │   │   ├── snapfit_layout.kicad_pcb
│   │   │   ├── gerbers/
│   │   │   └── vista_3d.png
│   │   ├── bom/
│   │   │   ├── mvp_charger_power_bom.csv
│   │   │   └── notas_abastecimiento.md
│   │   └── reglas_diseno/
│   │       ├── creepage_clearance.md
│   │       └── diseno_termico.md
│   │
│   └── mecanico/
│       ├── carcasa/
│       │   ├── snapfit_tapa_superior.step
│       │   ├── snapfit_tapa_inferior.step
│       │   └── ensamble.pdf
│       └── adaptadores_plug/
│           ├── tipo_a_us.step           # EE.UU./Norteamérica
│           ├── tipo_c_eu.step           # Europa
│           ├── tipo_g_uk.step           # Reino Unido
│           └── mecanismo_snap.pdf
│
├── firmware/
│   ├── config_usb_pd/
│   │   ├── stusb4500_config.h
│   │   └── perfiles_pd.c
│   └── README.md
│
├── docs/
│   ├── normas/
│   │   ├── resumen_IEC_62368-1.md
│   │   ├── especificacion_USB_PD_3.0.pdf
│   │   └── requisitos_EMC.md
│   ├── notas_diseno/
│   │   ├── descripcion_arquitectura.md
│   │   ├── seleccion_componentes.md
│   │   └── presupuesto_potencia.xlsx
│   ├── manual_usuario/
│   │   ├── inicio_rapido.md
│   │   └── advertencias_seguridad.md
│   └── competencia/
│       └── presentacion_honpe_challenge.md
│
├── pruebas/
│   ├── electricas/
│   │   ├── prueba_eficiencia.md
│   │   ├── regulacion_carga.md
│   │   └── resultados_pruebas.csv
│   ├── termicas/
│   │   ├── plan_pruebas_termicas.md
│   │   └── imagenes_ir/
│   ├── emc/
│   │   └── emisiones_conducidas.md
│   └── seguridad/
│       ├── prueba_hipot.md
│       └── resistencia_aislamiento.md
│
├── manufactura/
│   ├── instrucciones_ensamble.md
│   ├── stackup_pcb.pdf
│   ├── pick_and_place/
│   └── checklist_inspeccion.md
│
└── media/
    ├── renders/
    │   ├── hero_producto.png
    │   └── vista_explosionada.png
    ├── fotos/
    └── videos/
```

---

## 🔧 Arquitectura de Hardware

### Bloques de Etapa de Potencia

1. **Entrada AC y Filtro EMI**
   - Fusible SMD (F1) - 2A/250V
   - MOV (MOV1) - Protección contra sobretensiones 275VAC
   - Capacitor X (CX1) + Capacitores Y (CY1, CY2) - Supresión EMI
   - Choke de modo común (L1) - Atenuación diferencial/modo común

2. **Convertidor Flyback Primario**
   - IC Controlador (U1): HF500-15 (PSR con MOSFET integrado)
   - Red de arranque: R1, C3, C4
   - Bobinado primario de T1

3. **Transformador de Aislamiento**
   - Transformador planar (T1) - Aislamiento reforzado 1500Vrms
   - Ubicación centrada para óptimo creepage
   - Ranuras de aislamiento en PCB

4. **Rectificación Síncrona Secundaria**
   - Controlador SR (U2): UCC24610D
   - MOSFET de bajo Rds(on) (Q1)
   - Filtro de salida LC: L2 (4.7µH) + C5/C6 (22µF cada uno)
   - Divisor de retroalimentación: R2, R3

5. **Controlador USB-PD y Salida**
   - IC USB-PD (U3): STUSB4500QTR
   - Resistores línea CC (R4, R5) - Detección de perfil
   - Filtrado de salida: C7, C8
   - Conector USB-C (J1)

### Apilamiento PCB (4 capas)

- **Capa 1**: Señales lado primario (alto voltaje)
- **Capa 2**: Plano de tierra primario (aislado)
- **Capa 3**: Plano de tierra secundario
- **Capa 4**: Señales lado secundario (bajo voltaje)

**Aislamiento**: ≥3.2mm creepage/clearance entre primario y secundario

---

## 🎨 Innovación Mecánica

### Sistema de Plug Snap-Fit

El sistema modular de adaptadores de enchufe permite a los usuarios:
- Intercambiar tipos de plug en segundos sin herramientas
- Cargar solo los adaptadores regionales necesarios
- Reemplazar plugs dañados individualmente
- Reducir basura electrónica manteniendo el módulo de potencia

**Características del Mecanismo:**
- Pestañas de bloqueo con resorte
- Guías de alineación polarizadas
- Contactos AC chapados en oro clasificados para 10,000+ inserciones
- Almacenamiento compacto: los adaptadores se apilan juntos

---

## 📊 Lista de Materiales (Componentes Clave)

| Ref | Componente | Número de Parte | Cant | Función |
|-----|-----------|-------------|-----|----------|
| U1 | Controlador Flyback | HF500-15 | 1 | Regulación lado primario |
| U2 | Controlador SR | UCC24610D | 1 | Rectificación síncrona |
| U3 | Controlador USB-PD | STUSB4500QTR | 1 | Negociación power delivery |
| T1 | Transformador Planar | Personalizado | 1 | Aislamiento galvánico |
| Q1 | FET Rectificador Sync | TBD (30V, <10mΩ) | 1 | Rectificación secundaria |
| L1 | Choke Modo Común | TBD | 1 | Filtrado EMI |
| L2 | Inductor de Salida | 4.7µH, 3A | 1 | Filtrado de salida |
| J1 | Conector USB-C | GCT USB4105 | 1 | Salida de potencia |

*BOM completo disponible en `/hardware/pcb/bom/`*

---

## 🚀 Hoja de Ruta de Desarrollo

### Fase 1: MVP (Actual)
- [x] Diseño esquemático inicial
- [x] Selección de componentes
- [ ] Layout PCB (4 capas)
- [ ] Verificación de reglas de diseño
- [ ] Simulación térmica

### Fase 2: Prototipo
- [ ] Fabricación PCB (5 unidades)
- [ ] Adquisición de componentes
- [ ] Ensamble y puesta en marcha
- [ ] Pruebas eléctricas básicas
- [ ] Impresión 3D carcasa snap-fit

### Fase 3: Validación
- [ ] Caracterización eléctrica completa
- [ ] Pruebas térmicas
- [ ] Pruebas EMC pre-cumplimiento
- [ ] Pruebas de seguridad (hipot, aislamiento)
- [ ] Pruebas de durabilidad mecánica

### Fase 4: Preparación para Producción
- [ ] Optimización de diseño basada en pruebas
- [ ] Certificación (CE, FCC, UL)
- [ ] Selección de socio de manufactura
- [ ] Herramental para moldeo por inyección
- [ ] Corrida piloto de producción

---

## 🧪 Estrategia de Pruebas

### Pruebas Eléctricas
- Rango de voltaje de entrada (85-270V AC)
- Regulación de carga (0-100% carga)
- Regulación de línea
- Mapeo de eficiencia
- Respuesta transitoria
- Protección contra cortocircuito

### Pruebas Térmicas
- Pruebas de elevación de temperatura ambiente
- Mapeo de temperatura de componentes (cámara IR)
- Verificación de derating
- Prueba de estrés de operación continua

### Seguridad y Cumplimiento
- Resistencia de aislamiento (>5MΩ)
- Pruebas hipot (primario-secundario, 3000V AC)
- Verificación creepage/clearance
- Inflamabilidad (UL94 V-0)

### Pruebas EMC
- Emisiones conducidas (CISPR 32)
- Emisiones radiadas
- Inmunidad ESD
- Inmunidad a sobretensiones

---

## 🎓 Diseño para Honpe Challenge

Este proyecto está siendo desarrollado para presentación al **Honpe Challenge 2026** - Competencia de Innovación en Dispositivos de Consumo.

### Alineación con la Competencia

**Problemática Detectada**: Inconveniencia de viaje y basura electrónica por cargadores no modulares

**Usuario Objetivo**: 
- Viajeros internacionales
- Nómadas digitales
- Estudiantes en el extranjero
- Profesionales de negocios

**Contexto de Uso**: 
- Hoteles, aeropuertos, cafés, espacios de coworking
- Múltiples dispositivos por usuario (teléfono, tablet, accesorios de laptop)
- Viajes regionales frecuentes

**Objetivo del Dispositivo**: 
Proporcionar una solución de carga única y compacta que se adapte a cualquier región mediante modularidad snap-fit manteniendo seguridad y eficiencia.

**Puntos de Innovación**:
1. Sistema modular de plug snap-fit (mecánico)
2. Topología flyback ultra-compacta (eléctrico)
3. Voltaje de entrada universal (90-264V AC)
4. Compatibilidad USB-PD (software)

*Ver `/docs/competencia/` para materiales completos de presentación*

---

## 👥 Equipo

**Líder del Proyecto**: [Tu Nombre]
- Universidad: [Tu Universidad]
- Carrera: [Tu Carrera]
- Correo: [Tu Correo]

**Miembros del Equipo**:
- [Nombre Miembro 2] - [Rol]
- [Nombre Miembro 3] - [Rol]

---

## 📚 Referencias y Normas

- IEC 62368-1: Equipos de audio/video, tecnología de la información y comunicación - Requisitos de seguridad
- Especificación USB Power Delivery Rev 3.0
- CISPR 32: Compatibilidad electromagnética de equipos multimedia
- UL 60950-1: Seguridad de equipos de tecnología de la información
- Notas de Aplicación Power Integrations
- Guía de Diseño de Rectificación Síncrona Texas Instruments

---

## 📄 Licencia

Este proyecto está licenciado bajo la Licencia MIT - ver el archivo [LICENSE](LICENSE) para detalles.

**Nota**: Este es un proyecto de hardware abierto. El uso comercial requiere certificación de seguridad adecuada y pruebas de cumplimiento.

---

## 🤝 Contribuciones

¡Agradecemos contribuciones! Áreas donde se necesita ayuda:
- Revisión de layout PCB
- Simulación térmica
- Optimización de diseño mecánico
- Mejoras en documentación
- Pruebas y validación

Por favor lee nuestras pautas de contribución antes de enviar pull requests.

---

## 📞 Contacto

**Correo del Proyecto**: [tu-correo@example.com]

**Contacto Honpe Challenge**: honpechallenge@honpe.mx

**Repositorio**: [URL GitHub/GitLab]

---

## ⚠️ Advertencia de Seguridad

**Este dispositivo opera con voltajes AC potencialmente letales.**

- Solo personal calificado debe intentar construir o modificar este diseño
- Siempre usar transformadores de aislamiento durante pruebas
- Asegurar carcasa y aislamiento apropiados antes de conectar a red eléctrica
- Seguir todos los códigos y regulaciones de seguridad eléctrica aplicables
- Este diseño se proporciona con fines educativos - el uso comercial requiere pruebas de cumplimiento y certificación completas

---

## 🙏 Agradecimientos

- Honpe Prototyping México por organizar el desafío
- Comunidad de electrónica de potencia por referencias de código abierto
- Fabricantes de componentes por hojas de datos detalladas y notas de aplicación

---

**Última Actualización**: Octubre 2025  
**Estado del Proyecto**: Fase de Desarrollo MVP  
**Siguiente Hito**: Completar Layout PCB
