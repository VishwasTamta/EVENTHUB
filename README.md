# EventHub API

A Django REST Framework backend API for a simplified event ticketing platform where users can browse events, reserve seats, cancel reservations, and filter events and reservations.

---

## Table of Contents

- [Overview](#overview)
- [Tech Stack](#tech-stack)
- [Project Structure](#project-structure)
- [Installation](#installation)
- [Running the Project](#running-the-project)
- [API Endpoints](#api-endpoints)
  - [Event Endpoints](#event-endpoints)
  - [Reservation Endpoints](#reservation-endpoints)
- [Features](#features)
- [Design Decision](#design-decision)
- [API Testing Screenshots](#api-testing-screenshots)

---

# Overview

EventHub is a REST API built using Django and Django REST Framework. It allows users to:

- Browse events
- Reserve seats
- Cancel reservations
- Filter events by status and venue
- Filter reservations by event
- Prevent overbooking using serializer validation

---

# Tech Stack

- Python
- Django
- Django REST Framework (DRF)
- SQLite
- uv (Package Manager)

---

# Project Structure

```
eventhub/
│
├── eventhub/
│   ├── settings.py
│   └── urls.py
│
├── events/
│   ├── models.py
│   ├── serializers.py
│   ├── views.py
│   ├── urls.py
│   ├── middleware.py
│   └── migrations/
│
├── screenshots/
├── manage.py
├── requirements.txt
├── pyproject.toml
├── uv.lock
└── README.md
```

---

# Installation

## 1. Clone the repository

```bash
git clone https://github.com/VishwasTamta/EVENTHUB.git
cd eventhub
```

## 2. Create a virtual environment

```bash
uv venv
```

Activate it.

### Windows (PowerShell)

```powershell
.venv\Scripts\Activate.ps1
```

### Windows (Command Prompt)

```cmd
.venv\Scripts\activate
```

### macOS / Linux

```bash
source .venv/bin/activate
```

## 3. Install dependencies

```bash
uv sync
```

or

```bash
pip install -r requirements.txt
```

## 4. Apply migrations

```bash
uv run python manage.py migrate
```

---

# Running the Project

```bash
uv run python manage.py runserver
```

The API will be available at

```
http://127.0.0.1:8000/api/
```

---

# API Endpoints

## Event Endpoints

| Method | Endpoint | Description |
|---------|----------|-------------|
| GET | `/api/events/` | List all events |
| POST | `/api/events/` | Create an event |
| GET | `/api/events/{id}/` | Retrieve a single event |
| PUT | `/api/events/{id}/` | Update an event |
| DELETE | `/api/events/{id}/` | Delete an event |
| GET | `/api/events/?status=upcoming` | Filter events by status |
| GET | `/api/events/?venue=Bangalore` | Filter events by venue |

---

## Reservation Endpoints

| Method | Endpoint | Description |
|---------|----------|-------------|
| GET | `/api/reservations/` | List all reservations |
| POST | `/api/reservations/` | Create a reservation |
| GET | `/api/reservations/{id}/` | Retrieve a reservation |
| GET | `/api/reservations/?event_id=1` | Filter reservations by event |
| POST | `/api/reservations/{id}/cancel/` | Cancel a reservation |
| PUT | `/api/reservations/{id}/` | Update a reservation |
| DELETE | `/api/reservations/{id}/` | Delete a reservation |

---

# Features

- CRUD operations for Events
- CRUD operations for Reservations
- Event filtering by status
- Event filtering by venue
- Reservation filtering by event
- Automatic seat deduction on successful reservation
- Automatic seat restoration on reservation cancellation
- Overbooking prevention through serializer validation
- Restricts reservations for completed and cancelled events
- Request logging middleware
- Atomic database transaction for reservation creation

---

# Design Decision

The reservation creation logic is implemented inside the serializer's `create()` method, where the event's available seats are updated and the reservation is created together within a `transaction.atomic()` block. This ensures both operations are executed as a single database transaction. If any step fails, the transaction is rolled back, preventing partial updates and maintaining data consistency.

---

# API Testing Screenshots

## Successful Reservation (201 Created)

Demonstrates successful reservation creation and seat deduction.

![Successful Reservation](screenshots/successful_reservation.png)

---

## Overbooking Attempt (400 Bad Request)

Demonstrates validation that prevents reserving more seats than are available.

![Overbooking Failure](screenshots/overbooking_failure.png)

---

## Successful Reservation Cancellation

Demonstrates cancelling a reservation and restoring the reserved seats to the event.

![Successful Cancellation](screenshots/successful_cancellation.png)