# Última Milla — Simulador de capacidad

Aplicación web sencilla para el caso ficticio **“Última Milla y los kits del festival”**. Todo se ejecuta en el navegador; no necesita instalar dependencias ni configurar un servidor.

## Cómo abrirla

1. Entra a [index.html](./index.html).
2. Pulsa **Download raw file** o descarga el repositorio y abre `index.html` con Chrome, Edge o Firefox.
3. Cambia las cinco entradas y pulsa **Calcular escenario**.

> Para publicarla como sitio: en GitHub abre **Settings → Pages**, selecciona **Deploy from a branch**, elige `main` y `/ (root)`, y guarda. La URL aparecerá en esa misma sección cuando termine el despliegue.

## Entradas

- Pedido total entero (Q).
- Proporción de kits C entre 0 y 1.
- Opción: referencia, A (disposición en U), B (cuarta persona) o C (carro de doble contenedor).
- Minutos netos disponibles por persona.
- Bolsas/soportes disponibles (700 por defecto).

Los platos principales se mantienen fijos en 720. Las alternativas A, B y C son excluyentes.

## Cálculos usados

Promedios observados (segundos): A=18, B=22, extra C=12, D=20, E=16 y marcha W=12 s por 12 m, es decir, 1 s/m. Se aplica el suplemento del 15 % al tiempo normal manual más la marcha.

| Escenario | Recorrido V/C | Tiempo estándar V | Tiempo estándar C | Personas | Coste incremental/turno |
|---|---:|---:|---:|---:|---:|
| Referencia | 18 m / 30 m | 108,10 s | 135,70 s | 3 | 0 € |
| A · Disposición en U | 10 m / 14 m | 98,90 s | 117,30 s | 3 | 20 € |
| B · Cuarta persona | 18 m / 30 m | 108,10 s | 135,70 s | 4 | 91 € |
| C · Carro doble contenedor | 18 m / 30 m | 96,60 s | 124,20 s | 3 | 45 € |

La carga se convierte a cantidades enteras con (Q_C=\lfloor Qp+0,5\rfloor) y (Q_V=Q-Q_C). La capacidad laboral agregada es la parte entera de personas × segundos netos / tiempo medio. La capacidad por materiales es el menor entre 720 platos y el stock de bolsas/soportes.

## Reparto y reglas de decisión

La app reparte primero kits C y luego V, asignando cada kit indivisible a la persona con menor carga acumulada.

- **Viable:** el reparto calculado cabe en cada ventana individual y hay materiales suficientes.
- **Inviable:** faltan materiales, la carga total excede el tiempo del equipo o un solo kit supera la ventana individual.
- **No concluyente:** el reparto propuesto sobrecarga a alguien, pero las cotas globales no descartan el pedido. Se debe revisar el reparto; no se declara imposible.

La productividad se muestra únicamente cuando el pedido completo queda verificado: kits por hora-persona de trabajo requerido. La inversión se amortiza en 20 turnos; B agrega 85 € por turno a los 6 € de amortización.

## Pruebas de aceptación del documento

Con Q=660, 420 minutos netos, stock=700:

| Prueba | Opción | Mezcla C | Resultado esperado |
|---|---|---:|---|
| 1 | Referencia | 0,40 | Inviable por carga total (78 632,4 s > 75 600 s) |
| 2 | A | 0,40 | Viable; 396 V y 264 C |
| 3 | B | 0,90 | Viable con cuatro personas; 66 V y 594 C |

También se valida que minutos netos sean mayores que cero, Q y stock sean enteros no negativos, y la proporción C esté entre 0 y 1. La prueba de indivisibilidad indicada en el documento (un kit V, ventana individual de 1 minuto) debe dar **inviable** porque el tiempo de un kit V excede la ventana individual.

## Alcance

Es un simulador académico con datos sintéticos. No elimina las comprobaciones, separaciones de zonas, sentido único ni rutinas programadas. No constituye certificación de inocuidad ni asesoría normativa.
