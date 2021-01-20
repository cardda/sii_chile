---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - json

toc_footers:
  - <a href='https://github.com/slatedocs/slate'>Documentation Powered by Slate</a>

includes:

search: true

code_clipboard: true
---

# Introducción

## La API de Cardda Crawler

Bienvenido a la documentación de la API de Cardda Crawler! 👏

Puedes usar esta API para acceder a endpoints que permiten
automatizar tareas de emisión de facturas en el SII. 

# Facturas de venta electrónica

En esta sección verás los endpoints disponibles para generar facturas electrónicas de manera
automática. 

## Endpoints

### Generar un preview

Generar una visualización de una factura de venta electrónica sin emitirla.

| Método | Endpoint               | 
|--------|------------------------|
| `POST` | `/sii_preview_invoice` |

Si la operación es exitosa, retorna una captura
con los datos rellenados en el formulario del SII.

#### Parámetros

Los parámetros de este endpoint están
definidos en [Parámetros comunes](#factura-parametros-comunes)

### Emitir una factura

Emitir una factura de venta electrónica en el SII.

| Método | Endpoint              |
|--------|-----------------------|
| `POST` | `/sii_create_invoice` |

Si la operación es exitosa, retorna un JSON
con la `url` del pdf de la factura.

#### Parámetros

Los parámetros de este endpoint están
definidos en [Parámetros comunes](#factura-parametros-comunes)

## <span style="display: none">Factura-</span>Parámetros comunes

Los parámetros siguientes son comunes a todos los endpoints
de facturas de venta electrónicas.

> Ejemplo

```json
{
  "user": "12.345.678-9",
  "password": "pass",
  "certificate_password": "1234",
  "rut": "987654321",
  "phone": "56912345678",
  "contact_name": "Tulio Triviño",
  "charges": [
    {
      "name": "Servicio digital",
      "description": "Prestación de servicio de pago y gestión en Amazon AWS",
      "unit": "USDCLP",
      "price": "733.73",
      "amount": "121.12"
    }
  ]
}
```

| Parámetro              | Tipo              | Descripción                                                     |
|------------------------|-------------------|-----------------------------------------------------------------|
| `user`                 | **string**        | RUT del usuario del SII. Debe estar escrito con puntos y guión. |
| `password`             | **string**        | Contraseña del usuario del SII.                                 |
| `certificate_password` | **string**        | Contraseña para firmar facturas.                                |
| `rut`                  | **string**        | RUT del receptor de la factura de venta. No debe tener puntos ni guión.                             |
| `phone`                | **string**        | Teléfono de contacto del receptor de la factura de venta.                                           |
| `contact_name`         | **string**        | Nombre de contacto del receptor de la factura de venta.                                            |
| `charges`              | **array[Charge]** | Lista de cargos para cada producto. Ver [Charge](#charge)                                                |

## Definición de objectos

### Charge

Objeto correspondiente a los datos de un producto de 
una factura electrónica del SII.

> Ejemplo

```json
{
  "name": "Servicio digital",
  "description": "Prestación de servicio de pago y gestión en Amazon AWS",
  "unit": "USDCLP",
  "price": "733.73",
  "amount": "121.12"
}
```

| Atributo     | Tipo       | Descripción                        |
|---------------|------------|------------------------------------|
| `name`        | **string** | Nombre del producto.               |
| `description` | **string** | Descripción del producto.          |
| `unit`        | **string** | Tipo de unidad del producto.       |
| `price`       | **double** | Precio de una unidad del producto. |
| `amount`      | **double** | Cantidad de unidades del producto. |

# Facturas de compra electrónica

Cardda Crawler no solo puede emitir facturas de venta,
sino también facturas de compra a servicios extranjeros, los endpoints descritos acá
explican como puedes hacerlo.

## Endpoints

### Generar un preview

Generar una visualización de una factura de compra electrónica sin emitirla.

| Método | Endpoint                 |
|--------|--------------------------|
|`POST`  |`/sii_preview_buy_invoice`|

Si la operación es exitosa, retorna una captura
con los datos rellenados en el formulario del SII.

#### Parámetros

Los parámetros de este endpoint están definidos
en [Parámetros comunes](#factura-compra-parametros-comunes)

### Emitir una factura

Emitir una factura de compra electrónica en el SII.

| Método | Endpoint                 |
|--------|--------------------------|
|`POST`  |`/sii_create_buy_invoice`|

Si la operación es exitosa, retorna un JSON
con la `url` del pdf de la factura.

#### Parámetros

Los parámetros de este endpoint están
definidos en [Parámetros comunes](#factura-compra-parametros-comunes)

## <span style="display: none">Factura-Compra-</span>Parámetros comunes

Los parámetros siguientes son comunes
a todos los endpoints de facturas de compra electrónicas.

> Ejemplo

```json
{
    "user": "12.345.678-9",
    "password": "password",
    "certificate_password": "1234",
    "description": "Invoice emitida el 05/01/2021, $10.341 CLP",
    "service": {
      "rut": "59292930",
      "dv": "9",
      "address": "410 Terry Avenue North",
      "comuna": "Seattle",
      "city": "Seattle",
      "business_name": "AMAZON WEB SERVICES, INC."
    },
    "invoice": {
        "day": "05",
        "month": "01",
        "year": "2021",
        "chargeClp": "10341"
    }
}
```

| Parámetro              | Tipo         | Descripción                                                                            |
|------------------------|--------------|----------------------------------------------------------------------------------------|
| `user`                 | **string**   | RUT del usuario del SII. Debe estar escrito con puntos y guión.                        |
| `password`             | **string**   | Contraseña del usuario del SII.                                                        |
| `certificate_password` | **string**   | Contraseña para firmar facturas.                                                       |
| `description`          | **string**   | Descripción de la factura a emitir.                                                    |
| `service`              | **Business** | Información del servicio para el cual se hace esta factura. Ver [Business](#business). |
| `invoice`              | **Invoice**  | Información de la factura original extranjera. Ver [Invoice](#invoice).                |

## Definición de objetos

### Business

Objeto que describe la información de un servicio
dado por terceros.

> Ejemplo

```json
{
  "rut": "59292930",
  "dv": "9",
  "address": "410 Terry Avenue North",
  "comuna": "Seattle",
  "city": "Seattle",
  "business_name": "AMAZON WEB SERVICES, INC."
}
```

| Atributo        | Tipo       | Descripción                                                     |
|-----------------|------------|-----------------------------------------------------------------|
| `rut`           | **string** | RUT de la empresa. No debe llevar puntos ni dígito verificador. |
| `dv`            | **string** | Dígito verificador del RUT de la empresa.                       |
| `address`       | **string** | Dirección legal de la empresa.                                  |
| `comuna`        | **string** | Comuna en la que está ubicada la empresa.                       |
| `city`          | **string** | Ciudad en la que está ubicada la empresa.                       |
| `business_name` | **string** | Razón social de la empresa.                                     |

#### Nota

El nombre `comuna` no tiene un equivalente en inglés, por lo que decidimos no traducirlo y utilizar el mismo valor que en `city` para servicios extranjeros.

### Invoice

Objeto que describe datos de una factura extranjera.

> Ejemplo

```json
{
  "day": "01",
  "month": "01",
  "year": "2021",
  "chargeClp": "90548"
}
```

| Atributo    | Tipo       | Descripción                                                                                                                                                   |
|-------------|------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `day`       | **string** | Día en que se emitió la factura. Debe tener dos dígitos, si es menor a 10 se debe rellenar con 0 al principio.                                                |
| `month`     | **string** | Mes en que se emitió la factura. Debe tener dos dígitos, si es menor a 10 se debe rellenar con 0 al principio.                                                |
| `year`      | **string** | Año en que se emitió la factura.                                                                                                                              |
| `chargeClp` | **string** | Monto de la factura en pesos chilenos. Si la factura es en dólares, este monto debe corresponder al valor del dólar en el día posterior a la emisión de esta. |
