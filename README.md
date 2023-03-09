# Finanzas personales

Base de datos, Finanzas personales.

## Repositorio

```bash
git clone https://github.com/igmr/db-finance.git
```

## Modelo entidad relación

![img-modelo-entidad-relación](/docs/Model-ER.png)

## Script MySQL

```sql

-- ----------------------------------------
-- Database Finzadb
-- ----------------------------------------
DROP DATABASE IF EXISTS Finzadb;
CREATE DATABASE IF NOT EXISTS Finzadb;
USE Finzadb;

-- ----------------------------------------
-- Table users
-- ----------------------------------------
DROP TABLE IF EXISTS users;
CREATE TABLE IF NOT EXISTS users
(
  id          INT          NOT NULL AUTO_INCREMENT,
  first_name  VARCHAR(45)  NOT NULL,
  last_name   VARCHAR(45)  NOT NULL,
  user        VARCHAR(65)  NOT NULL,
  password    VARCHAR(255) NOT NULL,
  avatar      VARCHAR(255) NOT NULL,
  created_at  TIMESTAMP    NOT NULL DEFAULT NOW(),
  updated_at  TIMESTAMP        NULL DEFAULT NULL,
  deleted_at  TIMESTAMP        NULL DEFAULT NULL,
  CONSTRAINT pkUser        PRIMARY KEY(id),
  CONSTRAINT ukUser        UNIQUE(user)
);

-- ----------------------------------------
-- Table settings
-- ----------------------------------------
DROP TABLE IF EXISTS settings;
CREATE TABLE IF NOT EXISTS settings
(
  id               INT      NOT NULL AUTO_INCREMENT,
  show_all         BOOLEAN  NOT NULL DEFAULT FALSE,
  allow_negatives  BOOLEAN  NOT NULL DEFAULT FALSE,
  CONSTRAINT pkSetting      PRIMARY KEY(id)
);

-- ----------------------------------------
-- Table accounts
-- ----------------------------------------
DROP TABLE IF EXISTS accounts;
CREATE TABLE IF NOT EXISTS accounts
(
  id             INT          NOT NULL AUTO_INCREMENT,
  user_id        INT          NOT NULL DEFAULT 0,
  description    VARCHAR(255) NOT NULL,
  icon           CHAR(45)         NULL DEFAULT NULL,
  amount_ingress DOUBLE       NOT NULL DEFAULT 0,
  amount_egress  DOUBLE       NOT NULL DEFAULT 0,
  balance        DOUBLE       NOT NULL DEFAULT 0,
  created_at     TIMESTAMP    NOT NULL DEFAULT NOW(),
  updated_at     TIMESTAMP        NULL DEFAULT NULL,
  deleted_at     TIMESTAMP        NULL DEFAULT NULL,
  CONSTRAINT pkAccount        PRIMARY KEY(id),
  CONSTRAINT ukAccount        UNIQUE(description)
);

-- ----------------------------------------
-- Table subaccounts
-- ----------------------------------------
DROP TABLE IF EXISTS subaccounts;
CREATE TABLE IF NOT EXISTS subaccounts
(
  id             INT              NOT NULL AUTO_INCREMENT,
  account_id     INT              NOT NULL DEFAULT 1,
  user_id        INT              NOT NULL DEFAULT 0,
  description    VARCHAR(255)     NOT NULL,
  icon           CHAR(45)             NULL DEFAULT NULL,
  amount_ingress DOUBLE           NOT NULL DEFAULT 0,
  amount_egress  DOUBLE           NOT NULL DEFAULT 0,
  balance        DOUBLE           NOT NULL DEFAULT 0,
  created_at     TIMESTAMP        NOT NULL DEFAULT NOW(),
  updated_at     TIMESTAMP            NULL DEFAULT NULL,
  deleted_at     TIMESTAMP            NULL DEFAULT NULL,
  CONSTRAINT pkSubaccount         PRIMARY KEY(id),
  CONSTRAINT ukSubaccount         UNIQUE(description),
  CONSTRAINT fkAccountSubaccount  FOREIGN KEY(account_id) REFERENCES accounts(id)
);

-- ----------------------------------------
-- Table categories
-- ----------------------------------------
DROP TABLE IF EXISTS categories;
CREATE TABLE IF NOT EXISTS categories
(
  id             INT          NOT NULL AUTO_INCREMENT,
  user_id        INT          NOT NULL DEFAULT 0,
  description    VARCHAR(255) NOT NULL,
  icon           CHAR(45)         NULL DEFAULT NULL,
  amount_ingress DOUBLE       NOT NULL DEFAULT 0,
  amount_egress  DOUBLE       NOT NULL DEFAULT 0,
  balance        DOUBLE       NOT NULL DEFAULT 0,
  created_at     TIMESTAMP    NOT NULL DEFAULT NOW(),
  updated_at     TIMESTAMP        NULL DEFAULT NULL,
  deleted_at     TIMESTAMP        NULL DEFAULT NULL,
  CONSTRAINT pkCategory      PRIMARY KEY(id),
  CONSTRAINT ukCategory      UNIQUE(description)
);

-- ----------------------------------------
-- Table subcategories
-- ----------------------------------------
DROP TABLE IF EXISTS subCategories;
CREATE TABLE IF NOT EXISTS subCategories
(
  id             INT           NOT NULL AUTO_INCREMENT,
  user_id        INT           NOT NULL DEFAULT 0,
  category_id    INT           NOT NULL DEFAULT 1,
  description    VARCHAR(255)  NOT NULL,
  icon           CHAR(45)          NULL DEFAULT NULL,
  amount_ingress DOUBLE        NOT NULL DEFAULT 0,
  amount_egress  DOUBLE        NOT NULL DEFAULT 0,
  balance        DOUBLE        NOT NULL DEFAULT 0,
  created_at     TIMESTAMP     NOT NULL DEFAULT NOW(),
  updated_at     TIMESTAMP         NULL DEFAULT NULL,
  deleted_at     TIMESTAMP         NULL DEFAULT NULL,
  CONSTRAINT pkSubCategory         PRIMARY KEY(id),
  CONSTRAINT ukSubCategory         UNIQUE(description),
  CONSTRAINT fkCategorySubcategory FOREIGN KEY(category_id) REFERENCES categories(id)
);

-- ----------------------------------------
-- Table ingress
-- ----------------------------------------
DROP TABLE IF EXISTS ingress;
CREATE TABLE IF NOT EXISTS ingress
(
  id             INT          NOT NULL AUTO_INCREMENT,
  user_id        INT          NOT NULL DEFAULT 0,
  subcategory_id INT          NOT NULL DEFAULT 1,
  subaccount_id  INT          NOT NULL DEFAULT 1,
  amount         DOUBLE           NULL DEFAULT 0,
  observation    VARCHAR(255) NOT NULL,
  created_at     TIMESTAMP    NOT NULL DEFAULT NOW(),
  updated_at     TIMESTAMP        NULL DEFAULT NULL,
  deleted_at     TIMESTAMP        NULL DEFAULT NULL,
  CONSTRAINT pkIngress             PRIMARY KEY(id),
  CONSTRAINT fkIngressSubcategory FOREIGN KEY(subcategory_id) REFERENCES categories(id),
  CONSTRAINT fkIngressSubAccount  FOREIGN KEY(subaccount_id) REFERENCES  subaccounts(id)
);

-- ----------------------------------------
-- Table egress
-- ----------------------------------------
DROP TABLE IF EXISTS egress;
CREATE TABLE IF NOT EXISTS egress
(
  id             INT          NOT NULL AUTO_INCREMENT,
  user_id        INT          NOT NULL DEFAULT 0,
  subcategory_id INT          NOT NULL DEFAULT 1,
  subaccount_id  INT          NOT NULL DEFAULT 1,
  amount         DOUBLE           NULL DEFAULT 0,
  observation    VARCHAR(255) NOT NULL,
  created_at     TIMESTAMP    NOT NULL DEFAULT NOW(),
  updated_at     TIMESTAMP        NULL DEFAULT NULL,
  deleted_at     TIMESTAMP        NULL DEFAULT NULL,
  CONSTRAINT pkEgress             PRIMARY KEY(id),
  CONSTRAINT fkEgressSubcategory  FOREIGN KEY(subcategory_id) REFERENCES categories(id),
  CONSTRAINT fkEgressSubAccount   FOREIGN KEY(subaccount_id) REFERENCES  subaccounts(id)
);

-- ----------------------------------------
-- Inserts categories
-- ----------------------------------------
INSERT INTO categories(id, description)
VALUES
  (001,  'Ingreso'               ),
  (002,  '(Ninguno)'             ),
  (003,  'Vivienda'              ),
  (004,  'Comida'                ),
  (005,  'Impuestos y donaciones'),
  (006,  'Transporte'            ),
  (007,  'Seguros'               ),
  (008,  'Ahorros e Inversiones' ),
  (009,  'Servicios'             ),
  (010,  'Salud'                 ),
  (011,  'Vestimenta'            ),
  (012,  'Recreación'            ),
  (013,  'Personal'              ),
  (014,  'Deudas'                );

-- ----------------------------------------
-- Inserts subcategories
-- ----------------------------------------
INSERT INTO subcategories(id, category_id, description)
VALUES
  (001,  001,  'Ingreso'                        ),

  (002,  002,  '(Ninguno)'                      ),

  (003,  003,  'Hipoteca Uno/Renta'             ),
  (004,  003,  'Hipoteca Dos'                   ),
  (005,  003,  'Impuestos de vivienda'          ),
  (006,  003,  'Reparaciones/Mantenimiento'     ),
  (007,  003,  'Gastos de administración'       ),

  (008,  004,  'Despensa'                       ),
  (009,  004,  'Restaurantes'                   ),

  (010,  005,  'Impuestos'                      ),
  (011,  005,  'Donaciones'                     ),

  (012,  006,  'Gasolina & Aceite'              ),
  (013,  006,  'Reparación & Llantas'           ),
  (014,  006,  'Licencia & Impuestos'           ),
  (015,  006,  'Reemplazo de Auto'              ),
  (016,  006,  'Transporte público'             ),

  (017,  007,  'Seguro de vida'                 ),
  (018,  007,  'Seguro de gastos médicos'       ),
  (019,  007,  'Seguro de vivienda'             ),
  (020,  007,  'Seguro de auto'                 ),
  (021,  007,  'Seguro de incapacidad'          ),
  (022,  007,  'Seguro contra robo'             ),
  (023,  007,  'Cuidados a largo plazo'         ),

  (024,  008,  'Fondo de emergencias'           ),
  (025,  008,  'Fondo para el retiro'           ),
  (026,  008,  'Fondo para estudios'            ),

  (027,  009,  'Electricidad'                   ),
  (028,  009,  'Gas'                            ),
  (029,  009,  'Agua'                           ),
  (030,  009,  'Servicios de limpieza'          ),
  (031,  009,  'Teléfono'                       ),
  (032,  009,  'Internet'                       ),
  (033,  009,  'Cable'                          ),

  (034,  010,  'Medicamentos'                   ),
  (035,  010,  'Médicos'                        ),
  (036,  010,  'Dentista'                       ),
  (037,  010,  'Optometrista'                   ),
  (038,  010,  'Vitaminas'                      ),
  (039,  010,  'Salud otros 1'                  ),
  (040,  010,  'Salud otros 2'                  ),

  (041,  011,  'Adultos'                        ),
  (042,  011,  'Niños'                          ),
  (043,  011,  'Limpieza'                       ),

  (044,  012,  'Entretenimiento'                ),
  (045,  012,  'Vacaciones'                     ),

  (046,  013,  'Guardería/Niñera'               ),
  (047,  013,  'Artículos de tocador'           ),
  (048,  013,  'Cosméticos/Cuidado del cabello' ),
  (049,  013,  'Educación/Colegiatura'          ),
  (050,  013,  'Libros/Útiles'                  ),
  (061,  013,  'Manutención'                    ),
  (062,  013,  'Pensión alimenticia'            ),
  (063,  013,  'Suscripciones'                  ),
  (064,  013,  'Gastos de organización'         ),
  (065,  013,  'Regalos'                        ),
  (066,  013,  'Reemplazar muebles'             ),
  (067,  013,  'Dinero de bolsillo (de el)'     ),
  (068,  013,  'Dinero de bolsillo (de ella)'   ),
  (069,  013,  'Artículos para bebé'            ),
  (070,  013,  'Artículos para mascota'         ),
  (071,  013,  'Música/Tecnología'              ),
  (072,  013,  'Varios'                         ),
  (073,  013,  'Contador'                       ),
  (074,  013,  'Personal otro 1'                ),
  (075,  013,  'Personal otro 2'                ),

  (076,  014,  'Pago de auto 1'                 ),
  (077,  014,  'Pago de auto 2'                 ),
  (078,  014,  'Tarjeta de crédito 1'           ),
  (079,  014,  'Tarjeta de crédito 2'           ),
  (080,  014,  'Tarjeta de crédito 3'           ),
  (081,  014,  'Tarjeta de crédito 4'           ),
  (082,  014,  'Tarjeta de crédito 5'           ),
  (083,  014,  'Préstamo estudiantil 1'         ),
  (084,  014,  'Préstamo estudiantil 2'         ),
  (085,  014,  'Préstamo estudiantil 3'         ),
  (086,  014,  'Préstamo estudiantil 4'         ),
  (087,  014,  'Deuda otro 1'                   ),
  (088,  014,  'Deuda otro 2'                   ),
  (089,  014,  'Deuda otro 3'                   ),
  (090,  014,  'Deuda otro 4'                   ),
  (091,  014,  'Deuda otro 5'                   );

-- ----------------------------------------
-- Inserts account
-- ----------------------------------------
INSERT INTO accounts(description)
VALUES ('Efectivo');

-- ----------------------------------------
-- Inserts subaccount
-- ----------------------------------------
INSERT INTO subaccounts(account_id,description)
VALUES (1, 'Efectivo');

-- ----------------------------------------
-- Inserts Settings
-- ----------------------------------------
INSERT INTO settings (show_all,allow_negatives)
VALUES (FALSE, FALSE);

```
