# Autenticación

La autenticación del API de Smart CM es proporcionada por el API Key generado al crear una fuente. Los tokens adicionales de autenticación se encuentran dentro de nuestra plataforma.

# QR

## Generación QR Síncrono

```javascript
const axios = require('axios');
const url = 'https://api.qa.scmfix.com/lotes/mc/qr/sync';
const config = {
  headers: {
    'Content-Type': 'application/json',
    'SCM-Token': 'TOKEN GENERADO',
    'SCM-Modo-Ejecucion': 'PR'
  }
}
const body = {
  idLote: 'lote100',
  idMensaje: '129300-1',
  monto: 1234.78,
  concepto: 'Prueba1',
  referenciaNumerica: 12345,
  tipoPago: 20,
  fechaSolicitud: '1573590949',
  fechaVencimiento: '1573590949',
  informacionAdicional: {
    campoPrueba: 'Valor 1'
  },
  nombreBeneficiario2: 'NA',
  tipoCuentaBeneficiario2: 'NA',
  cuentaBeneficiario2: 'NA'
}
axios.post(
  url,
  body,
  config
)
  .then(...)
  .catch(...)
```

> Response

```json
{
  "codigoResultado": 200,
  "textoResultado": "QR Generado",
  "fechaProcesamiento": "1573590949",
  "idLote": "lote100",
  "idMensaje": "129300-1",
  "mcQR": {
    "Mensaje de cobro tipo JSON para desplegar el QR"
  },
  "imageMCQR": "IMAGEN DEL QR EN BASE 64"
}
```

Endpoint que permite la generación de QRs de manera síncrona


### HTTP Request

`POST https://api.qa.scmfix.com/lotes/mc/qr/sync`

### Headers

Header | Tipo | Requerido | Valor
------ | ---- | --------- | -----
Content-Type | string | ***true*** | application/json
SCM-Token | string | ***true*** | APP_TOKEN
SCM-Modo-Ejecucion | string | ***true*** | BL Ejecución del primer bloque con respuesta correcta dummy<br>UN Primer bloque ejecutado, siguiente en modo BL<br>IN Ejecucion de todos los procesos, sin persistencia de datos<br>PR Producción

### POST Body

Atributo | Descripcion | Tipo
-------- | ----------- | ----
idLote | Id del Lote en el que se generó el QR | ***string***
idMensaje | Id del mensaje de cobro asociado al QR generado | ***string***
monto | Monto a cobrar | ***number***
concepto | Concepto de cobro | ***string***
referenciaNumerica | Referencia numérica para la identifcación del cobro | ***number***
tipoPago | Tipo de cuenta a la que se realizará el pago | ***number***
fechaSolicitud | Fecha de generación del QR | ***UNIX DATE***
fechaVencimiento | Fecha límite de vigencia del QR generado | ***UNIX DATE***
informacionAdicional | Campos adicionales de informacion | ***json***
nombreBeneficiario2 | Nombre del beneficiario (Solo en tipo 22) | ***string***
tipoCuentaBeneficiario2 | Tipo de cuenta de beneficiario (Solo en tipo 22) | ***number***
cuentaBeneficiaro2 | Número de cuenta de beneficiario (Solo en tipo 22) | ***string***

<aside class="success">
Recuerda, siempre debes de agregar el token generado de tu aplicación para realizar una petición a SCM API
</aside>

## Consulta QR Síncrono

```javascript
const axios = require('axios');
const url = 'https://api.qa.scmfix.com/lotes/lote3/mc/127010-1/qr/sync?page=3&size=3';
const config = {
  headers: {
    'SCM-Token': '************',
    'SCM-Modo-Ejecucion': 'PR'
  },
  params: {
    page: '3',
    size: '3'
  }
}
axios.get(
  url,
  config
)
  .then(...)
  .catch(...)
```

> Reponse

```json
{
  "idLote": "lote3",
  "lista": [
    {
      "idMensaje": "127010-1",
      "estadoMensaje": "Procesado",
      "horaProcesamientoMC": "2012-04-23T18:25:43.511Z",
      "informacionAdicional": { "campoPrueba": "Prueba" }
    }
  ],
  "size": 3,
  "page": 3,
  "codigoResultado": 200,
  "textoResultado": "Success"
}
```

Este endpoint te permite consultar un QR Síncrono por su ID

### HTTP Request

`GET https://api.qa.scmfix.com/lotes/{idLote}/mc/{idMensaje}/qr/sync?page={page}&size={size}`

### Headers

Header | Tipo | Requerido | Valor
------ | ---- | --------- | -----
SCM-Token | string | ***true*** | APP_TOKEN
SCM-Modo-Ejecucion | string | ***true*** | BL Ejecución del primer bloque con respuesta correcta dummy<br>UN Primer bloque ejecutado, siguiente en modo BL<br>IN Ejecucion de todos los procesos, sin persistencia de datos<br>PR Producción

### Path Params

Param | Descripcion | Requerido
----- | ----------- | ---------
idLote | ID de Lote a consultar | ***true***
idMensaje | ID de mensaje a consultar | ***true***

### Query Params

Param | Descripcion | Requerido
----- | ----------- | ---------
page | Pagina a consultar | ***false***
size | Número de elementos a mostrar en la página actual | ***false***

## Envío de Lote de Mensaje de Cobro

```javascript
const axios = require('axios');
const url = 'https://api.qa.scmfix.com/lotes/mc/qr/sync';
const config = {
  headers: {
    'Content-Type': 'application/json',
    'SCM-Token': 'TOKEN GENERADO',
    'SCM-Modo-Ejecucion': 'PR'
  }
}
const body = {
  idLote: 'lote100',
  tipoPago: 20,
  fechaSolicitud: '2012-04-23T18:25:43.511Z',
  lote: [
    {
      idMensaje: "129300-1",
      celularComprador: "5555555555",
      referenciaNumerica: 1234567,
      concepto: "Pago SCM",
      fechaVencimiento: "2012-04-23T18:25:43.511Z",
      monto: 123.45,
      nombreCuentaBeneficairio2: "NA",
      tipoCuentaBeneficiario2: "NA",
      cuentaBeneficiario2: "NA",
      informacionAdicional: {
        campoPrueba: "Prueba2"
      }
    }
  ]
}
axios.post(
  url,
  body,
  config
)
  .then(...)
  .catch(...)
```

> Reponse

```json
{
  "idLote": "lote3",
  "totalMensajes": 12,
  "codigoResultado": 200,
  "textoResultado": "Success",
  "fechaProcesamiento": "2012-04-23T18:25:43.511Z",
}
```

Endpoint que permite el envío de Lotes de Mensajes de cobro de manera masiva

### Headers

Header | Tipo | Requerido | Valor
------ | ---- | --------- | -----
Content-Type | string | ***true*** | application/json
SCM-Token | string | ***true*** | APP_TOKEN
SCM-Modo-Ejecucion | string | ***true*** | BL Ejecución del primer bloque con respuesta correcta dummy<br>UN Primer bloque ejecutado, siguiente en modo BL<br>IN Ejecucion de todos los procesos, sin persistencia de datos<br>PR Producción

### POST Body

Atributo | Descripcion | Tipo
-------- | ----------- | ----
idLote | Id del Lote en el que se generó el QR | ***string***
tipoPago | Tipo de cuenta a la que se realizará el pago | ***number***
fechaSolicitud | Fecha de generación del QR | ***DATE***
lote | Arreglo de objetos del tipo Mensaje Cobro | ***Array***

## Consulta Mensaje de Cobro Síncrono

```javascript
const axios = require('axios');
const url = 'https://api.qa.scmfix.com/lotes/{idLote}/mc/{idMensaje}/sync';
const config = {
  headers: {
    'SCM-Token': 'TOKEN GENERADO',
    'SCM-Modo-Ejecucion': 'PR'
  }
}
axios.post(
  url,
  config
)
  .then(...)
  .catch(...)
```

> Reponse

```json
{
  "idLote": "lote3",
   "idMensaje": "127010-1",
   "estadoMensaje": "Procesado",
   "horaProcesamientoMC": "2012-04-23T18:25:43.511Z",
   "informacionAdicional": { "campoPrueba": "Prueba" },
  "codigoResultado": 200,
  "textoResultado": "Success"
}
```
Endpoint que permite la consulta de mensajes de Cobro Push de manera síncrona.

### HTTP Request

`GET https://api.qa.scmfix.com/lotes/{idLote}/mc/{idMensaje}/sync `

### Headers

Header | Tipo | Requerido | Valor
------ | ---- | --------- | -----
SCM-Token | string | ***true*** | APP_TOKEN
SCM-Modo-Ejecucion | string | ***true*** | BL Ejecución del primer bloque con respuesta correcta dummy<br>UN Primer bloque ejecutado, siguiente en modo BL<br>IN Ejecucion de todos los procesos, sin persistencia de datos<br>PR Producción


### Path Params

Param | Descripcion | Requerido
----- | ----------- | ---------
idLote | ID de Lote a consultar | ***true***
idMensaje | ID de mensaje a consultar | ***true***

## Consulta Mensaje de Cobro Asíncrono

```javascript
const axios = require('axios');
const url = 'https://api.qa.scmfix.com/lotes/{idLote}/mc/{idMensaje}';
const config = {
  headers: {
    'SCM-Token': 'TOKEN GENERADO',
    'SCM-Modo-Ejecucion': 'PR'
  }
}
axios.post(
  url,
  config
)
  .then(...)
  .catch(...)
```

> Reponse

```json
{
  "idLote": "lote3",
   "idMensaje": "127010-1",
   "fechaProcesamiento": "2012-04-23T18:25:43.511Z",
  "codigoResultado": 200,
  "textoResultado": "Success"
}
```
Endpoint que permite la consulta de Mensajes de Cobro Push de manera asíncrona.

### HTTP Request

`GET  https://api.qa.scmfix.com/lotes/{idLote}/mc/{idMensaje}`

### Headers

Header | Tipo | Requerido | Valor
------ | ---- | --------- | -----
SCM-Token | string | ***true*** | APP_TOKEN
SCM-Modo-Ejecucion | string | ***true*** | BL Ejecución del primer bloque con respuesta correcta dummy<br>UN Primer bloque ejecutado, siguiente en modo BL<br>IN Ejecucion de todos los procesos, sin persistencia de datos<br>PR Producción


### Path Params

Param | Descripcion | Requerido
----- | ----------- | ---------
idLote | ID de Lote a consultar | ***true***
idMensaje | ID de mensaje a consultar | ***true***

## Envía Respuesta

```javascript
const axios = require('axios');
const url = '{URL}/resultadosSCM';
const config = {
  headers: {
    'Content-Type': 'application/json',
    'SCM-Token': 'TOKEN GENERADO',
    'SCM-Modo-Ejecucion': 'PR'
  }
}
const body = {
  idLote: 'lote3',
  idMensaje: '1234567891-2',
  estadoMensaje: 'Procesado',
  horaProcesamientoMC: '2012-04-23T18:25:43.511Z',
  informacionAdicional: {campoPrueba: 'Probando'},
  signature: 'ASDFASDFASDFASDFASDFDFASSDFASDASF'

}
axios.post(
  url,
  body,
  config
)
  .then(...)
  .catch(...)
```

> Response

```json
{
  "codigoResultado": 200,
  "textoResultado": "Succes"
}
```
Endpoint que permite el envío de respuesta de un mensaje de cobro 

### HTTP Request

`POST {URL}/resultadoSCM`

### Headers

Header | Tipo | Requerido | Valor
------ | ---- | --------- | -----
Content-Type | string | ***true*** | application/json
SCM-Token | string | ***true*** | APP_TOKEN
SCM-Modo-Ejecucion | string | ***true*** | BL Ejecución del primer bloque con respuesta correcta dummy<br>UN Primer bloque ejecutado, siguiente en modo BL<br>IN Ejecucion de todos los procesos, sin persistencia de datos<br>PR Producción

### POST Body

Atributo | Descripcion | Tipo
-------- | ----------- | ----
idLote | Id del Lote en el que se generó el QR | ***string***
idMensaje | Id del mensaje de cobro asociado al QR generado | ***string***
estadoMensaje | Estado de mensaje de cobro | ***string***
horaProcesamientoMC | Hora de procesamiento de Mensaje de Cobro |  ***DATE***
informacionAdicional | Campos adicionales de informacion | ***json***
signature | Cadena de seguridad | ***string***

## Envía Respuesta QR

```javascript
const axios = require('axios');
const url = '{URL}/resultadosSCM/qr';
const config = {
  headers: {
    'Content-Type': 'application/json',
    'SCM-Token': 'TOKEN GENERADO',
    'SCM-Modo-Ejecucion': 'PR'
  }
}
const body = {
  idLote: 'lote3',
  lista: [
     {
         idMensaje: '1234567891-2',
         estadoMensaje: 'Procesado',
         horaProcesamientoMC: '2012-04-23T18:25:43.511Z',
         informacionAdicional: {campoPrueba: 'Probando'},
     }
  ],
  signature: 'ASDFASDFASDFADSFSDFADSFASDFADSFADSf'
}
axios.post(
  url,
  body,
  config
)
  .then(...)
  .catch(...)
```

> Response

```json
{
  "codigoResultado": 200,
  "textoResultado": "Succes"
}
```
Endpoint que permite el envío de respuesta de QRs

### HTTP Request

`POST {URL}/resultadosSCM/qr`

### Headers

Header | Tipo | Requerido | Valor
------ | ---- | --------- | -----
Content-Type | string | ***true*** | application/json
SCM-Token | string | ***true*** | APP_TOKEN
SCM-Modo-Ejecucion | string | ***true*** | BL Ejecución del primer bloque con respuesta correcta dummy<br>UN Primer bloque ejecutado, siguiente en modo BL<br>IN Ejecucion de todos los procesos, sin persistencia de datos<br>PR Producción

### POST Body

Atributo | Descripcion | Tipo
-------- | ----------- | ----
idLote | Id del Lote en el que se generó el QR | ***string***
lista | Arreglo de objetos de tipo Respuesta Mensaje Cobro | ***Array***
signature | Cadena de seguridad | ***string***

## Generación QR asíncrono

```javascript
const axios = require('axios');
const url = 'https://api.qa.scmfix.com/lotes/mc/qr';
const config = {
  headers: {
    'Content-Type': 'application/json',
    'SCM-Token': 'TOKEN GENERADO',
    'SCM-Modo-Ejecucion': 'PR'
  }
}
const body = {
  idLote: 'lote3',
  idMensaje: '1234567891-2',
  monto: 123.45,
  concepto: 'Pago Codi',
  refrenciaNumerica: 12323,
  tipoPago: 20,
  fechaSolicitud: '2012-04-23T18:25:43.511Z',
  fechaVencimiento: '2012-04-23T18:25:43.511Z',
  informacionAdicional: {
    campoPrueba: 'Probando...'
  },
  nombreBeneficiario2: 'NA',
  tipoCuentaBeneficiario2: 'NA',
  cuentaBeneficiario2: 'NA'
}
axios.post(
  url,
  body,
  config
)
  .then(...)
  .catch(...)
```

> Response

```json
{
   "idLote": "lote3",
   "idMensaje": "1234567891-2",
   "codigoResultado": 200,
   "textoResultado": "Success",
   "fechaProcesamiento": "2012-04-23T18:25:43.511Z",
}
```

Endpoint que permite la generación de QRs de manera asíncrona


### HTTP Request

`POST https://api.qa.scmfix.com/lotes/mc/qr`

### Headers

Header | Tipo | Requerido | Valor
------ | ---- | --------- | -----
Content-Type | string | ***true*** | application/json
SCM-Token | string | ***true*** | APP_TOKEN
SCM-Modo-Ejecucion | string | ***true*** | BL Ejecución del primer bloque con respuesta correcta dummy<br>UN Primer bloque ejecutado, siguiente en modo BL<br>IN Ejecucion de todos los procesos, sin persistencia de datos<br>PR Producción

### POST Body

Atributo | Descripcion | Tipo
-------- | ----------- | ----
idLote | Id del Lote en el que se generó el QR | ***string***
idMensaje | Id del mensaje de cobro asociado al QR generado | ***string***
monto | Monto a cobrar | ***number***
concepto | Concepto de cobro | ***string***
referenciaNumerica | Referencia numérica para la identifcación del cobro | ***number***
tipoPago | Tipo de cuenta a la que se realizará el pago | ***number***
fechaSolicitud | Fecha de generación del QR | ***UNIX DATE***
fechaVencimiento | Fecha límite de vigencia del QR generado | ***UNIX DATE***
informacionAdicional | Campos adicionales de informacion | ***json***
nombreBeneficiario2 | Nombre del beneficiario (Solo en tipo 22) | ***string***
tipoCuentaBeneficiario2 | Tipo de cuenya de beneficiario (Solo en tipo 22) | ***number***
cuentaBeneficiaro2 | Número de cueta de beneficiario (Solo en tipo 22) | ***string***

## Consulta QR asíncrono

```javascript
const axios = require('axios');
const url = 'https://api.qa.scmfix.com/lotes/{idLote}/mc/{idMensaje}/qr?page={page}&amp;size={size}';
const config = {
  headers: {
    'SCM-Token': '************',
    'SCM-Modo-Ejecucion': 'PR'
  },
  params: {
    page: '1',
    size: '10'
  }
}
axios.get(
  url,
  config
)
  .then(...)
  .catch(...)
```

> Response
```json
{
  "idLote": "lote3",
  "idMensaje": "1234567891-2",    
  "codigoResultado": 200,
  "textoResultado": "Success",
  "fechaProcesamientoMC": "2012-04-23T18:25:43.511Z"
}
```
Endpoint que permite la consulta de QRs de manera asíncrona

### HTTP Request

`GET https://api.qa.scmfix.com/lotes/{idLote}/mc/{idMensaje}/qr?page={page}&amp;size={size}`

### Headers

Header | Tipo | Requerido | Valor
------ | ---- | --------- | -----
SCM-Token | string | ***true*** | APP_TOKEN
SCM-Modo-Ejecucion | string | ***true*** | BL Ejecución del primer bloque con respuesta correcta dummy<br>UN Primer bloque ejecutado, siguiente en modo BL<br>IN Ejecucion de todos los procesos, sin persistencia de datos<br>PR Producción

### Path Params

Param | Descripcion | Requerido
----- | ----------- | ---------
idLote | ID de Lote a consultar | ***true***
idMensaje | ID de mensaje a consultar | ***true***

### Query Params

Param | Descripcion | Requerido
----- | ----------- | ---------
page | Pagina a consultar | ***false***
size | Número de elementos a mostrar en la página actual | ***false***

# Objetos Comunes 

La siguiente sección es para presentar la definición de los objetos utilizados.

## Generación QR

```
{
  idLote: 'lote100',
  idMensaje: 'mensaje1',
  monto: 345.50,
  concepto: 'pago',
  referenciaNumerica: 123456789,
  tipoPago: 20,
  fechaSolicitud: '1573590949',
  fechaVencimiento: '1573590949',
  informacionAdicional: {
    campoPrueba: 'Probando',
    .
    .
    .
  },
  nombreBeneficiario2: 'NA',
  tipoCuentaBeneficiario2: 1,
  cuentaBeneficiario2: '123456789'
}
```

Atributo | TipoDato
-------- | --------
idLote | ***string***
idMensaje | ***string***
monto | ***number***
concepto | ***string***
referenciaNumerica | ***number***
tipoPago | ***number***
fechaSolicitud | ***string***
fechaVencimiento | ***string***
informacionAdicional | ***object***
nombreBeneficiario2 | ***string***
tipoCuentaBeneficiario2 | ***number*** <br> ***string*** 'NA' en caso que no aplique
cuentaBeneficiario2 | ***number*** <br> ***string*** 'NA' en caso que no aplique

## Response Consulta QR Sincrono 

```
{
  idLote: 'lote3',
  lista: [ ... ],
  size: 5,
  page: 5,
  codigoResultado: 200,
  textoResultado: 'Success'
}
```

Atributo | TipoDato
-------- | --------
idLote | ***string***
lista | [***Array***](#generación-qr)
size | ***number***
page | ***number***
codigoResuktado | ***number***
textoResultado | ***string***

## Envio de Lote 

```
{
  idLote: 'lote3',
  tipoPago: 20,
  fechaSolicitud: '2019-12-17T18:25:43.511Z',
  lote: [
    ...
  ]
}
```

Atributo | TipoDato
-------- | --------
idLote | ***string***
lote | [***Array***](#generación-qr)
tipoPago | ***number***
fechaSolicitud | ***DATE***

## Request Envio Respuesta

```
{
  idLote: 'lote3',
  idMensaje: 'mensaje2',
  estadoMensaje: 'Procesado',
  horaProcesamientoMC: '2019-12-17T18:25:43.511Z',
  informacionAdicional: [
    ...
  ],
  signature: 'asdfghj'
}
```

Atributo | TipoDato
-------- | --------
idLote | ***string***
idMensaje | ***string***
estadoMensaje | ***string***
horaProcesamientoMC | ***DATE***
informacionAdicional | ***object***
signature | ***string***

## Request Envia Respuesta QR

```
{
  idLote: 'lote3',
  lista: [...],
  signature: 'asdfghj'
}
```

Atributo | TipoDato
-------- | --------
idLote | ***string***
lista | [***Array***](#request-envio-respuesta)
signature | ***string***

## Request Generacion QR Asincrono

```
{
  idLote: 'lote3',
  idMensaje: '1234567891-2',
  monto: 123.45,
  concepto: 'Pago Codi',
  referenciaNumerica: 12323,
  tipoPago: 20,
  fechaSolicitud: '2012-04-23T18:25:43.511Z',
  fechaVencimiento: '2012-04-23T18:25:43.511Z',
  informacionAdicional: {
    campoPrueba: 'Probando...'
  },
  nombreBeneficiario2: 'NA',
  tipoCuentaBeneficiario2: 'NA',
  cuentaBeneficiario2: 'NA'
}
```
Atributo | Tipo
-------- | ----
idLote | ***string***
idMensaje | ***string***
monto | ***number***
concepto |  ***string***
referenciaNumerica | ***number***
tipoPago | ***number***
fechaSolicitud | ***DATE***
fechaVencimiento | ***DATE***
informacionAdicional | ***object***
nombreBeneficiario2 |  ***string***
tipoCuentaBeneficiario2 | ***number***
cuentaBeneficiaro2 | ***string***


