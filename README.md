# 🏢 Şirket & İK Yönetim Sistemi Veritabanı

Oracle SQL ile geliştirilmiş kurumsal bir insan kaynakları veritabanı şeması. Çalışan, departman, proje ve sipariş yönetimini kapsar.

## 📋 Tablolar

| Tablo | Açıklama |
|---|---|
| `USERS` | Sisteme kayıtlı kullanıcılar |
| `DEPARTMENT` | Şirket departmanları ve yöneticileri |
| `EMPLOYEE` | Çalışan bilgileri |
| `DEPT_LOCATIONS` | Departman lokasyonları |
| `PROJECT` | Projeler ve atandıkları departmanlar |
| `DEPENDENT` | Çalışanların bakmakla yükümlü oldukları kişiler |
| `WORKS_ON` | Çalışan-Proje atamaları |
| `ORDERS` | Kullanıcı siparişleri |

## 🗺️ ER Diyagramı

```mermaid
erDiagram
    USERS {
        NUMBER ID PK
        VARCHAR2 NAME
        NUMBER AGE
    }
    DEPARTMENT {
        NUMBER DNUMBER PK
        VARCHAR2 DNAME
        CHAR MNGSSN FK
        DATE MNGSTARTDATE
    }
    EMPLOYEE {
        CHAR ESSN PK
        VARCHAR2 FNAME
        VARCHAR2 LNAME
        DATE BDATE
        VARCHAR2 ADDRESS
        CHAR SEX
        NUMBER SALARY
        NUMBER DEPTNO FK
    }
    DEPT_LOCATIONS {
        NUMBER DNUMBER FK
        VARCHAR2 DLOCATION
    }
    PROJECT {
        NUMBER PID PK
        VARCHAR2 PNAME
        VARCHAR2 PLOCATION
        NUMBER DEPTNO FK
    }
    DEPENDENT {
        CHAR ESSN FK
        VARCHAR2 NAME
        CHAR SEX
        DATE BDATE
        VARCHAR2 RELATIONSHIP
    }
    WORKS_ON {
        CHAR ESSN FK
        NUMBER PID FK
        NUMBER HOURS
    }
    ORDERS {
        NUMBER ID PK
        NUMBER USER_ID FK
        NUMBER AMOUNT
    }

    EMPLOYEE }o--|| DEPARTMENT : "works in"
    DEPARTMENT |o--o| EMPLOYEE : "managed by"
    DEPT_LOCATIONS }o--|| DEPARTMENT : "location"
    PROJECT }o--|| DEPARTMENT : "belongs to"
    DEPENDENT }o--|| EMPLOYEE : "dependent of"
    WORKS_ON }o--|| EMPLOYEE : "works"
    WORKS_ON }o--|| PROJECT : "on"
    ORDERS }o--|| USERS : "places"
```

## ⚙️ Kurulum

### Gereksinimler
- Oracle Database (19c veya üzeri önerilir)
- Oracle SQL Developer veya SQL*Plus

### Çalıştırma Sırası

> ⚠️ Önce `schema.sql`, sonra `data.sql` çalıştırılmalıdır.

```bash
# SQL*Plus ile
@schema.sql
@data.sql
```

SQL Developer kullanıyorsanız dosyaları sırasıyla açıp **F5** ile çalıştırın.

## 🗂️ Dosya Yapısı

```
├── schema.sql   # Tablo tanımları, FK'lar, View, Trigger, Procedure
└── data.sql     # Örnek test verileri
```

## 🛠️ Şema Nesneleri

### View — `V_EMPLOYEE_REPORT`
Çalışanların departman, maaş, bakmakla yükümlü sayısı ve toplam çalışma saati bilgilerini getiren rapor görünümü.

### Trigger — `TRG_CHECK_SALARY`
Bir çalışanın maaşının 2000 birimin altına düşmesini engeller.

### Procedure — `PR_GIVE_RAISE`
Belirtilen departmandaki tüm çalışanlara yüzde bazlı zam uygular.

```sql
-- Kullanım örneği: 1 numaralı departmana %10 zam
EXEC PR_GIVE_RAISE(1, 10);
```

## 👤 Yazar

**Yusuf Koyuncu** — 2026
