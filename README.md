# Monitor Agropecuario · Entre Ríos

## Estructura del proyecto

```
├── index.html              ← página principal (única)
├── data/
│   ├── auto.json           ← datos actualizados automáticamente (GitHub Actions)
│   └── manual.json         ← datos de actualización manual mensual (OPCIONAL)
├── scripts/
│   └── fetch_data.py       ← script Python que fetchea los APIs oficiales
└── .github/
    └── workflows/
        └── update-data.yml ← corre cada lunes 8 AM Argentina
```

## Despliegue en Netlify

1. Crear cuenta en https://netlify.com
2. "Import from Git" → conectar este repositorio
3. Configuración: Build command: (vacío), Publish directory: `.`
4. Deploy site

## Actualización automática

- **FOB SAGyP**: API pública `apis.datos.gob.ar/series/api` — se actualiza en el browser al cargar la página
- **TC BNA**: API pública `api.estadisticasbcra.ar` — idem
- **Gasoil ER**: CKAN API de datos.energia.gob.ar — GitHub Actions lo fetchea semanalmente

## Datos de actualización manual (mensual)

Actualizar en `data/manual.json` o en Google Sheets conectada:
- Pizarra BCR (soja, maíz, trigo, arroz Uru5) → fuente: Coop. Lehmann / BOLSACER
- Novillito/Ternero → fuente: Rosgan
- Capón Porcino → fuente: SIO Carnes / SAGyP
- Leche al tambo → fuente: OCLA (ocla.org.ar/portafolio/16)
- Faena / exportaciones → fuente: SAGyP, DGEC-ER

## Fuentes
- SAGyP: https://www.magyp.gob.ar
- BOLSACER: https://bolsacer.org.ar
- OCLA: https://www.ocla.org.ar
- DGEC ER: https://www.entrerios.gov.ar/dgec
- Sec. Energía: http://datos.energia.gob.ar
- BCRA: https://www.bcra.gob.ar
