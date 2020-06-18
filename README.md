# TP

## 2.1 Sélections simple

### 1)

Sélection du contenu de la table REGIONS

```sql
SELECT * FROM REGIONS;
```

### 2)

Sélections seulement region_name de toutes les régions dans la table REGIONS

```sql
SELECT REGION_NAME FROM REGIONS;
```

### 3)

Sélection pour tous les employés: EMPLOYEE_ID, FIRST_NAME, LAST_NAME, EMAIL, PHONE_NUMBER dans la table EMPLOYEES

```sql
SELECT EMPLOYEE_ID, FIRST_NAME, LAST_NAME, EMAIL, PHONE_NUMBER FROM EMPLOYEES;
```

### 4)

Sélection pour tous les employés: FIRST_NAME, LAST_NAME et COMMISSION ( SALAIRE x %COMMISSION)

```sql
SELECT FIRST_NAME, LAST_NAME, (SALARY * COMMISSION_PCT) AS COMMISSION FROM EMPLOYEES;
```

## 2.2 Sélection avec fonction

### 1)

Sélection pour tous les employés : FIRST_NAME, LAST_NAME et COMMISSION (SALAIRE x %COMMISSION_PCT) avec une valeur non nulle

```sql
SELECT FIRST_NAME, LAST_NAME, (SALARY * NVL(COMMISSION_PCT, 0)) AS COMMISSION FROM EMPLOYEES;
```

### 2)

Sélection pour tous les employés : FIRST_NAME, LAST_NAME et COMMISSION (SALAIRE x %COMMISSION_PCT) arrondie à la centaine

```sql
SELECT FIRST_NAME, LAST_NAME, ROUND((SALARY * NVL(COMMISSION_PCT, 0)), -2) AS COMMISSION FROM EMPLOYEES;
```

### 3)

Sélection pour tous les employés : FIRST_NAME, LAST_NAME et date d'embauche(HIRE_DATE)

```sql
SELECT FIRST_NAME, LAST_NAME, HIRE_DATE FROM EMPLOYEES;
```

### 4)

Sélection pour tous les employés : FIRST_NAME, LAST_NAME et année d'embauche(HIRE_DATE)

```sql
SELECT FIRST_NAME, LAST_NAME, EXTRACT(YEAR FROM HIRE_DATE) FROM EMPLOYEES;
```

## 2.3

### 1)

Sélection de : FIRST_NAME, LAST_NAME et SALARY pour les employés ayant un salaire > 12000.

```sql
SELECT FIRST_NAME, LAST_NAME, SALARY FROM EMPLOYEES WHERE SALARY > 12000;
```

### 2)

Sélection de : FIRST_NAME, LAST_NAME et SALARY pour les employés ayant un salaire
compris entre 5000 et 6000.

```sql
SELECT FIRST_NAME, LAST_NAME, SALARY FROM EMPLOYEES WHERE SALARY > 5000 AND SALARY < 6000;
```

### 3)

Sélection de : FIRST_NAME, LAST_NAME et année d'embauche (HIRE_DATE) pour les employés embauchés avant 2003.

```sql
SELECT FIRST_NAME, LAST_NAME, EXTRACT(YEAR FROM HIRE_DATE) FROM EMPLOYEES WHERE EXTRACT(YEAR FROM HIRE_DATE) < 2003;
```

### 4)

Sélection de : FIRST_NAME, LAST_NAME et COMMISSION_PCT pour les employés ayant une commission.

```sql
SELECT FIRST_NAME, LAST_NAME, COMMISSION_PCT FROM EMPLOYEES WHERE COMMISSION_PCT IS NOT NULL;
```

### 5)

Sélection de : first_name, last_name et commission_pct au format « 0,xx » pour les employés ayant une commission.

```sql
SELECT FIRST_NAME, LAST_NAME, TO_CHAR(COMMISSION_PCT, '0D00') FROM EMPLOYEES WHERE COMMISSION_PCT IS NOT NULL;
```

> Remarque : avec '0,99', toutes les valeurs étaient amenées à 0,00. Une solution ?

### 6)

Sélection de : first_name, last_name et department_id pour les employés des département 70 et 90.

```sql
SELECT FIRST_NAME, LAST_NAME, DEPARTMENT_ID FROM EMPLOYEES WHERE DEPARTMENT_ID IN (70,90);
```

### 7)

Sélection de : first_name, last_name, job_id et department_id pour les employés ayant une fonction d'administration c'est-à-dire que le job_id commence par « AD ».

```sql
SELECT FIRST_NAME, LAST_NAME, JOB_ID, DEPARTMENT_ID FROM EMPLOYEES WHERE JOB_ID LIKE('AD%');
```

### 8)

Sélection de : first_name, last_name, nombre de mois d'ancienneté pour tous les employés.

```sql
SELECT FIRST_NAME, LAST_NAME, ROUND(MONTHS_BETWEEN(SYSDATE, HIRE_DATE)) AS "ANCIENNETE (Mois)" FROM EMPLOYEES;
```

### 9)

Sélection de : first_name, last_name, job_id et nombre de jours d'ancienneté pour les employés ayant moins d'une année d'ancienneté au 10 août 2008.

```sql
SELECT FIRST_NAME, LAST_NAME, ROUND((TO_DATE('10/08/2008') - HIRE_DATE)) AS "ANCIENNETE (Jours)"
FROM EMPLOYEES
WHERE TO_DATE('10/08/2008') - HIRE_DATE < 366;
```

### 10)

Sélection des noms de départements n'ayant pas de manager

```sql
SELECT DEPARTMENT_NAME FROM DEPARTMENTS WHERE MANAGER_ID IS NULL;
```

### 11)

Sélection des adresses, des états et des villes des locations du pays US

```sql
SELECT STREET_ADDRESS, STATE_PROVINCE, CITY FROM LOCATIONS
WHERE COUNTRY_ID = 'US';
```

### 12)

Sélection numéro de rue (s'il existe sinon '....', nom de la rue, ville de

> WTF ???

### 13)

Sélection dans la table historique de jobs numéro de l'employé, le mois/année de début et de fin et l'ID du job.

```sql
SELECT EMPLOYEE_ID, TO_CHAR(START_DATE, 'MM/YYYY') AS START_DATE, TO_CHAR(END_DATE, 'MM/YYYY') AS END_DATE, JOB_ID FROM JOB_HISTORY;
```

### 14)

Sélection, dans la table des employés, le prénom et le nom et la reconstruire l'email à partir de nom et du prénom de chaque employé

```sql
SELECT FIRST_NAME, LAST_NAME, UPPER(SUBSTR(FIRST_NAME, 0, 1) || LAST_NAME) as EMAIL FROM EMPLOYEES;
```

### 15)

Sélection dans la table historique de jobs numéro de l'employé, le mois/année de début et de fin et l'ID du job.

> Voir exercice 13

### 16)

Sélection des ID, nom, et prénom des employés dont le salaire annuel + commission > 82000

```sql
SELECT EMPLOYEE_ID, LAST_NAME, FIRST_NAME FROM EMPLOYEES WHERE (COMMISSION_PCT*SALARY+SALARY)*12 > 82000;
```

### 17)

Sélection du ° de téléphone, nom, prénom des employés qui n'ont pas un ° de téléphone du type 123.123.1234,

```sql
SELECT PHONE_NUMBER, LAST_NAME, FIRST_NAME FROM EMPLOYEES WHERE PHONE_NUMBER NOT LIKE ('___.___.____');
--version REGEX :
SELECT PHONE_NUMBER, LAST_NAME, FIRST_NAME FROM EMPLOYEES WHERE NOT REGEXP_LIKE(PHONE_NUMBER, '[0-9]{3}.[0-9]{3}.[0-9]{4}');
```

### 18)

Sélection pour chaque employé ayant une commission >= a 15%, le prénom et le nom et leur trigramme (1 lettre du prénom + 1 lettre du nom + dernière lettre de nom) le tout en majuscule

```sql
SELECT FIRST_NAME, LAST_NAME, UPPER(SUBSTR(FIRST_NAME, 0, 1) || SUBSTR(LAST_NAME, 0, 1) || SUBSTR(LAST_NAME, -1, 1)) AS TRIGRAMME FROM EMPLOYEES
=======
```

### 19)

Sélection pour de tous les employés ayant 2 e de suite « ee » dans leur nom ou leur prénom.

```sql
SELECT * FROM EMPLOYEES WHERE FIRST_NAME LIKE ('%ee%') OR LAST_NAME LIKE('%ee%');
```

### 20)

Sélection du nom et prénom des employés avec pour les noms la première et dernière lettre en clair et les autres remplacées par « \* ».

```sql
SELECT
  SUBSTR(LAST_NAME, 0, 1) || REPLACE(LAST_NAME, SUBSTR(LAST_NAME, 1, LENGTH(LAST_NAME)), '*') || SUBSTR(LAST_NAME, -1, 1) AS LAST_NAME,
  SUBSTR(FIRST_NAME , 0, 1) || REPLACE(FIRST_NAME, SUBSTR(FIRST_NAME, 1, LENGTH(FIRST_NAME)), '*') || SUBSTR(FIRST_NAME , -1, 1) AS FIRST_NAME
FROM EMPLOYEES;
```

### 21)

Sélection pour du nom et prénom des employés qui ont le même nombre de lettres dans leur nom que dans leur prénom.

```sql
SELECT LAST_NAME, FIRST_NAME FROM EMPLOYEES WHERE LENGTH(LAST_NAME) = LENGTH(FIRST_NAME);
```

### 22)

> Comme 21.

### 23)

Sélection pour du nom et prénom des employés qui ont moins 2 l (lettre L) dans leur nom et prénom

```sql
SELECT * FROM EMPLOYEES WHERE (LENGTH(FIRST_NAME) - LENGTH(REPLACE(FIRST_NAME, 'l', ''))) + (LENGTH(LAST_NAME) - LENGTH(REPLACE(LAST_NAME, 'l', ''))) < 2;
```

### 24)

Sélection des locations dont le code postal n'a que des chiffres

```sql
SELECT * FROM LOCATIONS WHERE REGEXP_LIKE(POSTAL_CODE, '^[0-9]+$');
```

### 25)

Sélection des locations dont leur nom de ville est composé de plusieurs mots

```sql
SELECT * FROM LOCATIONS
WHERE CITY LIKE '% %' ;
```

 <hr/>

# TP3 (Jointures)

## 3.1 Produit cartésien

### 1)

Sélection de : toutes les colonnes du produit catésien de COUNTRIES et de REGIONS.

```sql
SELECT * FROM COUNTRIES, REGIONS;
```

## 3.2 Jointures simple (équijointures)

### 1)

Sélection de : nom du pays et du nom de sa régions pour tous les pays de COUNTRIES.

```sql
SELECT c.COUNTRY_NAME, r.REGION_NAME
FROM COUNTRIES c, REGIONS r
WHERE c.REGION_ID = r.REGION_ID;
```

### 2)

Sélection de : nom de la ville, nom du pays et du nom de sa régions pour toutes les villes de LOCATIONS.

```sql
SELECT l.CITY, c.COUNTRY_NAME, r.REGION_NAME
FROM LOCATIONS l
INNER JOIN COUNTRIES c
  ON l.COUNTRY_ID = c.COUNTRY_ID
INNER JOIN REGIONS r
  ON c.REGION_ID = r.REGION_ID
;
```

### 3)

Sélection de : nom du département et du nom de sa régions pour tous les départements de DEPARTMENTS.

```sql
SELECT d.DEPARTMENT_NAME, r.REGION_NAME
FROM DEPARTMENTS d
INNER JOIN LOCATIONS l
  ON d.LOCATION_ID = l.LOCATION_ID
INNER JOIN COUNTRIES c
  ON c.COUNTRY_ID = l.COUNTRY_ID
INNER JOIN REGIONS r
  ON r.REGION_ID = c.REGION_ID
;
```

### 4)

Sélection de : nom du département et du nom de sa ville pour tous les départements de DEPARTMENTS.

```sql
SELECT d.DEPARTMENT_NAME, l.CITY
FROM DEPARTMENTS d
INNER JOIN LOCATIONS l
  ON d.LOCATION_ID = l.LOCATION_ID
;
```

### 3.3 Jointures externes

### 1)

Sélection de : nom département, nom de sa ville et nom de son manager, s'il existe, pour tous les départements de DEPARTMENTS

```sql
SELECT d.DEPARTMENT_NAME, l.CITY, e.LAST_NAME
FROM DEPARTMENTS d
INNER JOIN LOCATIONS l
  ON d.LOCATION_ID = l.LOCATION_ID
LEFT OUTER JOIN EMPLOYEES e
  ON d.MANAGER_ID = e.EMPLOYEE_ID
;
```

### 2)

Sélection de : nom région, titre de job s'il existe, pour tous les régions de REGIONS. Si une région n'a pas de job retourner la région , ‘pas de job'

```sql
SELECT DISTINCT(REGION_NAME), NVL(JOB_TITLE, 'Pas de Job !')
FROM JOBS
  LEFT JOIN EMPLOYEES ON JOBS.JOB_ID = EMPLOYEES.JOB_ID
  INNER JOIN DEPARTMENTS ON EMPLOYEES.DEPARTMENT_ID = DEPARTMENTS.DEPARTMENT_ID
  INNER JOIN LOCATIONS ON DEPARTMENTS.LOCATION_ID = LOCATIONS.LOCATION_ID
  INNER JOIN COUNTRIES ON LOCATIONS.COUNTRY_ID = COUNTRIES.COUNTRY_ID
  FULL OUTER JOIN REGIONS ON COUNTRIES.REGION_ID = REGIONS.REGION_ID
;
```

## 3.4) Auto-jointures

### 1)

Sélection de : prénom, nom et nom du manager pour tous les employés de EMPLOYEES ayant un salaire < 2500

```sql
SELECT E.FIRST_NAME, E.LAST_NAME, M.FIRST_NAME
FROM EMPLOYEES E
INNER JOIN EMPLOYEES M
  ON E.MANAGER_ID = M.EMPLOYEE_ID
WHERE E.SALARY < 2500;
```

# TP4 (Regroupements)

## 4.1 Requêtes de regroupement

### 1)

Sélection du salaire moyen de l'ensemble des employés de EMPLOYEES

```sql
SELECT ROUND(AVG(SALARY),2) as "Salaire moyen" FROM EMPLOYEES;
```

### 2)

Sélection de : nom de département, salaire moyen des employés pour tous les départements de DEPARTMENTS

```sql
SELECT d.DEPARTMENT_NAME as "Département", NVL(ROUND(AVG(SALARY)), 0) as "Salaire moyen", COUNT(e.EMPLOYEE_ID) as "Nombre d''employés"
FROM DEPARTMENTS d
LEFT JOIN EMPLOYEES e
  ON e.DEPARTMENT_ID = d.DEPARTMENT_ID
GROUP BY d.DEPARTMENT_NAME
ORDER BY d.DEPARTMENT_NAME;
```

### 3)

Sélection de : nom de département, nom de fonction, nombre d'employés pour la fonction pour les départements de DEPARTMENTS et les fonctions de JOBS ayant des employés

```sql
SELECT  d.DEPARTMENT_NAME as "Département", j.JOB_TITLE as "Métier", COUNT(e.EMPLOYEE_ID) as "Nombre d'employés"
FROM JOBS j
INNER JOIN EMPLOYEES e
  ON j.JOB_ID = e.JOB_ID
INNER JOIN DEPARTMENTS d
  ON e.DEPARTMENT_ID = d.DEPARTMENT_ID
GROUP BY j.JOB_TITLE, d.DEPARTMENT_NAME
ORDER BY d.DEPARTMENT_NAME
;
```

### 4)

Sélection de : nom de la ville, le salaire minimum, le salaire maximum, nombre d'employés pour toutes les villes de LOCATIONS ayant des employés

```sql
SELECT l.CITY as "Ville", MIN(e.SALARY) as "Salaire minimum", MAX(e.SALARY) as "Salaire maximum"
FROM LOCATIONS l
INNER JOIN DEPARTMENTS d
  ON d.LOCATION_ID = l.LOCATION_ID
INNER JOIN EMPLOYEES e
  ON d.DEPARTMENT_ID = e.DEPARTMENT_ID
GROUP BY l.CITY
;
```

## 4.2 Filtrage de regroupements

### 1)

Sélection de : nom de la ville, le salaire minimum, le salaire maximum, le salaire moyen arrondi à l'unité pour toutes les villes de LOCATIONS ayant au moins 5 employés.

```sql
SELECT l.CITY as "Ville", MIN(e.SALARY) as "Salaire minimum", MAX(e.SALARY) as "Salaire maximum", ROUND(AVG(e.SALARY)) as "Salaire moyen"
FROM LOCATIONS l
INNER JOIN DEPARTMENTS d
  ON l.LOCATION_ID = d.LOCATION_ID
INNER JOIN EMPLOYEES e
  ON d.DEPARTMENT_ID = e.DEPARTMENT_ID
GROUP BY l.CITY
HAVING COUNT(e.EMPLOYEE_ID) >= 5
;
```

### 2)

Sélection de : nom du manager, nom de la fonction des employés, commission moyenne arrondie à l'unité pour tous les manager des employés de EMPLOYEES ayant une commission et chaque fonction de JOBS de ces employés.

```sql
SELECT
  m.LAST_NAME as "Manager",
  j.JOB_TITLE as "Métier",
  ROUND(AVG(e.COMMISSION_PCT*e.SALARY)) as "Commission moyenne"
FROM EMPLOYEES e
INNER JOIN EMPLOYEES m
  ON e.MANAGER_ID = m.EMPLOYEE_ID
INNER JOIN JOBS j
  ON e.JOB_ID = j.JOB_ID
WHERE e.COMMISSION_PCT IS NOT NULL
GROUP BY m.LAST_NAME, j.JOB_TITLE
;
```

### 3)

Sélection de : prénom, nom des employés de EMPLOYEES ayant changés de fonction (indiqués dans JOB_HISTORY)

```sql
SELECT DISTINCT e.FIRST_NAME, e.LAST_NAME
FROM EMPLOYEES e
INNER JOIN JOB_HISTORY j
  ON e.EMPLOYEE_ID = j.EMPLOYEE_ID
;
```

### 4)

Sélection de : ville, nom du manager, % de commission moyenne des employés de EMPLOYEES par villes et managers dont les subordonnées ont une commission.

```sql
SELECT l.CITY, m.LAST_NAME, (ROUND(AVG(e.COMMISSION_PCT), 2) * 100) || '%' as "Commission moyenne"
FROM EMPLOYEES e
INNER JOIN EMPLOYEES m
  ON e.MANAGER_ID = m.EMPLOYEE_ID
INNER JOIN DEPARTMENTS d
  ON e.DEPARTMENT_ID = d.DEPARTMENT_ID
INNER JOIN LOCATIONS l
  ON d.LOCATION_ID = l.LOCATION_ID
WHERE e.COMMISSION_PCT IS NOT NULL
GROUP BY m.LAST_NAME, l.CITY
;
```

### 5)

Sélection de : Pays, Nom du manager, Nombre d'employés des managers de EMPLOYEES par managers et pays hors « US ».

```sql
SELECT c.COUNTRY_ID, m.LAST_NAME, COUNT(e.EMPLOYEE_ID) as "Nombre d'employés"
FROM LOCATIONS l
INNER JOIN DEPARTMENTS d
  ON l.LOCATION_ID = d.LOCATION_ID
INNER JOIN COUNTRIES c
  ON l.COUNTRY_ID = c.COUNTRY_ID
INNER JOIN EMPLOYEES m
  ON m.DEPARTMENT_ID = d.DEPARTMENT_ID
INNER JOIN EMPLOYEES e
  ON e.MANAGER_ID = m.EMPLOYEE_ID
GROUP BY m.LAST_NAME, c.COUNTRY_ID
HAVING c.COUNTRY_ID != 'US'
;
```

# TP5 (Sous-requêtes)

## 5.1 Sous requête dans la clause WHERE

### 1)

Sélection first_name, last_name des employés de EMPLOYEES ayant le plus petit salaire

```sql
SELECT FIRST_NAME, LAST_NAME
FROM EMPLOYEES
WHERE SALARY = (SELECT MIN(SALARY) FROM EMPLOYEES);
```

### 2)

Sélection first_name, last_name des employés de EMPLOYEES qui sont managers

```sql
SELECT FIRST_NAME, LAST_NAME
FROM EMPLOYEES
WHERE EMPLOYEE_ID IN (SELECT DISTINCT(MANAGER_ID) FROM EMPLOYEES);
```

### 3)

Sélection de : first_name, last_name des employés de EMPLOYEES ayant le même last_name.

```sql
SELECT FIRST_NAME, LAST_NAME
FROM EMPLOYEES
WHERE LAST_NAME IN (SELECT LAST_NAME FROM EMPLOYEES GROUP BY LAST_NAME HAVING COUNT(*) > 1);
```

### 4)

Sélection de : prénom, nom, fonction, précédente fonction des employés de EMPLOYEES ayant changés de fonction (indiqués dans JOB_HISTORY)

```sql
SELECT e.FIRST_NAME, e.LAST_NAME, e.JOB_ID, h.JOB_ID
FROM EMPLOYEES e
INNER JOIN JOB_HISTORY h
  ON e.EMPLOYEE_ID = h.EMPLOYEE_ID
WHERE END_DATE =
(
  SELECT MAX(END_DATE)
  FROM JOB_HISTORY
  WHERE EMPLOYEE_ID = h.EMPLOYEE_ID
)
;
```

## 5.2 Sous requêtes dans la clause FROM

### 1)

Sélection first_name, last_name, salaire des employés de EMPLOYEES ayant le plus petit salaire ou le plus gros salaire

```sql
SELECT FIRST_NAME, LAST_NAME, SALARY
FROM EMPLOYEES
JOIN
(
  SELECT
    MIN(SALARY) salaire_min,
    MAX(SALARY) salaire_max
  FROM EMPLOYEES
) salaires
  ON SALARY = salaires.salaire_min
  OR SALARY = salaires.salaire_max
;
```

### 2)

Sélection de : prénom, nom, fonction, précédente fonction des employés de EMPLOYEES ayant changés de fonction (indiqués dans JOB_HISTORY)

```sql
SELECT FIRST_NAME, LAST_NAME, job_actuel.JOB_TITLE, job_precedent.JOB_TITLE
FROM JOB_HISTORY h
JOIN EMPLOYEES e ON e.EMPLOYEE_ID = h.EMPLOYEE_ID
JOIN JOBS job_actuel ON e.JOB_ID = job_actuel.JOB_ID
JOIN JOBS job_precedent ON e.JOB_ID = job_precedent.JOB_ID
JOIN
(
  SELECT EMPLOYEE_ID, MAX(end_date) max_date
  FROM JOB_HISTORY
  GROUP BY EMPLOYEE_ID
) precedent
ON precedent.EMPLOYEE_ID = e.EMPLOYEE_ID AND precedent.max_date = h.END_DATE
;
```

### 3)

Sélection first_name, last_name des employés de EMPLOYEES qui sont managers

```sql
SELECT e.FIRST_NAME, e.LAST_NAME
FROM EMPLOYEES e
JOIN
(
  SELECT MANAGER_ID, COUNT(EMPLOYEE_ID)
  FROM EMPLOYEES
  GROUP BY MANAGER_ID
) manager
ON e.EMPLOYEE_ID = manager.MANAGER_ID
;
```

# TP6 (Tris)

## 6.1

### 1)

Sélection pour tous les employés : first_name, last_name et salaire + commission ordonnés par nom

```sql
SELECT FIRST_NAME, LAST_NAME, (SALARY+NVL((SALARY*COMMISSION_PCT),0)) as "Salaire total"
FROM EMPLOYEES
ORDER BY LAST_NAME;
```

### 2)

Sélection pour tous les employés : first_name, last_name, nom de département ordonnés par nom de département, nom et prénom

```sql
SELECT DEPARTMENT_NAME, FIRST_NAME, LAST_NAME
FROM EMPLOYEES e
JOIN DEPARTMENTS d
  ON e.DEPARTMENT_ID = d.DEPARTMENT_ID
ORDER BY DEPARTMENT_NAME, LAST_NAME, FIRST_NAME;
```

### 3)

Sélection first_name, last_name des employés de EMPLOYEES qui sont managers ordonnés par salaire + commission décroissant

```sql
SELECT FIRST_NAME, LAST_NAME
FROM EMPLOYEES
WHERE EMPLOYEE_ID IN (SELECT DISTINCT(MANAGER_ID) FROM EMPLOYEES)
ORDER BY (SALARY+(SALARY*NVL(COMMISSION_PCT,0))) DESC;
```

## 6.2 Opérateurs ensemblistes

### 1)

Sélection de : first_name, last_name, salaire des employés de EMPLOYEES ayant le plus petit salaire ou le plus gros salaire

> Pareil que 5.2.1

### 2)

Sélection pour les fonctions des employés ayant changés de fonctions de : nom, fonction, date début, date de fin le tout ordonné par nom, date début

```sql
SELECT e.LAST_NAME, j.JOB_TITLE, h.START_DATE, h.END_DATE
FROM EMPLOYEES e
INNER JOIN JOB_HISTORY h
  ON e.EMPLOYEE_ID = h.EMPLOYEE_ID
LEFT JOIN JOBS j
  ON h.JOB_ID = j.JOB_ID
ORDER BY e.LAST_NAME, h.START_DATE;
```

## 6.3

### 1)

Sélection de : level, nom, prénom de tous les employés de EMPLOYEES :

```sql
SELECT LEVEL, LAST_NAME, FIRST_NAME
from employees
-- On détermine le côté parent de la relation (le parent est employee_id)
CONNECT BY PRIOR EMPLOYEE_ID = MANAGER_ID
START WITH MANAGER_ID IS NULL;
```

### 2)

Sélection de : nom prénom de tous les employés de EMPLOYEES sous forme d'une arborscence de type :
Branche niveau 1
... Branche niveau 1
....... Feuille niveau 3
....... Branche niveau 3

```
SET PAGESIZE 2000
select
  case level
    when 1 then ''
    when 2 then '... '
    when 3 then '...... '
    when 4 then '......... '
    end
  ||
  last_name ||
  first_name as "Employé"
from employees
connect by prior employee_id = manager_id
start with manager_id is null;
```
