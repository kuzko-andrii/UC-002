# UС-002: Detect deletion of important files from the Document folder

## TOC

- [UС-002: Detect deletion of important files from the Document folder](#uс-002-detect-deletion-of-important-files-from-the-document-folder)
  - [TOC](#toc)
  - [General](#general)
    - [Definition](#definition)
    - [Description](#description)
    - [Preparation Tasks](#preparation-tasks)
    - [Implementation Details](#implementation-details)
    - [Status](#status)
    - [Automation](#automation)
    - [Classification](#classification)
    - [Response and Personnel Involvement](#response-and-personnel-involvement)
  - [IBM QRadar SIEM](#ibm-qradar-siem)
    - [Building Blocks](#building-blocks)
      - [BB:UC-002.1: Detect access to the Documents folder](#bbuc-0021-detect-access-to-the-documents-folder)
      - [BB:UC-002.1: Detect deletion files](#bbuc-0021-detect-deletion-files)
  - [ITS Inventory/Userventory](#its-inventoryuserventory)
    - [Correlation Rules](#correlation-rules)
      - [UC-002: Detect deletion of important files from the Document folder](#uc-002-detect-deletion-of-important-files-from-the-document-folder)
    - [ITS Incident Management Playbook Runner](#its-incident-management-playbook-runner)

## General

### Definition

|             |                                                             |
| ----------- | ----------------------------------------------------------- |
| _Code_      | 002                                                         |
| _Name_      | Detect deletion of important files from the Document folder |
| _Need Test_ | Notest                                                      |
| _Notes_     | -                                                           |

---

### Description

Даний Use Case допомагає виявити небажане й нелегітимне видалення важливих для користувача файлів, котрі можуть знаходитись у папці "Documents".
Таке виявлення прикріплене до хоста з ip адресою 192.168.111.49, з джерелом даних "WindowsAuthServer @ NT-STUDENT3".
План реагування розписано в Playbook UC-002.

### Preparation Tasks

- [x] Налаштувати політику на хості (Замовник)
- [x] Створити Log Source для збірки відповідних логів (Виконавець)
- [x] Моніторинг (Виконавець)

### Implementation Details

|                             |                                                          |
| --------------------------- | -------------------------------------------------------- |
| _Security Systems_          | IBM QRadar SIEM                                          |
| _Implementation Conditions_ | Відповідно налаштовані групові політики та джерело даних |
| _Event ID_ (для UC)         | 4660, 4663                                               |
| _QID Name_ (для UC)         | 5000850, 5000847                                         |
| _Event Category_            | Audit                                                    |
| _Log Source Types_          | Microsoft Windows Security Event Log                     |
| _Requestor_                 | SOC Tier 1                                               |
| _Owner_                     | AKuzko                                                   |
| _Passport_                  | ?                                                        |

### Status

|                         |            |
| ----------------------- | ---------- |
| _UC Status_             | Monitoring |
| _Date Created_          | 28/06/2024 |
| _Date Start Monitoring_ | 28/06/2024 |
| _Date Deprecated_       | -          |

### Automation

|                      |     |
| -------------------- | --- |
| _Automated_          | No  |
| _Automation Step_    | -   |
| _Date of Automation_ | -   |

### Classification

|                          |                    |
| ------------------------ | ------------------ |
| **Security Domain**      | **Група контролю** |
| _Type_                   | UC                 |
| _ORG Priority_           | medium             |
| _Complexity_             | low                |
| _SLA_                    | 10 minutes         |
| _MITRE Tactics[^fn2]_    | -                  |
| _MITRE Techniques[^fn2]_ | -                  |

### Response and Personnel Involvement

|                         |                                                                                                                 |
| ----------------------- | --------------------------------------------------------------------------------------------------------------- |
| _SOC Tier 1_            | Yes                                                                                                             |
| _SOC Tier 2_            | Yes                                                                                                             |
| _SOC Engineers_         | No                                                                                                              |
| _SOC Manager Замовника_ | No                                                                                                              |
| _IS-відділ Замовника_   | Yes                                                                                                             |
| _IT-відділ Замовника_   | Yes                                                                                                             |
| _Playbook Activation_   | Наявність спрацювання кореляційного правила UС-002: Detect deletion of important files from the Document folder |
| _Playbook_              | ?                                                                                                               |

## IBM QRadar SIEM

- [ ] Reference Data
- [x] Building Blocks
- [x] Correlation Rules
- [x] Searches
- [ ] Dashboards
- [ ] Reports

### Building Blocks

#### BB:UC-002.1: Detect access to the Documents folder

**Description:** виявлення запиту на доступ до папок/файлів котрі підлягають захисту від видалення

**Test Definitions:**

```py
APPLY BB:UC-002.1: Detect access to the Documents folder on events which are detected by the LOCAL system

AND when the destination IP is one of the following 192.168.111.49

AND when the event QID is one of the following (5000850) Success Audit: An attempt was made to access an object
```

#### BB:UC-002.1: Detect deletion files

**Description:** виявлення видалення папок/файлів котрі підлягають захисту від таких дій

**Test Definitions:**

```py
APPLY BB:UC-002.2: Detect deletion files on events which are detected by the LOCAL system

AND when the destination IP is one of the following 192.168.111.49

AND when the event QID is one of the following (5000847) Success Audit: An object was deleted
```

## ITS Inventory/Userventory

- [ ] Fields
- [ ] Usecases
- [ ] Connectors

### Correlation Rules

#### UC-002: Detect deletion of important files from the Document folder

**Description:** Виявлення видаленя важливих файлів на хості 192.168.111.49 в папці Documents.

**Status:** Implemented

**Automatization:** -

**Set Rule Logic:**<br>

```py
AND when BB:UC-002.2: Detect deletion files match at least 1 times in 1 minutes after any of BB:UC-002.1: Detect access to the Documents folder match
```

### ITS Incident Management Playbook Runner

В разі наявності автоматизованого плейбуку по опрацюванню юзкейсу в даному розділі надається посилання на документ з експортом конфігурації у JSON-форматі.
