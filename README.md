# Finanzas personales

Base de datos, Finanzas personales.

## Repositorio

```bash
git clone https://github.com/igmr/db-finance.git
```

## Modelo relacional

![img-modelo-relacional](/docs/Model-ER.png)

## Script MySQL

<details>

<summary>Script SQL para MySQL</summary>

Ejecutar base de datos desde terminal.

```bash
mysql -u <user_name> -p<password> < <path/script/sql>
```

```sql

-- ----------------------------------------
-- Database dbFinza
-- ----------------------------------------
DROP DATABASE IF EXISTS dbFinza;
CREATE DATABASE IF NOT EXISTS dbFinza;
USE dbFinza;

-- ----------------------------------------
-- Table users
-- ----------------------------------------
DROP TABLE IF EXISTS users;
CREATE TABLE IF NOT EXISTS users
(
  id            INT          NOT NULL AUTO_INCREMENT,
  first_name    VARCHAR(45)  NOT NULL,
  last_name     VARCHAR(45)  NOT NULL,
  user          VARCHAR(65)  NOT NULL,
  password      VARCHAR(255) NOT NULL,
  avatar        VARCHAR(255) NOT NULL,
  created_date  TIMESTAMP    NOT NULL DEFAULT NOW(),
  updated_date  TIMESTAMP        NULL DEFAULT NULL,
  deleted_date  TIMESTAMP        NULL DEFAULT NULL,
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
  created_date   TIMESTAMP    NOT NULL DEFAULT NOW(),
  updated_date   TIMESTAMP        NULL DEFAULT NULL,
  deleted_date   TIMESTAMP        NULL DEFAULT NULL,
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
  created_date   TIMESTAMP        NOT NULL DEFAULT NOW(),
  updated_date   TIMESTAMP            NULL DEFAULT NULL,
  deleted_date   TIMESTAMP            NULL DEFAULT NULL,
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
  created_date   TIMESTAMP    NOT NULL DEFAULT NOW(),
  updated_date   TIMESTAMP        NULL DEFAULT NULL,
  deleted_date   TIMESTAMP        NULL DEFAULT NULL,
  CONSTRAINT pkCategory      PRIMARY KEY(id),
  CONSTRAINT ukCategory      UNIQUE(description)
);

-- ----------------------------------------
-- Table subcategories
-- ----------------------------------------
DROP TABLE IF EXISTS subcategories;
CREATE TABLE IF NOT EXISTS subcategories
(
  id             INT           NOT NULL AUTO_INCREMENT,
  user_id        INT           NOT NULL DEFAULT 0,
  category_id    INT           NOT NULL DEFAULT 1,
  description    VARCHAR(255)  NOT NULL,
  icon           CHAR(45)          NULL DEFAULT NULL,
  created_date   TIMESTAMP     NOT NULL DEFAULT NOW(),
  updated_date   TIMESTAMP         NULL DEFAULT NULL,
  deleted_date   TIMESTAMP         NULL DEFAULT NULL,
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
  created_date   TIMESTAMP    NOT NULL DEFAULT NOW(),
  updated_date   TIMESTAMP        NULL DEFAULT NULL,
  deleted_date   TIMESTAMP        NULL DEFAULT NULL,
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
  created_date   TIMESTAMP    NOT NULL DEFAULT NOW(),
  updated_date   TIMESTAMP        NULL DEFAULT NULL,
  deleted_date   TIMESTAMP        NULL DEFAULT NULL,
  CONSTRAINT pkEgress             PRIMARY KEY(id),
  CONSTRAINT fkEgressSubcategory  FOREIGN KEY(subcategory_id) REFERENCES categories(id),
  CONSTRAINT fkEgressSubAccount   FOREIGN KEY(subaccount_id) REFERENCES  subaccounts(id)
);

```

</details>
