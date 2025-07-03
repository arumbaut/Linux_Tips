Comprobar si el espacio libre de los PV que se quieren eliminar se encuentra libre en su totalidad.

```
pvs
```

**Si todavía se encuentra en uso entonces realizaremos una migración de dato con ayuda de 'pvmove' valorando el riesgo y el impacto de esta acción.**

Eliminar el PV del VG al que está asignado.

vgreduce VVVVV PPPPP