# ICAO MRTD Personalization Complex / Комплекс персонализации ICAO MRTD

## 🇷🇺 Русский / Russian

### О проекте

Документация аппаратно-программного комплекса персонализации пластиковых карт и документов с machine-readable zone (MRZ). Комплекс предназначен для персонализации пластиковых карт, включая управление лазерным оборудованием, проверку шрифтов MRZ и взаимодействие с печатными устройствами.

### Навигация по документации

| Документ | Описание |
|---|---|
| [SPECIFICATION.md](SPECIFICATION.md) | Архитектурное и пользовательское описание комплекса |
| [ARCHITECTURE_DIAGRAMS.md](ARCHITECTURE_DIAGRAMS.md) | Технические диаграммы системы (Mermaid) |

### Основные компоненты

- Интерфейс пользователя / UI
- Менеджер очереди заданий / Job Queue Manager
- Модуль графической персонализации / Graphic Personalization
- Модуль электронной персонализации / Electronic Personalization (PecLib)
- Лазер YLPM / Laser YLPM (RS-232C)
- Транспорт CIM 2000 / Transport CIM 2000
- Принтеры Evolis / HDP600 / Card Printers Evolis/HDP600
- ABBYY Passport Reader SDK

---

## 🇬🇧 English

### About the Project

Documentation for the ICAO MRTD plastic card and document personalization complex with machine-readable zone (MRZ). The complex is designed for plastic card personalization, including laser equipment control, MRZ font verification, and interaction with printing devices.

### Documentation Navigation

| Document | Description |
|---|---|
| [SPECIFICATION.md](SPECIFICATION.md) | Architectural and user specification of the complex |
| [ARCHITECTURE_DIAGRAMS.md](ARCHITECTURE_DIAGRAMS.md) | System technical diagrams (Mermaid) |

### Main Components

- User Interface / UI
- Job Queue Manager
- Graphic Personalization Module
- Electronic Personalization Module (PecLib)
- YLPM Laser (RS-232C)
- Transport CIM 2000
- Card Printers Evolis/HDP600
- ABBYY Passport Reader SDK

---

*Документ подготовлен на основе извлечённых фактов без домыслов отсутствующих параметров. Все закрытые данные анонимизированы.*

*Document prepared based on extracted facts only, without guessing missing parameters. All sensitive data anonymized.*