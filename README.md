### 1. Checking whether an event exists

Before creating a reservation, the API uses the event ID to search the database with `session.get(Event, event_id)`. If no event is found, it raises a `404 Not Found` error with the message `"Event not found"`. This ensures that every reservation is linked to an existing event.

### 2. Calculating booked and remaining seats

The API retrieves all reservations belonging to the event and counts them using `len(reservations)`. The booked seats are the number of existing reservations. Remaining seats are calculated using:

`remaining = event.capacity - booked`

### 3. Preventing overbooking

Before creating a reservation, the API compares the number of booked seats with the event capacity. If `booked >= event.capacity`, it rejects the reservation with a `400 Bad Request` error and the message `"Event is full"`. Therefore, reservations cannot exceed the defined capacity.

### 4. Preventing reservations for closed events

The API checks the event's `status` before creating a reservation. If the status is not `"Open"`, the request is rejected with a `400 Bad Request` error and the message `"Event is closed"`. This prevents students from reserving seats in closed events.

### 5. Using SQLModel Session

`Session(engine)` is used to communicate with the SQLite database. It is used to retrieve events and reservations, add new records, update records, and delete records. Changes are saved using `session.commit()`, while `session.refresh()` retrieves the updated database record after insertion or modification.
