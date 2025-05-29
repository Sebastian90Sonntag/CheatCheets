# 🛢️ SQL Cheat Sheet – Syntax, Tools & Best Practices

Ein umfassender Überblick über SQL: von Grundsyntax, Abfragen, Sicherheit, Triggern und Migration bis zu Optimierung, Replikation, ORMs und Systemvergleichen (MySQL, MariaDB, PostgreSQL).

---

## 📌 Grundlegende SQL-Befehle

```sql
SELECT * FROM users;
INSERT INTO users (name, email) VALUES ('Max', 'max@mail.com');
UPDATE users SET last_login = NOW() WHERE id = 1;
DELETE FROM users WHERE inactive = 1;
```

## 🔗 Joins & Aggregationen

```sql
-- INNER JOIN
SELECT u.name, o.total FROM users u JOIN orders o ON u.id = o.user_id;

-- LEFT JOIN
SELECT u.name, o.total FROM users u LEFT JOIN orders o ON u.id = o.user_id;

-- GROUP BY
SELECT country, COUNT(*) AS user_count FROM users GROUP BY country HAVING COUNT(*) > 10;
```

## 🧱 Views, Indexe & Constraints

```sql
CREATE VIEW active_users AS SELECT * FROM users WHERE active = 1;
CREATE INDEX idx_email ON users(email);
CREATE TABLE emails (id INT PRIMARY KEY, address VARCHAR(255) NOT NULL UNIQUE);
```

## 🔄 Trigger & Stored Procedures

```sql
-- Trigger
CREATE TRIGGER log_insert AFTER INSERT ON users FOR EACH ROW INSERT INTO log (action, timestamp) VALUES ('inserted', NOW());

-- Stored Procedure (MySQL)
DELIMITER $$
CREATE PROCEDURE getUserCount()
BEGIN
  SELECT COUNT(*) FROM users;
END $$
DELIMITER ;

CALL getUserCount();
```

## 🗺️ Datenmodellierung (ER-Diagramme)

* Entity = Tabelle, Relation = Fremdschlüssel
* Kardinalitäten: 1:1, 1\:n, n\:m
* Tools: dbdiagram.io, SQLDBM, DrawSQL, DBeaver

## 🌐 NoSQL vs. SQL Überblick

| Kriterium     | SQL (PostgreSQL) | NoSQL (MongoDB)          |
| ------------- | ---------------- | ------------------------ |
| Schema        | Fix              | Flexibel                 |
| Abfragen      | SQL              | JSON-Query               |
| Transaktionen | ACID             | BASE (ab Mongo 4.0 ACID) |

```js
// MongoDB-Beispiel:
Users.find({ active: true, projects: { $exists: true } });
```

## 🔐 Sicherheit in SQL

* Prepared Statements nutzen, nie `eval` oder dynamisches SQL
* Rollen und Rechte differenziert vergeben (`GRANT`, `REVOKE`)
* Keine Adminrechte für API-User
* Validierung client- und serverseitig
* Logging, Auditing und CSP nutzen

## ⚖️ Vergleich MySQL, MariaDB, PostgreSQL

| Feature             | MySQL   | MariaDB | PostgreSQL  |
| ------------------- | ------- | ------- | ----------- |
| JSON                | Basis   | Gut     | Vollwertig  |
| CTE / WITH          | ab v8.0 | ab 10.2 | Ja          |
| Transaktionen       | Ja      | Ja      | Vollständig |
| Materialized Views  | Nein    | Nein    | Ja          |
| Window Functions    | Ja      | Ja      | Ja          |
| Geo/Spatial         | Ja      | Ja      | PostGIS     |
| Standardkonformität | Mittel  | Gut     | Hoch        |

## 🚀 Performance-Tuning

* Nutze `EXPLAIN` zur Abfrageoptimierung
* Verwende `WHERE` mit indizierten Spalten
* Caching: Redis/Memcached
* Connection-Pooling (pgBouncer, ProxySQL)
* Monitoring: pgAdmin, Percona, Grafana

## 💾 Replikation & Backup

* Replikation: MySQL (Binlog), PostgreSQL (Streaming, Logical)
* Backup: `mysqldump`, `pg_dump`, `pg_basebackup`, XtraBackup
* Automatisierung mit `cron`, `systemd`, `CI/CD`

```bash
pg_dump -Fc -U postgres mydb > mydb.dump
pg_restore -l mydb.dump
```

## 📥 Datenmigration & CSV-Import

### CSV-Import in MySQL

```sql
LOAD DATA INFILE '/pfad/zur/datei.csv'
INTO TABLE users
FIELDS TERMINATED BY ',' ENCLOSED BY '"'
LINES TERMINATED BY '\n'
IGNORE 1 ROWS;
```

### PostgreSQL

```sql
COPY users(name, email)
FROM '/pfad/zur/datei.csv'
DELIMITER ',' CSV HEADER;
```

Oder lokal über `\copy` in `psql`:

```bash
\copy users FROM 'users.csv' DELIMITER ',' CSV HEADER;
```

### Tools

| Tool            | Beschreibung                  |
| --------------- | ----------------------------- |
| DBeaver         | GUI für DB-Import/Export      |
| pgAdmin         | CSV-Import für PostgreSQL     |
| MySQL Workbench | Wizard für CSV/Excel          |
| Pentaho Kettle  | Grafisches ETL-Werkzeug       |
| Apache NiFi     | Datenfluss-Management mit GUI |

### Tipps

* Zeichensatz auf UTF-8 prüfen
* Temporäre Validierungstabellen verwenden
* `STRICT`-Modus und Fehlerprotokoll nutzen

## 🧰 Datenbankdesign-Patterns

* **Single Table Inheritance**: eine Tabelle für viele Entitäten (Polymorphismus)
* **Table per Type**: separate Tabellen mit 1:1-Joins
* **Table per Concrete Class**: jede Klasse/Tabelle ist eigenständig
* **Audit Trail Pattern**: Logging-Tabellen per Trigger (History pro Entität)
* **Soft Deletes**: `deleted_at`-Spalte statt Löschung
* **Multi-Tenancy**: Tenant-ID als Schlüssel oder separate DBs/Schemas
* **Temporal Tables**: Historienverwaltung per Versionierungsspalte

## 🔧 ORMs: Prisma, Sequelize, TypeORM

### Prisma (Node.js, TypeScript)

```prisma
model User {
  id    Int     @id @default(autoincrement())
  name  String
  email String  @unique
}
```

```ts
const users = await prisma.user.findMany({ where: { active: true } });
```

* Schema-first
* CLI & Introspection
* Migrationen via `prisma migrate`

### Sequelize (Node.js)

```js
const User = sequelize.define('User', {
  name: Sequelize.STRING,
  email: Sequelize.STRING
});
await User.findAll({ where: { active: true } });
```

* Dynamisches Model-Definition
* Unterstützt Hooks, Validierung, Assocs

### TypeORM (TS/JS, aktiv in NestJS)

```ts
@Entity()
export class User {
  @PrimaryGeneratedColumn()
  id: number;

  @Column()
  name: string;
}
```

```ts
await connection.getRepository(User).find();
```

* Decorator-basiert (ähnlich Angular)
* CLI & Migration Tools
* Integriert in NestJS mit `@nestjs/typeorm`

---

## 📚 Ressourcen

* [sqltutorial.org](https://www.sqltutorial.org/)
* [postgresql.org/docs](https://www.postgresql.org/docs/)
* [mariadb.com/kb](https://mariadb.com/kb)
* [mysql.com/doc](https://dev.mysql.com/doc/)
* [sqlzoo.net](https://sqlzoo.net)
* [Mode SQL Tutorial](https://mode.com/sql-tutorial/)
* [dbdiagram.io](https://dbdiagram.io)
* [DrawSQL](https://drawsql.app/)
* [Prisma Docs](https://www.prisma.io/docs)
* [TypeORM](https://typeorm.io)
* [Sequelize](https://sequelize.org)

---

Diese All-in-One-Referenz eignet sich für Entwickler, Architekten und Administratoren im gesamten Lebenszyklus von relationalen Datenbanken – lokal, cloudbasiert oder im Microservice-Umfeld.
