# ICAO MRTD Personalization Complex — Architecture Diagrams

**Repository:** `ekatmoz88-alt/icao-mrtd-documentation`
**Source:** Sanitized project source archive
**Status:** Draft — based exclusively on extracted source documents
**Languages:** English / Russian
**Anonymization:** All customer names, personal data, serial numbers, keys, and internal addresses replaced with placeholders.

---

## Содержание / Contents

### RU
1. [Контекстная диаграмма (C4 Context)](#1-контекстная-диаграмма-c4-context)
2. [Контейнерная диаграмма (C4 Container)](#2-контейнерная-диаграмма-c4-container)
3. [Диаграмма последовательности управления лазером YLPM](#3-диаграмма-последовательности-управления-лазером-ylpm)
4. [Диаграмма алгоритма контроля качества MRZ](#4-диаграмма-алгоритма-контроля-качества-mrz)
5. [Статистика распознавания MRZ](#5-статистика-распознавания-mrz)

### EN
1. [C4 Context Diagram](#1-c4-context-diagram)
2. [C4 Container Diagram](#2-c4-container-diagram)
3. [YLPM Laser Control Sequence Diagram](#3-ylpm-laser-control-sequence-diagram)
4. [MRZ Quality Control Algorithm Diagram](#4-mrz-quality-control-algorithm-diagram)
5. [MRZ Recognition Statistics](#5-mrz-recognition-statistics)

---

## 1. Контекстная диаграмма (C4 Context) / C4 Context Diagram

### RU
Показывает границу системы и внешних акторов: Оператор, смарт-карта, внешняя среда.

```mermaid
graph TD
    subgraph External["Внешняя среда / External Environment"]
        Operator[Оператор / Operator]
        SmartCard[Смарт-карта / Smart Card]
    end

    subgraph System["Комплекс персонализации / Personalization Complex"]
        UI[Интерфейс пользователя / UI]
    end

    Operator -->|Задание на персонализацию / Personalization job| UI
    UI -->|Электронная персонализация / Electronic personalization| SmartCard
```

### EN
Shows the system boundary and external actors: Operator, Smart Card, external environment.

```mermaid
graph TD
    subgraph External["External Environment"]
        Operator[Operator]
        SmartCard[Smart Card]
    end

    subgraph System["Personalization Complex"]
        UI[User Interface / UI]
    end

    Operator -->|Personalization job| UI
    UI -->|Electronic personalization| SmartCard
```

---

## 2. Контейнерная диаграмма (C4 Container) / C4 Container Diagram

### RU
Внутренняя структура системы: компоненты, их взаимодействие и интеграция с внешним оборудованием.

```mermaid
graph TD
    Operator[Оператор / Operator]
    UI[Интерфейс пользователя / UI]
    JQM[Менеджер очереди заданий / Job Queue Manager]
    SOAP[SOAP API / SOAP API]
    GPM[Модуль графической персонализации / Graphic Personalization]
    EPM[Модуль электронной персонализации / Electronic Personalization]
    PecLib[Библиотека PecLib / PecLib Library]
    Laser[Лазер YLPM / Laser YLPM]
    RS232[RS-232C / RS-232C]
    Transport[Транспорт CIM 2000 / Transport CIM 2000]
    Printers[Принтеры Evolis / HDP600 / Card Printers Evolis/HDP600]
    MRZ[ABBYY Passport Reader SDK / ABBYY Passport Reader SDK]
    SmartCard[Смарт-карта / Smart Card]

    Operator --> UI
    UI --> JQM
    JQM --> GPM
    JQM --> EPM
    JQM --> SOAP
    EPM --> PecLib
    PecLib --> SmartCard
    GPM --> Laser
    Laser --> RS232
    JQM --> Transport
    JQM --> Printers
    JQM --> MRZ
```

### EN
Internal system structure: components, their interaction, and integration with external hardware.

```mermaid
graph TD
    Operator[Operator]
    UI[User Interface / UI]
    JQM[Job Queue Manager]
    SOAP[SOAP API]
    GPM[Graphic Personalization Module]
    EPM[Electronic Personalization Module]
    PecLib[PecLib Library]
    Laser[YLPM Laser]
    RS232[RS-232C]
    Transport[Transport CIM 2000]
    Printers[Card Printers Evolis/HDP600]
    MRZ[ABBYY Passport Reader SDK]
    SmartCard[Smart Card]

    Operator --> UI
    UI --> JQM
    JQM --> GPM
    JQM --> EPM
    JQM --> SOAP
    EPM --> PecLib
    PecLib --> SmartCard
    GPM --> Laser
    Laser --> RS232
    JQM --> Transport
    JQM --> Printers
    JQM --> MRZ
```

---

## 3. Диаграмма последовательности управления лазером YLPM / YLPM Laser Control Sequence Diagram

### RU
Порядок команд управления лазером YLPM через RS-232C и проверка битов расширенного статуса. Платы ввода/вывода PCI-7200 используются для импульсной перфорации.

```mermaid
sequenceDiagram
    participant UI as Интерфейс / UI
    participant API as SOAP API / SOAP API
    participant Laser as Лазер YLPM / YLPM Laser
    participant PCI7200 as Плата PCI-7200 / PCI-7200 Board

    UI->>API: Команда управления / Control command
    API->>Laser: Команда $11 — чтение расширенного статуса / Read extended status
    Laser-->>API: Бит 0 — аварийный останов / Bit 0 — emergency stop<br/>Бит 8 — лазерное излучение включено / Bit 8 — laser emission on<br/>Бит 11 — команда включения получена / Bit 11 — emission command received<br/>Бит 13 — питание 24 В допустимо / Bit 13 — 24 V supply valid
    API->>Laser: Команда $24 — установка режима работы / Set operating mode
    API->>Laser: Команда $26 — установка начального режима / Set initial operating mode
    API->>Laser: Команда $23 — чтение режима / Read operating mode
    API->>Laser: Команда $27 — чтение начального режима / Read initial operating mode
    API->>Laser: Команда $25 — информация об опциях / Read options information
    Laser-->>API: Статус прибора / Device status
    API->>PCI7200: Импульс перфорации / Perforation pulse
    PCI7200-->>API: Подтверждение импульса / Pulse confirmation
    API-->>UI: Результат выполнения / Execution result
```

### EN
Sequence of YLPM laser control commands via RS-232C and extended status bit verification. PCI-7200 I/O board is used for perforation pulse generation.

```mermaid
sequenceDiagram
    participant UI as User Interface / UI
    participant API as SOAP API
    participant Laser as YLPM Laser
    participant PCI7200 as PCI-7200 Board

    UI->>API: Control command
    API->>Laser: Command $11 — Read extended status
    Laser-->>API: Bit 0 — emergency stop<br/>Bit 8 — laser emission on<br/>Bit 11 — emission command received<br/>Bit 13 — 24 V supply valid
    API->>Laser: Command $24 — Set operating mode
    API->>Laser: Command $26 — Set initial operating mode
    API->>Laser: Command $23 — Read operating mode
    API->>Laser: Command $27 — Read initial operating mode
    API->>Laser: Command $25 — Read options information
    Laser-->>API: Device status
    API->>PCI7200: Perforation pulse
    PCI7200-->>API: Pulse confirmation
    API-->>UI: Execution result
```

---

## 4. Диаграмма алгоритма контроля качества MRZ / MRZ Quality Control Algorithm Diagram

### RU
Анализ распознавания шрифтов MRZ с помощью ABBYY Passport Reader SDK. Статистика: 242 документа, распознано 226 (93,39%).

```mermaid
flowchart TD
    Start([Начало / Start]) --> Scan[Сканирование документа / Scan Document]
    Scan --> ABBYY[ABBYY Passport Reader SDK]
    ABBYY --> Result[Результат распознавания / Recognition Result]
    Result --> OK{Распознано? / Recognized?}
    OK -->|Да / Yes| Pass[Пройдено / Passed]
    OK -->|Нет / No| Classify[Классификация дефектов / Defect Classification]
    
    Classify --> B[Б — Блёклый шрифт / Faint font]
    Classify --> K[К — Крупный шрифт / Large font]
    Classify --> R[Р — Шрифт разрывами / Broken font]
    Classify --> S[С — Смещение текста / Text offset]
    Classify --> BL[БЛ — Блики / Glare]
    Classify --> SSH[СШ — Не точная сшивка / Imprecise stitching]
    
    B --> Mark[Маркировка ошибки / Error Marking]
    K --> Mark
    R --> Mark
    S --> Mark
    BL --> Mark
    SSH --> Mark
    Mark --> End([Конец / End])
    Pass --> End
    
    style Start fill:#e1f5e1
    style End fill:#ffcccc
    style OK fill:#fff3cd
```

### EN
MRZ font recognition analysis using ABBYY Passport Reader SDK. Statistics: 242 documents, 226 recognized (93.39%).

```mermaid
flowchart TD
    Start([Start]) --> Scan[Scan Document]
    Scan --> ABBYY[ABBYY Passport Reader SDK]
    ABBYY --> Result[Recognition Result]
    Result --> OK{Recognized?}
    OK -->|Yes| Pass[Passed]
    OK -->|No| Classify[Defect Classification]
    
    Classify --> B[Б — Faint font]
    Classify --> K[К — Large font]
    Classify --> R[Р — Broken font]
    Classify --> S[С — Text offset]
    Classify --> BL[БЛ — Glare]
    Classify --> SSH[СШ — Imprecise stitching]
    
    B --> Mark[Error Marking]
    K --> Mark
    R --> Mark
    S --> Mark
    BL --> Mark
    SSH --> Mark
    Mark --> End([End])
    Pass --> End
    
    style Start fill:#e1f5e1
    style End fill:#ffcccc
    style OK fill:#fff3cd
```

---

## 5. Статистика распознавания MRZ / MRZ Recognition Statistics

### RU

| Параметр | Значение |
|---|---|
| Всего проанализировано документов | 242 |
| Распознано | 226 (93,39%) |
| Не распознано | 16 (6,61%) |

### EN

| Parameter | Value |
|---|---|
| Total documents analyzed | 242 |
| Recognized | 226 (93.39%) |
| Not recognized | 16 (6.61%) |

---

## Связь с документацией / Links to Documentation

- [SPECIFICATION.md](SPECIFICATION.md) — архитектурное и пользовательское описание / architectural and user specification
- [README.md](README.md) — главная страница проекта / project home page

---

*Документ подготовлен на основе извлечённых фактов без домыслов отсутствующих параметров. Все закрытые данные анонимизированы.*

*Document prepared based on extracted facts only, without guessing missing parameters. All sensitive data anonymized.*