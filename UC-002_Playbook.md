# UС-002: Detect deletion of important files from the Document folder

## TOC

- [UС-002: Detect deletion of important files from the Document folder](#uс-002-detect-deletion-of-important-files-from-the-document-folder)
  - [TOC](#toc)
  - [Playbook Overview](#playbook-overview)
  - [Known issues](#known-issues)
    - [Source justifying the exception](#source-justifying-the-exception)
  - [Phase 1 - Offense Analysis](#phase-1---offense-analysis)
    - [Task 1 - Event Analysis](#task-1---event-analysis)
    - [Task 2 - Check Violation](#task-2---check-violation)
    - [Task 3 - Search remote communication](#task-3---search-remote-communication)
    - [Task 4 - Reputation check](#task-4---reputation-check)
    - [Task 5 - Create a message from SOC Analyst to SOC Analyst](#task-5---create-a-message-from-soc-analyst-to-soc-analyst)
  - [Phase 2 - Eliminating the consequences of the incident](#phase-2---eliminating-the-consequences-of-the-incident)
    - [Task 1 - Create an appeal from SOC Analyst to customer's IS](#task-1---create-an-appeal-from-soc-analyst-to-customers-is)
    - [Task 2 - Close offense](#task-2---close-offense)

## Playbook Overview

Даний плейбук містить план реагування на інцидент з ідентифікатором UC-002.

## Known issues

### Source justifying the exception

Якщо дія виконана користувачем з Username "ddraguncov", то такі дії вважаються легітимними й не підлягають під подальший аналіз.

## Phase 1 - Offense Analysis

### Task 1 - Event Analysis

- **Task Name** : Event Analysis
- **Performer** : SOC L1
- **Required** : Mandatory
- **Due Date** : No
- **Instructions** :

1. Відкрити Offense, змінити його статус на Protected
2. Призначити Offense на свій обліковий запис.
3. Перейти у список подій і визначити значення полів:

- [ ] Log Source
- [ ] Source IP
- [ ] Destination IP
- [ ] Username
- [ ] Count of events

* **Incident Fields** : Значення полів з п.3
* **Comments** : -

### Task 2 - Check Violation

- **Task Name** : Check Violation
- **Performer** : SOC L1
- **Required** : Mandatory
- **Due Date** : No
- **Instructions** :

1. Перевірити, що Username не є винятком, а саме не є "ddraguncov"
2.  **Сценарій 1 - Username є винятком для даного інциденту:**:

- [ ] Note:

```
Дія від користувача "ddraguncov" погоджена в рамках проекту #328-17
```

- [ ] Перейти до закриття Offense з Close Reason = Non-issue

**Сценарій 2 - Дійсне спрацювання:**:

- [ ] Перейти до наступного завдання

* **Incident Fields** : Обрано сценарій опрацювання
* **Comments** : --

### Task 3 - Search remote communication

- **Task Name** : Search by Destionation IP
- **Performer** : SOC L1
- **Required** : Mandatery
- **Due Date** : No
- **Instructions** :

1. Перейти у Log Activity.
2. Додати фільтр Destination IP contains any of <Вказати Destination IP>
3. Виконати пошук за останні 3 години з групуванням по назві події.
4. Визначити, чи є події котрі свідячать про попереднє віддалене підключення до хоста.
5. **Сценарій 1 - Не виявлено підозрілі підключення:**:

- [ ]  Перейти до наступного завдання

**Сценарій 2 - Виявлено підозрілі підключення:**:

- [ ] Note:

```
Історичний пошук подій за даною IP адресою показав:
<Вписати час події-1> - <Назва події-1> - <Source IP-1> - <Destination-1> - <Log Source-1>
<Вписати час події-2> - <Назва події-2> - <Source IP-2> - <Destination-2> - <Log Source-2>...
```

- [ ]  Перейти до наступного завдання

* **Incident Fields** : Обрано сценарій опрацювання
* **Comments** : --

### Task 4 - Reputation check

- **Task Name** : Reputation check
- **Performer** : SOC L1
- **Required** : Optional (умова пропуску: сценарій 1 в попередньому завданні)
- **Due Date** : No
- **Instructions** :

1. Проаналізувати репутацію Source IP з події про віддалене підключення на ресурсі IBM X-Force (https://exchange.xforce.ibmcloud.com/)
2. Проаналізувати репутацію Source IP з події про віддалене підключення з допомогою ресурсу Virus Total (https://www.virustotal.com/).
3. Виконати перевірку репутації з допомогою Cisco Talos (https://talosintelligence.com/reputation_center/).
4. Додати нотатку, якщо репутація негативна:

```
Репутація ІР <ip_address> є негативною згідно бази <Вписати назву бази>.
<Посилання на звіт>
```

- **Incident Fields** : Визначена репутація Source IP.
- **Comments** : -

### Task 5 - Create a message from SOC Analyst to SOC Analyst

- **Task Name** : Create a message from SOC Analyst to SOC Analyst
- **Performer** : SOC L1\SOC Analyst
- **Required** : Optional (умова пропуску: хибне спрацювання)
- **Due Date** : No
- **Instructions** :

1. Сформувати звернення з параметрами згідно поточного workflow на Замовника. Звернення повинно містити контекст виявленого потенційного інциденту та чітке ТЗ на Замовника (уточнення легітимності по виявленій активності, блокування ОЗ, ізоляція хоста, тощо).
2. Змінити статус Offense на Follow Up, додати нотатку з номером звернення.

- **Incident Fields** : Note
- **Comments** : --

## Phase 2 - Eliminating the consequences of the incident

### Task 1 - Create an appeal from SOC Analyst to customer's IS

- **Task Name** : Create an appeal from SOC Analyst to customer's IS
- **Performer** : SOC Analyst\Customer's IS
- **Required** : Mandatory
- **Due Date** : No
- **Instructions** :

1. Сформувати звернення до IS замовника з інформацією про інцидент та проханням реагування, наприклад блокування хоста, розірвання віддаленого підключення до хоста, відновлення видалених даних, тощо.
2. Змінити статус Offense на Follow Up, додати нотатку з номером звернення.

- **Incident Fields** : Note
- **Comments** : --

### Task 2 - Close offense

- **Task Name** : Close offense
- **Performer** : SOC-L1\SOC Analyst
- **Required** : Mandatory
- **Due Date** : No
- **Instructions** :

1. Переконатись, що статус всіх звернень, звязаних з інцидентом - Виконано.
2. Закрити Offense відповідно до Close Reason у сценарії\закрити як Resolved.

- **Incident Fields** : Закритий Offense.
- **Comments** : --
